---
title: "Docker in Devops"
date: 2020-06-26
description: ""
draft: true
author: "Darshan A M"
imageUrl: "/images/darshan.png"
---

**Docker** is a set of platform as a service (PaaS) products that uses OS-level virtualization to deliver software in packages called containers.Containers are isolated from one another and bundle their own software, libraries and configuration files; they can communicate with each other through well-defined channels.

# Docker architecture

![Docker](/images/docker-architecture.png)


## Docker File
```
FROM ubuntu:18.04
COPY . /app
RUN make /app
CMD python /app/app.py
```
## Each instruction creates one layer:

- **FROM** creates a layer from the ubuntu:18.04 Docker image.
- **COPY** adds files from your Docker - client’s current directory.
- **RUN** builds your application with make.
- **CMD** specifies what command to run within the container.

## Build the docker image and run the container
```
docker build .
```
```
Docker run -d --name <containername> -p <localport>:<hostport> <imagename>
```

## My personal experiance on docker

- Basic undurstanding of Computer science is enough to start a docker
- Eaasy to install and easy to use.
- Docker made developer to setup the local environmet easy.
- Increase the development speed.