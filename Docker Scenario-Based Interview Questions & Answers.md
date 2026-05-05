

---

## Suppose you need to create a Docker image for a Python web application. How would you go about it?

First, I make sure the Python application works properly outside Docker.  
Then I create a Dockerfile where I choose a lightweight Python base image, usually `python:3.x-slim`.

Inside the Dockerfile, I copy the application code, install dependencies using `requirements.txt`, expose the required port, and define the startup command using CMD or ENTRYPOINT.

After that, I build the image using `docker build` and test it locally by running a container.  
This ensures the app runs the same way in development, testing, and production.

---

## Describe a scenario where you would use Docker's bridge networking mode

Bridge networking is useful when multiple containers need to communicate with each other on the same host.

For example, if I have a backend API container and a database container running on one server, I use bridge networking so they can talk to each other internally while only exposing required ports to the outside world.

This keeps internal communication private and improves security while remaining simple to manage.

---

## How would you handle persistent storage for a Dockerized database application?

For databases, containers alone are not enough because container data is lost when the container stops.

I use Docker volumes to store database data outside the container.  
Volumes are managed by Docker and persist even if the container is deleted.

In production, volumes are always preferred over bind mounts because they are safer, portable, and easier to back up.



---

## Explain a scenario where Docker Compose would be beneficial for managing multi-container applications

Docker Compose is helpful when an application consists of multiple services.

For example, a web application might need a frontend, backend, database, and Redis cache.  
Using Docker Compose, I define all services in a single `docker-compose.yml` file and start everything with one command.

This makes local development, testing, and onboarding new team members much easier.

---

## Describe a scenario where you would use Docker Swarm for container orchestration

Docker Swarm is useful when I need simple container orchestration without Kubernetes complexity.

For example, if I want to deploy an application across multiple servers with load balancing and auto-recovery, Docker Swarm allows me to define services and replicas easily.

It’s a good choice for small to medium production setups.

---

## How would you secure a Docker container to prevent unauthorized access?

First, I use trusted and minimal base images.  
Then I avoid running containers as the root user.

I restrict network access using firewall rules and Docker networks, scan images for vulnerabilities, and apply least privilege permissions.

Security is layered — not dependent on a single setting.

---

## Explain how you would scale a Dockerized application to handle increased traffic

To handle more traffic, I scale containers horizontally.

This means running multiple container instances of the same application behind a load balancer.  
I can do this using Docker Swarm or Kubernetes by increasing the number of replicas.

This approach improves availability and performance without changing application code.

---

## Describe a scenario where you would use Docker monitoring tools to troubleshoot performance issues

If users report slow responses, I use monitoring tools like `docker stats`, Prometheus, and Grafana.

These tools help me identify high CPU usage, memory leaks, or network bottlenecks at the container level.

Monitoring gives visibility before problems become outages.

---

## Explain how Docker healthchecks can be used to ensure the reliability of containerized applications

Docker healthchecks allow me to define how Docker checks if a container is healthy.

For example, I can configure Docker to hit a `/health` endpoint periodically.  
If the healthcheck fails, Docker marks the container as unhealthy and restarts it automatically.

This improves reliability without manual intervention.

---

## How would you securely manage sensitive data such as passwords and API keys in Docker containers?

I never hardcode secrets in images.

Docker secrets allow you to
securely store and manage
sensitive information such as
passwords, API keys, and TLS
certificates, encrypted at rest
and in transit.
Secrets can be accessed by
up to
services running in Docker
Swarm and are only ava able t
authorized containers,
preventing unauthorized access
to sensitive data.


---

## Describe a scenario where you would use Docker's built-in load balancing features

In Docker Swarm, services automatically get load balancing.

For example, if I deploy a web service with multiple replicas, Swarm distributes incoming traffic across containers.

This improves fault tolerance and ensures no single container becomes a bottleneck.

---

## Explain how you would perform rolling updates without downtime

For rolling updates, I deploy new containers gradually while old containers are still running.

Docker Swarm replaces containers one by one instead of stopping everything at once.  
Healthchecks ensure traffic is routed only to healthy containers.

This allows zero-downtime deployments.

---

## How would you configure logging for Docker containers?

Docker container logs are stored on the host system, not inside the container itself. The default location depends on the operating system:

