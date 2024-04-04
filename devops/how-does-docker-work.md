---
id: how-does-docker-work
aliases: []
tags:
  - Docker
  - DevOps
date: 2024-02-08-0107 (February 8, 2024 at 1:07 AM)
title: How does Docker work?
---

# How does Docker work? 

- This [diagram](/devops/how-does-docker-work-diagram.webp) shows the architecture of Docker and how it works when we run “docker build”, “docker pull” and “docker run”. 
 
## There are 3 components in Docker architecture: 
 
1. Docker client 
- The docker client talks to the Docker daemon. 
 
2. Docker host 
- The Docker daemon listens for Docker API requests and manages Docker objects such as images, containers, networks, and volumes. 
 
3. Docker registry 
- A Docker registry stores Docker images. Docker Hub is a public registry that anyone can use. 
 
## Example
- Let’s take the “docker run” command as an example. 
1. Docker pulls the image from the registry. 
2. Docker creates a new container. 
3. Docker allocates a read-write filesystem to the container. 
4. Docker creates a network interface to connect the container to the default network. 
5. Docker starts the container. 
