# 

```
Docker capabilities  
- This can be used for finding the difference between containers and chef-client

Kubernetes capabilities  
- Intelligent Scheduling
- Self Healing
- Horizontal Scaling
- Service discovery & load balancing
- Automated rollouts and rollbacks
- Secret and configuration management

K8s out of box provides:
- Reliability
- Availability
- Scalability
- Performance
- Security

IBM added capabilities  
- Simplified Cluster Management
- Container Security & Isolation
- Design Your Own Cluster
- Leverages IBM Cloud and Watson
- Native Kubernetes Experience
- Integrated Operational Tools

WAMS added capabilities

- Take points from here - https://ibm.ent.box.com/notes/287884324304?s=38enzqhnc5njgy009hy6gyl8ztyl0kyw

Microservice Architecture:
(Required for interviews)

```

# 

```
A Container is a set of processes that are isolated with the rest of the Linux system and can be easily moved to other environments. In other words, they are completely portable. The same image that you build to run on your linux laptop can run on the cloud!

Containers bring many benefits. It allows application developers to increase application development efficiency by enabling continuous integration/continuous delivery (CICD). Containers can scale easily on the cloud. Since containers are completely portable, the same application developed on a cloud provider can be easily ported to another cloud provider. Containers are very light weight (compared to Virtual Machines - VMs). Multiple (disjoint) containers can be packed on a single host, and since they share the same OS kernel, containers facilitate efficient use of resources (CPU, memory, storage) available on the host machine. Moreover, containers can be provisioned very quickly which translates to high application availability for deployment and maintenance.
It is important to differentiate container from virtualization. In the virtualization environment, a single system can have multiple virtual operating systems running on it, and each virtual operating system has its supporting files and runtime environment. In containerization, all containers share the same operating system kernel; what are being isolated are the application processes.

A container needs the execution environment – the container runtime. As the name implies, a container runtime is the software that execute the container. Docker is the most popular container technology in the industry, but others have gained traction. Docker has many components; among those is the core runtime called containerd. Docker donated it to the open source community, and is now becoming popular. rkt is an alternative to Docker and is from CoreOS. It has a more Linux-like architecture than Docker. The discussion “docker vs containerd vs rkt” continues to be a popular topic in the container community.

Container Orchestration:
Why do we need to have container orchestration? Isn’t container alone with its benefits enough? While this argument may be sufficient for simple applications, it will fail when applications become complex. When you have many machines hosting your containers, you need a system to federate multiple hosts into one target. When your application is composed of many containers running on several hosts, you’ll need to have a mechanism to move containers around when a host is down. You’ll need ways for containers on different hosts to communicate with each other. You’ll need ways to make sure you can update your application with zero downtime. Container orchestrators also deploy/mange multiple replicates of the container, manage unhealthy containers, etc., All of these arguments are just a taste of why you need to have container orchestration.

There are many container orchestrators out there. Kubernetes is the most popular one. Its competitors include docker swarm, Cloud Foundry Diego, and Mesos.

Micro Services
Microservice is the new architecture paradigm. Its main idea is to decompose your application into many smaller components where each component has its own responsibilities and by definition, a microservice. Each microservice are loosely coupled from one another. They communicate using light weight protocol, can be distributed across different host machines and updated independently of one another.

[Picture taken from https://martinfowler.com/articles/microservices.html ]

From the diagram above, comparing the architectures, in a monolithic architecture, all components are run together in one processing unit and deployed as a single unit on a single host machine. This means, the functions of scaling, upgrades, patches, etc., are all performed together and in most cases the entire application will need to be rebuilt and redeployed to the cloud. This is in stark contrast to the microservice architecture wherein every component can be deployed, managed, scaled independently of each other. Containers are a natural deployment topology for microservices.
```