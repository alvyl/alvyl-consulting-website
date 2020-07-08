---
title: "Tame your VM's Daemon Threads. #TrueStory"
date: 2020-07-08T23:22:34+05:30
author : Hari Krishna Ganji
tags : ["devops"]
imageUrl: /images/hari.png
---

![Java VM Blocked Threads spike on custom Grafana Dashboard](/images/blog/grafana-jvm-threads.png)

> With Great Power comes Great Responsibility

### Prologue - The Force Awakens

> And I knew, I had to Tame em' Daemon Processes.

I remember the feeling when I implemented my first parallel processing code in Perl. It was pure power.

This was back in 2007 when I was Interning at Synopsys's Software Engineering R&D Team. I was still doing my last semester of Bachelor's degree.

We were building a test coverage tool from scratch using cool code instrumentation support provided by the GNU GCC compiler. It was some seriously hardcore stuff. I was a Unix/Linux fanboy, and it was like a dream come true.

[Even after all these years, I'm still proud of it. Not many projects have been close to the challenge.]

After implementing the batch Perl jobs that run the code coverage tooling on some 20GB code base, I picked on some suggestions from my mentor.

It started with 2 parallel child process forks. And the processing time was cut by half. My mentor and I were pleasantly surprised.

And soon, I was flaunting a 10X improvement in speed. Jobs that took hours were reduced to minutes.

And then at some point, I hit the limit. Beyond this the servers became unresponsive.

And I knew, I had to Tame em' Daemon Processes.

. . .

### Episode 1: The Phantom Menace - Losing Sleep

> A problem that wasn't.

Fast forward to recent times.

One of our client's production server was stalling.

We soon realized a pattern. It was happening every Monday and Tuesday. In the beginning, we thought it was the database.

Scaling the database was long due. So, we upgraded the database to 2X size. And it costed 4X because of replication. Things were good and the 4X cost was acceptable.

And then it happened again after 6 months. Our data was scaling fast. We decided to upgrade the database again to 4X the original size. And it cost us 8X the original cost.

Click of a button. And, the upgrade had fixed it and we got to sleep well.

But then, it happened again after 3 months. And we knew it was time. This can't go on.

It was a problem that wasn't.

. . .

### Episode 2: Attack of the Clones - The Forensics.

> Indeed, a picture is worth a 1000 log entries.
> 
> Seeing was believing.

It was time to handle this.

Our options were to

- Upgrade the Database. (Monetarily Expensive)
- Partition the Database. (Temporally Expensive)
- Tune the Database and Queries. (Obvious next step)
  
But then, before beginning we did something right.

I always believed strongly in the philosophy of - "If you can't measure it, you can't fix it".

We implemented Prometheus [(https://prometheus.io/)](https://prometheus.io/) based monitoring across our production infrastructure to gather data.

We started gathering valuable information on the following:

- Node metrics (CPU, RAM, Disk, and Network). 
- Java JVM metrics (Heap Memory, Threads, Garbage Collection).
- Tomcat metrics (Requests, Errors, Response Times, Threads).
- Database metrics (Connections, and Table Sizes).

We built a custom Grafana [(https://grafana.com/)](https://grafana.com/) dashboard, that could correlate data points across multiple systems.

And we found it, the Spike in the Graph. Every "Node Down" event had a correlated spike in "JVM Blocked Threads". And it meant something.

All this time we were looking at Tomcat health and it all looked sane, with a healthy Tomcat thread and memory count. The custom dashboard was golden.

It was the JVM Threads. We had parallel Deamon Threads spawning code in our Web App. Blocked threads pileup was the cause for the stalling our Server when the Database was under load. The database was kind of innocent. 🤦‍♂️

A picture is indeed worth a thousand textual log entries. Seeing was believing.

. . .

### Episode 3: Return of the Jedi - Bring balance.

> There was no Thread Rate Limiting or Thread Pooling or Thread Timeouts.
> Basically, it was fire and forget.

Equipped with the data and knowing that the JVM threads were spiking when the Database became slow, we started looking into the Thread spawning code.

We exported a thread dump from the production JVM. We had at a moment of crash 1000s of Threads marked as "java.lang.Thread.State: BLOCKED (on object monitor)" state.

We were consuming messages from a cloud-based Queue service and were processing the pushed messages by spawning Threads. These threads were piling up

There was no Thread rate limiting or Thread pooling or Thread Timeouts. Basically, it was fire and forget. However, it worked for a long time.

The solution was to :

**Indirect:**

1. Tune the tomcat threads and don't accept requests, beyond a limit of active threads. (Less blocked tomcat threads, and less load on Database Connection Pool)
2. Tune long-running Database queries.

**Direct:**

3. Add a Java Thread Pool. Implement a Leaky Bucket protocol. If the Threads exceed a fixed number, do not accept any more messages from the queue. 
4. Add a Connection Timeout on Thread execution time. 
5. Limit the surface area of synchronized thread execution, if any.

This taming of Threads brought the system under control. A slow database would soon recover when the load went down. And the Blocked message processing Threads would no longer pile up and stall the Server.

. . .

### Episode 4: A New Hope - Know your Daemons.

> A young apprentice must ask these questions to self if he wishes to master robust multi-threading.

A young apprentice must ask these questions to self if he wishes to master robust multi-threading.

**Know your Daemons:**

- Does your platform support Threads?
- Native Threads or Processes emulated as Threads?
- Does the Processor support Multi-Threading?
- Are your threads/processes utilizing all the Processor Cores?
- What is the memory overhead?
  
**Know your Thread's/Process's nature of Blocking IO:**

- Are they waiting for Disk/Network/Children/Events?
- Are they short lived or long running?
- Are they doing any database transactions?
- Are they doing locking of resources or tables?
- Is this Sync or Async IO?

**Know the triggers that create your Threads:**

- Who creates the Threads/Processes?
- Who manages the Threads?
- What events create these Threads? Are they created on a message arrival? Are they created on schedule?
- How many Threads are created? 
- How long do they run?
- How may are destroyed?
- How many Threads could end up in Blocked state at any given moment?

**Know your Thread Pooling options:**

- Do you a need a Thread Pool?
- Does the platform support Thread Pools?
- Are there any off-the-shelf Thread Pool options?
- What's the size of Thread Pool?
- How do you tune the Thread Pool based on underlying Hardware?
- Do you also need a Connection Pool too?

**Have robust monitoring in place:**

- How can you monitor your threads?
- How can you know how many of them are blocked?
- What are the individual and net throughput of your Threads?
- How can you add alerting (email) to know spikes in your thread counts?
- How do you know the memory consumption of your Threads?
Graceful degradation:
- How does your Application/System behave under load? 
- How does your Application/System behave when the load is above the capacity?
- Is there a timeout, I can set?
- Can I circuit break my System from stalling the entire VM/Server and Other Applications?

. . .

### Epilogue: With Great Power Come Great Responsibility

> If you can, always prefer a proven and tested threading library, rather than writing threading and pooling abilities your self.

It's better to avoid writing threads if you don't understand them well and allow the underlying library/platform/VM/server to do the job for you.

If you can, always prefer a proven and tested threading library, rather than writing threading and pooling abilities your self.

Always attach logging and monitoring abilities to your Threading infrastructure, so that you know whats going on.

Threads are awesome, but if you don't know how to tame them, they could prove to b expensive. They bring in a lot of complexities with them.

May the force be with you!

~ On Software Design

#MultiThreading #Programming #SoftwareEngineering #SoftwareDesign #AlvylConsulting

***Post Script:***

*Need a monitoring stack? I can build it for you.*

*Already have a monitoring stack? I can fix it for you.*

*Talk to me!*

. . .

As posted to [Medium on Aug 25, 2018](https://medium.com/alvyl-consulting/tame-your-vms-daemon-threads-truestory-f63592cf2512).