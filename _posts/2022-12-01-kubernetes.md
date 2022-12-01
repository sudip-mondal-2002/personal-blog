---
layout: post
title:  "Kubernetes: A way to play with microservices"
date:   2022-12-01
excerpt: "Kubernetes is at the cutting-edge of application deployment. The best way to kick-start your DevOps career is by learning how to effectively deploy Kubernetes.   "
---

Kubernetes is a container orchestration system that allows organizations to deploy and manage containers on Kubernetes clusters. 
The Kubernetes platform provides an open source framework for automating deployment, scaling, and management of applications using containerized runtimes.
Kubernetes is an open-source system for automating deployment, scaling, and management of containerized applications. 
It was originally designed by Google and is now maintained by the Cloud Native Computing Foundation.

![k8s](/assets/images/kubernetes.png)

## Need of Kubernetes in the art of Microservices
You need to know that microservices are a new architectural style of breaking down a service into multiple small ones. 
They’re like Lego bricks: you can assemble them in many different ways and give them different shapes.

<img align="right" src ="/assets/images/microservices-vs-monolithic.png" />
Microservice architecture is an approach to developing software that breaks down large applications into smaller pieces called services, 
each of which is responsible for performing just one function (e.g., connecting users with data). 
This allows organizations to scale their applications independently, making it easier for them to maintain and update their code when necessary—and also easier for developers who aren't necessarily experts on how everything works together at once!

The main benefit of microservices architecture is increased flexibility. 
By breaking down your application into smaller services, each one can be updated independently of the others. 
So if you need to make a change that affects just one service, it won’t affect any other connected components—and this makes it much easier to maintain and update your code! 
This also means that if one component fails or isn't working as expected, it doesn't bring down the entire application. 
You can also scale each service independently, so if you need to increase performance or add a new feature on one of them, it won’t have an impact on any other components.

## Setup Kubernetes
There are several ways you can setup kubernetes. However I recommend to install Docker desktop for Mac and Windows users and for linux users minikube is recommended.
However these provide you kubernetes clusters in which you can develop Kubernetes based application. These clusters are resource intensive, so you may wish to avoid these if you have a low spec computer and leverage the cloud.
In that case install kubectl. To get broader knowledge about how to use them, please head over to their respective websites.

## Components of Kubernetes:
There are certain concepts that you should know about kubernetes. I'm talking about a few but there are a lot more than those:
  - Pod: Pods are the smallest, most basic deployable objects in Kubernetes. A Pod represents a single instance of a running process in your cluster. Pods contain one or more linux containers, such as Docker containers. When a Pod runs multiple containers, the containers are managed as a single entity and share the Pod's resources.
  - Deployment: A Kubernetes Deployment tells Kubernetes how to create or modify instances of the pods that hold a containerized application. Deployments can help to efficiently scale the number of replica pods, enable the rollout of updated code in a controlled manner, or roll back to an earlier deployment version if necessary.
  - Replicaset: A ReplicaSet (RS) is a Kubernetes object used to maintain a stable set of replicated pods running within a cluster at any given time. A Kubernetes pod is a cluster deployment unit that typically contains one or more containers. Pods (and, by extension, containers) are, nevertheless, short-lived entities.

## Networking in Kubernetes
Kubernetes Services (or kubelets) is a process that runs in the pod and provides network services to pods. 
It's responsible for managing incoming connections and outgoing connections, so it can manage ingress rules as well as load balancing between services based on current load.

Kubernetes provides several services, each of which can be used to help manage your containers. These include:
 - LoadBalancing: This allows you to load-balance across multiple instances or ports for a service. 
   It’s useful if you have multiple applications running on different hosts and want them to connect together via their ports.
 - Ingress: This works similarly to ELB in AWS (Elastic Load Balancing), but instead of routing traffic from outside servers into your application container, 
   it routes traffic from external hosts into the container itself, allowing easy access by other applications on the same host/IPs as your main application container itself

This is useful if you have multiple applications running on different hosts and want them to connect together via their ports. 
This allows you to access your application using the hostname instead of needing to set up a load balancer externally.

## Deployment in Microservice
Deploying a Microservice Application in Kubernetes is creating an application that can be scaled and managed in Kubernetes. 
A usual way to deploy microservices is while coding the services, we write dockerfile for the production. Build the docker images and run them inside kubernetes containers.
Kubernetes containers are configured using YAML file. YAML files are simple key value pairs like JSON or XML.
Well, In this article, I am not explaining how to deploy microservices using k8s tooling on Kubernetes cluster, but, you may check out a simple microservice based project I coded [here](https://github.com/sudip-mondal-2002/ticketing).

## Autoscaling
Autoscaling is one of the key features in Kubernetes cluster. 
It is a feature in which the cluster is capable of increasing the number of nodes as the demand for service response increases and decrease the number of nodes as the requirement decreases.

![k8s-microservices](/assets/images/kubernetes-microservices.png)

The primary strength of Kubernetes is its modularity and generality. Nearly every kind of application that you might want to deploy you can fit within Kubernetes, and no matter what kind of adjustments or tuning you need to make to your system, they're generally possible.
