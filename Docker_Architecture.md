<img width="1536" height="1024" alt="image" src="https://github.com/user-attachments/assets/0c704091-50df-42b1-b60a-6edcfdfecbbe" />

---

# 🐳 Docker Architecture

Docker is a platform that allows developers to **build, ship, and run applications inside containers**.  
To understand how Docker works, let’s break down its **architecture** step by step.  

---

## 📌 Main Components of Docker Architecture

### 1. **Docker Client (CLI / Docker Desktop)**
- The Docker Client is the tool we use to talk to Docker.

- Most of the time it’s the Docker CLI (command line interface). When I type commands like docker run, docker ps, or docker build, I am using the Docker Client.

- The client doesn’t run containers by itself. It just sends my commands to the Docker Daemon using the Docker API. The daemon then does the actual work — like creating containers or building images.
- You run commands like:

   ```bash
  docker build
  docker run
  docker pull
  docker push
   ```

- The client sends these commands to the Docker Daemon

### 2. **Docker Daemon (dockerd)**
- The Docker Daemon is the background service in Docker. It’s the part that actually does all the heavy work.

- When I run commands like docker run or docker build, my request goes to the Docker Daemon. The daemon then pulls images, builds them, runs containers, manages networks, and handles storage.

- The Docker client (CLI) only sends instructions, but the daemon is the engine that makes containers run.
- It listens to requests from the Docker Client and executes them.

### 3. **Docker Images**
- A Docker image is like a blueprint or template for creating containers.

- It has the application code, runtime, libraries, and all dependencies inside it.

- When we run the image, it becomes a container. The image itself is read-only, and the container adds a writable layer on top of it.
- Example:
    - nginx:latest → official Nginx image
    - python:3.9 → Python environment
- You can create your own images using a Dockerfile.

### 4. **Docker Containers**

- A Docker container is like a small, lightweight box that has everything an application needs to run — the code, libraries, and settings.

- It is built from a Docker image, and when we run the image, it becomes a container.

- Unlike a virtual machine, a container doesn’t need a full operating system. It shares the host OS kernel, so it’s faster, smaller, and starts quickly.

- For example: If I have a Python app, instead of installing Python and all dependencies on every server, I just put it in a Docker image and run it as a container anywhere
  it will work the same on my laptop, test server, or cloud
``docker run -d -p 8080:80 nginx``
**👉 Runs an Nginx web server in a container.**

### 5. **Docker Registry (Docker Hub / Private Registry)**

- A Docker Registry is like a storage or library where Docker images are kept.

- When we build an image, we can push it to a registry so that others (or our servers) can pull it and run containers.

- The most common public registry is Docker Hub, but we can also use private registries like AWS ECR, GCP Artifact Registry, or Harbor.
- Workflow:
    - docker pull <image> → downloads from registry
    - docker push <image> → uploads to registry 


**✅ Key Points to Remember**

- Images → Blueprints

- Containers → Running instances

- Daemon → The engine

- Client → Your commands

- Registry → Where images live

----
### What ia the Dockerfile
- A Dockerfile is a simple text file that has a set of instructions to build a Docker image.

- Instead of creating an image manually, we write steps inside the Dockerfile — like which base image to use, what packages to install, copy application code, and what command should run.

- Docker reads the Dockerfile step by step and creates an image. Later, we can run containers from that image anywhere.


----

# 📌 Step-by-step with Dockerfile Instructions

| Instruction | What it does                               | Scenario Example                                                         |
| ----------- | ------------------------------------------ | ------------------------------------------------------------------------ |
| `FROM`      | Choose a base image (starting point).      | You need Python, so: <br> `FROM python:3.9`                              |
| `WORKDIR`   | Set working folder inside container.       | Tell Docker: “work inside `/app`.” <br> `WORKDIR /app`                   |
| `COPY`      | Copy files from host → container.          | Bring your project code into the image. <br> `COPY . /app`               |
| `RUN`       | Execute a command at build time.           | Install Flask & dependencies: <br> `RUN pip install -r requirements.txt` |
| `ENV`       | Set environment variable.                  | Example: set the app port <br> `ENV PORT=5000`                           |
| `EXPOSE`    | Declare which port container listens on.   | Flask runs on `5000`: <br> `EXPOSE 5000`                                 |
| `CMD`       | Final default command when container runs. | Start your app: <br> `CMD ["python", "app.py"]`                          |


---


### Common instructions:

```bash
FROM → Base image (like Ubuntu, Python, Node).

COPY / ADD → Copy files into image.

RUN → Install dependencies.

CMD / ENTRYPOINT → Default command when container runs.

EXPOSE → Ports used.
```

> Used with docker build to create images.

### what is the build time and run time in docker

**📌 The Core Idea**

- Build Time = when Docker is building the image (using your Dockerfile).
    - Think of it like: Build time → baking a cake 🎂 (ingredients + recipe).

**Happens when you run:**

``docker build -t myapp . ``

### 🔹 What happens here:

- Docker reads the Dockerfile.

- Executes instructions like FROM, RUN, COPY, ENV, etc.

- Creates an image (a frozen snapshot with everything ready).
> 👉 Once done, you have a nice Docker image 🍱 (like a packed lunchbox).
---

- Run Time = when Docker is running a container from that image.
  - Think of it like: Run time → eating the cake 🍴 (cake is ready to serve).
 

  