Linux: /var/lib/docker/containers/<container-id>/<container-id>-json.log
Each container has a dedicated log file in JSON format, containing timestamps, stream origin (stdout/stderr), and message content.
Windows: C:\ProgramData\docker\containers\<container-id>\<container-id>-json.log
Logs are stored in the Docker Desktop WSL2 VM, accessible via the path \\wsl.localhost\docker-desktop\mnt\docker-desktop-disk\data\docker\containers\....
macOS: Logs are stored within the Docker Desktop VM at ~/Library/Containers/com.docker.docker/Data/log/vm/. Access them using docker logs or directly via the VM.

To configure logging for Docker
containers, I would use Docker's
logging drivers to capture and
forward application logs to a
centralized logging system such as
Elasticsearch, Fluentd, or Splunk.

I would configure the logging driver
in the Docker daemon or Docker
Compose file to specify the
destination and format of the logs.

---

## Describe a scenario where you would use Docker security scanning tools

Before pushing images to production, I scan them using tools like Docker Scout or Trivy.

These tools detect vulnerabilities in OS packages and dependencies.  
This helps prevent deploying insecure images into production.

Security scanning is a standard CI/CD step.

---

## How would you implement backup and disaster recovery for Dockerized applications?

To implement backup and disaster
recovery strategies for Dockerized
applications, I would use a combination of
Docker volume backups, container image
registries, and cloud storage solutions.
I would regularly back up Docker volumes
containing persistent data using tools like
Docker Volume Backup or container-
native backup solutions.
Additionally, I would store container
images in a secure registry such as Docker
Hub or AWS ECR, enabling easy recovery
and redeployment in the event of a
disaster.
Cloud storage solutions like AWS S3 or
Azure Blob Storage can be used to store
backups offsite for added redundancy and
resilience.

---

## What is the difference between ADD and COPY in Docker?

COPY is used to just copy files and folders from your system into the Docker image.
It does only one job, so it’s safe and recommended for most cases.

ADD can also copy files, but it has extra features like extracting .tar files and downloading files from a URL.

---

### What is Docker Container Lifecycle?

Docker container lifecycle starts with creating an image, then creating and running a container.
A container can be paused, stopped, restarted, and finally removed.
Once removed, the container is permanently deleted and cannot be reused.

**Common Interview Trap (Tell This If Asked)**

- Stopping a container does NOT delete it

- Deleting a container does NOT delete the image

---

## What if Docker daemon is down? How do you solve it?

If Docker daemon is down, I first check the service status and restart it.
If it doesn’t start, I check daemon logs, disk space, system resources, and Docker configuration files.
Most issues are caused by disk full, config errors, or resource exhaustion.
As a last step, I reinstall Docker.


--- 

## Is it possible to use JSON instead of YAML for Docker Compose?

We can use JSON instead of YAML for a Docker Compose file. When we
are using the JSON file for composing, we have to specify the filename
with the following command:
``docker-compose -f docker-compose. json up``


---

## What is Immutable Infrastructure? How Docker Supports Immutable Infrastructure
Immutable infrastructure means: Once something is created and deployed, we do NOT change it.

Docker supports this by using immutable images and disposable containers.
Any change requires building a new image and redeploying containers, which improves consistency, reliability, and rollback.

---

## Running container as a root means ?
In Linux, root = UID 0.
Running a container as root means the main process inside the container runs with UID 0, giving it full permissions inside the container, which increases security risk if the container is compromised.

**you avoid root containers**
We avoid root containers by creating non-root users in the image, enforcing securityContext in Kubernetes, dropping capabilities, and avoiding privileged mounts.

**Use a non-root user in Dockerfile**
```bash
FROM ubuntu:22.04

# Create a non-root user
RUN useradd -m -u 10001 appuser

# Switch to non-root
USER appuser

CMD ["bash"]
```


---
## Can you delete a Docker image while a container is running from it?

No, you cannot delete a Docker image if a container is currently running from it.

Docker prevents this because the running container depends on that image. You must stop and remove the container first, and then you can delete the image.

---

## What is the difference between mutable and immutable in Docker?

In Docker, mutable means something can be changed after it is created, while immutable means it cannot be changed.

