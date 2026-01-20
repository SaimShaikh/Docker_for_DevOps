# Day 4 Docker

---

## Q1 What is Docker

Docker is a containerization platform used to package applications with their dependencies into containers.  
It ensures applications run consistently across different environments.

---

## Q2 What are the key components of Docker

The key components of Docker are Docker Engine, Docker images, Docker containers, Dockerfile, Docker registry, and Docker CLI.  

Docker Engine manages containers, images define the application, containers run the application, Dockerfile builds images, and registries store images.

---

## Q3 What is Docker Hub

Docker Hub is a cloud-based registry used to store, share, and download Docker images.

---

## Q4 What is Docker Image

Docker image is a read-only template that contains an application and its dependencies.  
It is used to create containers.

---

## Q5 How is a Docker container different from a virtual machine (VM)

A virtual machine runs a full operating system and uses more resources, while a Docker container shares the host OS and is lightweight.  

Containers start faster, use fewer resources, and are better suited for modern DevOps and microservices architectures.

---

## Q6 How do you create and run a Docker image for a Java application

To create and run a Docker image for a Java application, I first write the Java code and compile it.  

Then I create a Dockerfile using a Java base image, copy the application into it, and define the run command.  

After building the image using docker build, I run it as a container using docker run.

---

## Q7 What is Docker container

A Docker container is a running instance of a Docker image.

---

## Q8 Can you tell what is the functionality of a hypervisor?

A hypervisor is software that enables multiple virtual machines to run on a single physical system.  

It manages hardware resources, ensures isolation between VMs, and controls their creation and execution.  

Hypervisors are the foundation of virtualization and cloud computing.

Hypervisors are of 2 types:

### 1. Native Hypervisor
This type is also called a Bare-metal Hypervisor and runs directly on the underlying host system which also ensures direct access to the host hardware which is why it does not require base OS.

### 2. Hosted Hypervisor
This type makes use of the underlying host operating system which has the existing OS installed.

---

## Q9 Can you tell something about Docker namespace?

Docker namespaces are used to isolate containers by giving them separate views of system resources such as processes, network, and file systems.

---

## Q10 CMD vs ENTRYPOINT

CMD defines the default command or arguments for a container and can be overridden at runtime.  

ENTRYPOINT defines the main command that always runs when the container starts.

---

## Q11 What is port mapping

Port mapping is used to map a host machine port to a container port so that applications running inside Docker containers can be accessed from outside.  

It allows external users or browsers to communicate with containerized applications.

---

## Q12 Can you describe a situation where you used Docker to solve a specific problem?

Yes. In a recent project, we faced inconsistency issues between development and production environments, leading to deployment errors.  

To solve this, we adopted Docker. And deployed the application with Kubernetes.

---

## Q13 You're in charge of maintaining Docker environments in your company and you've noticed many stopped containers and unused networks taking up space. Describe how you would clean up these resources effectively to optimize the Docker environment.

The docker prune command is used to clean up unused Docker resources, such as containers, volumes, networks, and images.  

It helps reclaim disk space and tidy up the Docker environment by removing objects that are not in use.

There are different types of docker prune commands:

- docker container prune : Removes stopped containers  
- docker volume prune : Deletes unused volumes  
- docker network prune : Cleans up unused networks  
- docker image prune : Removes unused images  

Running docker system prune combines these functionalities into one command, ensuring that Docker removes any resources not associated with a running container.

---

## Q14 You're working on a project that requires Docker containers to persistently store data. How do you handle persistent storage in Docker?

To handle persistent storage in Docker, I use Docker volumes or bind mounts.  

Docker volumes are preferred in production because they are managed by Docker and persist data even after containers are stopped or removed.

---

## Q15 What is Docker Swarm

Docker Swarm is a container orchestration tool provided by Docker that allows users to manage and scale containers across multiple machines.  

It provides clustering, load balancing, and high availability for Docker containers.

---

## Q16 You're managing a Docker environment and need to ensure that each container operates within defined CPU and memory limits. How do you limit the CPU and memory usage of a Docker container?

Docker allows you to limit the CPU and memory usage of a container using resource constraints.  

You can set the CPU limit with the --cpu option and the memory limit with the --memory option when running the container using the docker run command.  

For example:  
docker run --cpu 2 --memory 1g mycontainer limits the container to use a maximum of 2 CPU cores and 1GB of memory.

---

## Q17 You've been tasked with ensuring the application can handle increased loads by scaling Docker containers horizontally. How do you scale Docker containers horizontally?

To scale Docker containers horizontally, you can use Docker Swarm or a container orchestration tool like Kubernetes.  

Which lets you create and manage multiple docker containers defining the number of replicas in declarative way. (Manifests)

---

## Q18 How do you create a multi-stage build in Docker?

Multi-stage builds in Docker allow you to create optimized Docker images by leveraging multiple build stages.  

To create a multi-stage build, you define multiple FROM instructions in the Dockerfile, each representing a different build stage.  

Each stage can use a different base image and perform specific build steps.  

Intermediate build artifacts can be copied between stages using the COPY --from instruction.  

This technique helps reduce the image size by excluding build tools and dependencies from the final image.

---

## Q19 What is Docker network and its types

Docker networking enables communication between containers and external systems.

