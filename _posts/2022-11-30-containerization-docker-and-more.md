---
layout: post
title:  "Containerization: Docker and More"
date:   2022-11-30
excerpt: "Docker is one of the most popular tools for application containerization. Docker enables efficiency and reduces operational overheads so that any developer, in any dev environment, can build stable and reliable applications."
---

Docker is an open-source platform for building, shipping and running distributed applications. 
It allows you to run an application in a lightweight container that includes everything needed to run your code,
including operating system libraries, kernel, and hardware drivers -- on any server with the Docker daemon.

![docker](/assets/images/docker.png)

## Developement before Containers

Before containers and the containerization of application development, developers had to use multiple virtual machines (VMs) or use personal computers as servers. 
This made it difficult for them to scale their infrastructure as needed. 
They also had trouble with maintaining software updates, since each VM was on its own physical server, which meant that if one VM got infected by malware or crashed, all other VMs could be compromised at once.

Developers now have access to tools like Docker that allow them to run applications in isolated environments without having control over everything else on those machines—for example:

- OS configuration (e.g., operating system version)
- Hardware drivers
- Software packages

With this new way of working comes a different set of challenges compared with what we've experienced before when deploying applications via traditional means such as virtual machines or physical servers.

## What is Containerization

Containerization is a lightweight alternative to virtual machines. 
It enables you to create isolated environments for applications and run them in the same way as they would be on physical machines, but with less overhead and with faster response times than virtual machines.

Containers provide portability between operating systems, enabling developers to use one container image across different platforms without having to create multiple images or update them every time they need to switch between systems. 
This also makes it easier for developers who might be working on multiple projects at once (or even multiple teams) because they can use the same base image across all their projects instead of having their own separate build environments.

Containers are lightweight because they don’t have their own operating system, but they can still run applications just like a VM would.

## Docker: What and Why

Docker is a software technology providing containers, promoted by the company Docker, Inc. 
It was originally developed by Solomon Hykes and Greg Kroah-Hartman at oDesk in 2010 as an open source project to simplify application deployment and allow developers to package their applications into lightweight "containers". 
Today it is used by many organizations around the world to deploy applications quickly while keeping them isolated from each other and their underlying infrastructure.

The first major release of Docker was v1.0 which introduced multi-stage builds for more effective builds/deployment process with support for Linux distros like Debian LTS or CentOS 7 (which are still supported today). 
The latest version of Docker CE is 17 beta 1 which includes new features such as runc runtime for container orchestration using LXC or cgroup controllers that allows you to manage individual workloads within a single namespace or across multiple namespaces based on resource limits set up during compilation time when creating images.

Docker is an open-source platform for building, shipping and running distributed applications. 
It makes it easy to create lightweight, portable and self-sufficient containers from any application. Using Docker containers you can get started with the power of Linux in minutes and scale your application up or down as your business needs change.

## How does docker work

Docker is a container management platform. Containers are isolated from each other, but share the same operating system and kernel.

Containers are created from images, which are built from Dockerfiles. 
Images can be shared and reused, or they can be built from scratch or based on existing images.
Dockerfiles are nothing but a description of a Docker container written in certain syntax.

## Images and Containers

Images and containers are the fundamental building blocks of Docker. 
An image is a template for creating instances of containerized applications, while a container contains the application itself and can run in any environment that supports running it.

Images are created from Dockerfiles, which are text files that contain instructions for creating an image from scratch or by copying another one (and not much else). 
They're written in plain text so that you can easily share them with others, but they do require some knowledge about how images work in order to write them correctly!

Images layer on top of each other—an image A has layers B through Z added onto it—so multiple images can be layered together into one large image if necessary. 
You'll see this concept referred as stacking later on in this guide when discussing how to build larger deployments using multiple images instead of just one big one or two smaller ones stacked together like pancakes; 
though we won't dive into specifics here because there's plenty more information available elsewhere online as well as plenty more coming soon...

![containerized](/assets/images/docker-containerized-app.png)

Docker is a revolutionary technology that simplifies isolation and provides environment independency in development, testing and production environments. 
It is solving a great problem that early developers faced and what caused a lot of inefficiency in the workspace just to debug things. Even when I wasn't aware of Docker, similar problems were faced by myself.