Docker images are immutable — once built, they don’t change. If updates are needed, a new image is created.
Containers, however, are mutable — you can modify them while they are running.

---

## Container starts but application not reachable. What will you check?

If a container is running but the application is not reachable, I follow a structured troubleshooting approach.

First, I check the container logs using docker logs to see if the application has any runtime errors or crashes.

Next, I verify the port mapping to ensure the container port is correctly exposed to the host, using docker ps or docker port.

Then, I go inside the container using docker exec and check whether the application is actually running and listening on the expected port using netstat or ss.

I also confirm that the application is bound to 0.0.0.0 and not 127.0.0.1, because binding to localhost will prevent external access.

After that, I check the network and firewall settings:

- Security Groups or firewall rules (in cloud)

---


## Container exits immediately after start. What could be wrong?

If a container exits immediately after starting, it usually means the main process inside the container has stopped.

First, I check the container logs using docker logs to identify any error messages or crashes.

The most common reasons are:

The application inside the container failed to start due to errors or missing dependencies
The CMD or ENTRYPOINT is incorrect or not defined properly
The process runs and exits immediately (for example, a script finishes execution)
The container is not running a long-running process (like a server)
Configuration issues such as wrong environment variables

I also verify the Dockerfile to ensure the correct command is used and that the application is designed to run in the foreground.


---

## Image build succeeds but app fails at runtime. Why?

Common causes
- Missing runtime dependencies: Build uses dev packages (like npm install with devDeps), but runtime lacks libraries, drivers, or Node modules.

- Environment variables: App expects DB_URL or API keys not set at runtime.

- Port binding or networking: App tries to listen on wrong port or can't reach DB/external services.

- Permissions/files: Wrong user (non-root), file paths, or volumes not mounted, causing "file not found" or access denied.

- Resources: OOM at runtime due to low memory limits, not hit during build.

Debug steps
- docker logs <container> for error like "Unable to access jarfile" or connection refused.

- docker run -it <image> /bin/sh to inspect files, env (env), and test the CMD manually.

- Compare build vs runtime: Check Dockerfile ENTRYPOINT/CMD, multi-stage build stripping dev tools.

---

## Port mapping configured but not working. What will you verify?

First, I check the port mapping configuration using docker ps to ensure the host port is correctly mapped to the container port.

Next, I confirm that the application inside the container is actually running and listening on the expected port by entering the container with docker exec and using netstat or ss.

Then, I verify that the application is bound to 0.0.0.0 and not 127.0.0.1, because binding to localhost will block external access.

I also check for port conflicts on the host machine to ensure the host port is not already in use.

After that, I validate firewall or security group rules (in case of cloud) to ensure the port is open.

If multiple containers are involved, I check the Docker network configuration to ensure proper connectivity.

---

## Logs are empty or missing. Where will you look?

If container logs are empty or missing, I don’t rely only on docker logs. I check multiple places to trace the issue.

First, I verify how the application is handling logs.
👉 If the app is writing logs to files inside the container instead of stdout/stderr, they won’t appear in docker logs. So I go inside the container using docker exec and check log files (like /var/log/... or app-specific paths).

Next, I check if the container is exiting too quickly, which might result in no logs being captured.

Then, I inspect the container using docker inspect to verify:

Logging driver configuration
Any misconfiguration in log settings

I also check the Docker daemon logs on the host system (like /var/log/docker.log or journalctl -u docker) in case the issue is at the engine level.

---

## Volume not persisting data. What will you troubleshoot? 

What to troubleshoot
- Wrong write path: App writes to container filesystem (e.g., /app/data) instead of mounted path (/data). Verify with docker exec ls -la /mount/path.

- Mount not applied: docker inspect <container> | grep Mounts — confirm volume/bind shows correct source:target. Wrong syntax in docker run/compose creates anonymous volume. 

- Bind mount override: Host dir empty or read-only; container init scripts populate non-mounted paths first.

- Volume type: Anonymous (dangling) vs named; recreate with docker volume rm and recreate properly. Check docker volume ls.

- Permissions: App runs as non-root, can't write to volume owned by root. Fix with USER in Dockerfile or chown.

---

## Container cannot reach another service. Why?