Docker supports different network types such as:

### 1. bridge (default)

What it is:  
A private internal network on a single Docker host. When you docker run without --network, containers join the default bridge.

Best for:  
Simple single-host setups, dev/test.

Simple explain:  
Like a small office LAN inside your machine. Containers can talk to each other but not to external machines unless you map ports.

Example:  
docker network create shop-net  
docker run --network shop-net --name catalog ...

---

### 2. host

What it is:  
Containers share the host's network stack (no network isolation). No NAT, they use host IP addresses.

Best for:  
Low-latency apps, performance-sensitive workloads, or when you need to access host network services directly.

Simple explain:  
Container sits directly on the host's NIC — it’s like the container is a process on the host as far as networking goes.

Caveat:  
No port isolation — use carefully.

---

### 3. none

What it is:  
Container has no network interfaces except loopback.

Best for:  
Ultra-isolation, specialized containers that don't need network (e.g., build tasks), or when you plan to manually configure networking.

Simple explain:  
The container is in a room with the door locked. No talking to anyone.

---

### 4. overlay

What it is:  
Multi-host networking driver used by Docker Swarm (also used by some orchestration tools).  

It creates a virtual network that spans multiple Docker hosts.

Best for:  
Services deployed across multiple hosts (swarm mode) that need to communicate securely.

Simple explain:  
A VPN between hosts that lets containers on different machines act like they are on the same LAN.

Notes:  
Requires a key/value store (older Docker) or built-in swarm control plane.

---

### 5. macvlan

What it is:  
Assigns a MAC address to a container and allows it to appear as a physical device on the network.

Best for:  
Integrating containers into an existing physical network, when you need unique MAC/IP and direct L2 access.

Simple explain:  
The container gets its own presence on your physical network switch — good when devices need to be directly reachable by other network machines.

---

### 6. ipvlan

What it is:  
Similar to macvlan but with different L2/L3 behavior; often used to conserve MAC addresses or when certain switch features are restrictive.

Best for:  
Advanced use cases where you need IP-level separation and better performance.

---

### 7. Third-party plugins (Weave Net, Calico, Cilium, Flannel)

What they are:  
Plugins that add advanced networking, security policies, or overlay alternatives — often used in Kubernetes or advanced Docker setups.

Best for:  
Fine-grained network policy, observability, or large-scale Kubernetes clusters.

---

## Q20 You're responsible for securing the Docker containers hosting your organization's sensitive applications. How do you secure Docker containers?

Securing Docker containers involves implementing a multi-layered approach.  

Some recommended practices include:

- Using trusted base images from reputable sources  
- Regularly updating Docker and the underlying host system with security patches  
- Scanning container images for vulnerabilities using security tools like docker scout  
- Implementing network segmentation and access controls  
- Monitoring container activity and logging for security analysis  
- Applying the least privilege principle  
- Implementing container-specific security measures like AppArmor or seccomp profiles  

---

## Q21 Have you ever used Docker in combination with a CI/CD system?

Yes, in nearly all my projects.  

For example, in one project, we integrated Docker with Jenkins.  

In the build stage, Jenkins would trigger a job that builds a new Docker image and pushes it to a Docker registry.  

During the deployment stage, this image was pulled from the registry and deployed to the production environment using Kubernetes.  

This approach ensured uniformity across all stages of the deployment process.

---

## Q22 How is Docker used within AWS?

In my previous project, we utilized Docker within AWS using Amazon's Elastic Container Service (ECS).  

We deployed our microservices as Docker containers on ECS, which enabled us to efficiently manage the lifecycle of these services.  

We also used AWS Fargate for serverless compute for our containers.

---

## Q23 How do you monitor Docker containers?

There are various ways to monitor Docker containers, including:

- Using Docker's built-in container monitoring commands such as docker stats and docker container stats  
- Integrating with monitoring and logging tools like Prometheus, Grafana, or ELK stack (Elasticsearch, Logstash, Kibana)  
- Leveraging container orchestration platforms that offer built-in monitoring capabilities such as Docker Swarm's service metrics or Kubernetes' metrics API  
- Using specialized monitoring agents or tools that provide container-level insights and integration with broader monitoring and alerting systems  

---

## What are some best practices for using Docker in production environments?

Some best practices for using Docker in production environments include:

- Building and using lightweight container images to improve deployment speed and reduce the attack surface  
- Regularly updating Docker and the underlying host system with security patches  
- Implementing container orchestration platforms such as Docker Swarm or Kubernetes  
- Configuring resource limits for containers  
- Monitoring container health, resource usage, and application metrics  
- Implementing secure network configurations  
- Backing up critical data and using persistent storage solutions  
- Implementing a comprehensive security strategy including vulnerability scanning, access controls, and least privilege principles  

Docker Interview Questions 16 Implementing a comprehensive security strategy, including container vulnerability scanning, access controls, and least privilege principles

---

## Q24 What is ARG and ENV in Docker

ARG is used to pass variables at build time while creating a Docker image and is not accessible at runtime.  

ENV is used to define environment variables that are available inside the running container and used by the application.

🚫 If Interviewer Pushes More (Add This One Line)

ARG is mostly used for build configuration, while ENV is used for application configuration.

---
