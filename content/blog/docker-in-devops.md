---
title: "Docker in Devops"
date: 2020-06-26
description: ""
author: "Darshan A M"
imageUrl: "/images/darshan.png"
---

**Docker** is a set of platform as a service (PaaS) products that uses OS-level virtualization to deliver software in packages called containers.Containers are isolated from one another and bundle their own software, libraries and configuration files; they can communicate with each other through well-defined channels. 

Docker can package an application and its dependencies in a virtual container that can run on any Linux server. This helps provide flexibility and portability enabling the application to be run in various locations, whether on-premises, in a public cloud, or in a private cloud.

# Docker architecture

Docker architecture. Docker uses a client-server architecture. The Docker client talks to the Docker daemon, which does the heavy lifting of building, running, and distributing your Docker containers. The Docker client and daemon can run on the same system, or you can connect a Docker client to a remote Docker daemon. The Docker client and daemon communicate using a REST API, over UNIX sockets or a network interface.

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

Build the docker image with image name and tag
```
docker build SOURCE_IMAGE[:TAG] .
```
Create a tag TARGET_IMAGE that refers to SOURCE_IMAGE
```
docker tag SOURCE_IMAGE[:TAG] TARGET_IMAGE[:TAG]
```
Push an image or a repository to a registry. Use docker push to share your images to the Docker Hub registry or to a self-hosted one(ex: AWS ECR, Azure container registry, etc). **Docker Hub** is a hosted repository service provided by Docker for finding and sharing container images with your team and the Docker community
```
docker push [OPTIONS] NAME[:TAG]
```
The docker run command first creates a writeable container layer over the specified image, and then starts it using the specified command. A stopped container can be restarted with all its previous changes intact using docker start. See docker ps -a to view a list of all containers
```
Docker run -d --name <containername> -p <localport>:<hostport> <imagename>
```
## Other usefull commads are
- ```docker images ``` to list the images
- ```docker top CONTAINER``` Display the running processes of a container.
- ``` docker cp [OPTIONS] SRC_PATH|- CONTAINER:DEST_PATH ``` Copy files/folders between a container and the local filesystem.
- ```docker exec [OPTIONS] CONTAINER COMMAND [ARG...] ``` Run a command in a running container
- ```docker inspect [OPTIONS] NAME|ID [NAME|ID...] ``` Return low-level information on Docker objects
- ``` docker rmi image-id ``` remove the image from the machine.
- ```docker stop container-id ``` to stop the container.
- ```docker rm container-id``` remove the container.

## Run your docker APP in AWS Fargate
AWS Fargate is a serverless compute engine for containers that works with both Amazon Elastic Container Service (ECS) and Amazon Elastic Kubernetes Service (EKS). Fargate makes it easy for you to focus on building your applications. Fargate removes the need to provision and manage servers, lets you specify and pay for resources per application, and improves security through application isolation by design

![Fargate](/images/aws-fargate-app.png)

## My personal experiance on docker

- Basic undurstanding of Computer science is enough to start a docker
- Eaasy to install and easy to use.
- Docker made developer to setup the local environmet easy.
- Increase the development speed.