Exec into the source container and try ping <service-name> and curl service-name:port to test DNS and connectivity. Verify both on same network via docker network inspect. Check target service is running, listening on 0.0.0.0:port, and healthy. Rule out host firewall blocking docker bridge or startup race conditions with healthchecks/retries.

---

## Docker build cache causing issues. How will you handle it?

If Docker build cache is causing issues, it usually means old layers are being reused, so recent changes are not reflected in the image.

First, I confirm whether caching is the problem by rebuilding the image with:

- docker build --no-cache -t <image_name> .

If the issue is resolved, then it’s clearly a caching problem.

Next, I handle it properly instead of always disabling cache:

I reorder Dockerfile instructions so frequently changing steps (like copying source code) come later
I ensure that COPY or ADD commands are placed correctly, because Docker cache depends on file changes
I avoid unnecessary caching for dependency installation when required

If needed, I manually clean unused cache using:

- docker builder prune

I also make sure that:

Build context is correct
No stale files are being copied

---

## Image pull failing from registry. What could be the reason?

Common reasons
- Authentication: Not logged in to private registry (docker login), expired token, or wrong creds. Public like Docker Hub rate-limited.

- Network/Connectivity: TLS handshake timeout, proxy/VPN/firewall blocking registry.docker.io, DNS issues, or IPv6 problems.

- Image not found: Typo in repo/tag (e.g., ngnix vs nginx), 404, or doesn't exist. Verify on hub.docker.com.

- Disk/Storage full: Local disk out of space during pull, causing verification fail.

- Registry-specific: ECR needs aws ecr get-login-password, disk full on host for layered pulls.

---

## Wrong environment variables inside container. How will you verify?

Verification methods
Inspect stopped/any container/image: docker inspect <container/image> --format '{{range .Config.Env}}{{.}}\n{{end}}' or docker inspect <name> | jq '.[].Config.Env'. Shows all set vars. 

Exec running container: docker exec <container> env or docker exec <container> printenv VAR_NAME. Real runtime values.

K8s: kubectl exec <pod> -- env | grep VAR or kubectl describe pod for env sources. 

Compose: docker compose config to see resolved vars before run.


---

## Need to rollback to previous image version. How will you do it?

- Rollback steps (Docker)

```bash
# 1. List local images to find previous version
docker images | grep myapp

# 2. Stop current container
docker stop mycontainer
docker rm mycontainer

# 3. Run previous version (use digest or tag)
docker run -d --name mycontainer myapp:v1.2.0

```
Docker Compose rollback

```bash

# Edit docker-compose.yml, change image: myapp:latest → myapp:v1.2.0
# Then:
docker-compose down
docker-compose up -d
```

---

## Container not starting after host reboot. Why?

Containers are not persistent services by default — restart policies or orchestration tools are needed for auto-recovery.

---

## Docker Compose service not starting. What will you verify?

Verification checklist

- Config syntax: docker compose config -q — fails on YAML errors like wrong indentation.

- Service status/exit codes: docker compose ps -a — look for "Exited (1)", then docker compose logs --tail=50 <service>.

- Image availability: docker compose images — pull fails? Check auth/network.

- Port conflicts/volumes: docker compose ps --format "table {{.Name}}\t{{.Ports}}" and docker compose config --volumes — host ports taken, volumes missing.

- Dependencies: depends_on but service crashes before ready — use healthchecks.

- Env vars: docker compose run --rm <service> env — missing vars cause startup fail.

---

## Works in dev but fails in production container. Why?

If it works in dev but fails in a production container, it usually means there’s a difference between the environments.

- First, I check environment variables and configuration:

Missing or incorrect values (DB URL, API keys, ports)

- Next, I verify dependency differences:

Different versions of libraries or packages
Something installed locally but missing in the container

- Then, I check file paths and OS differences:

Case sensitivity (Linux vs local system)
Wrong working directory or missing files

- I also look at network and external services:

Database or API not reachable in production
Firewall or security group restrictions

- Another common issue is port and binding configuration:

App might be running on a different port
Bound to 127.0.0.1 instead of 0.0.0.0

- I also check permissions:

File or directory access issues inside container

- And finally, I review build vs runtime differences:

Something works during build but fails at runtime


Dev and production environments must be consistent — otherwise hidden differences cause failures.

---




