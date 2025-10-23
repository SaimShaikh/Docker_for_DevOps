# Docker Container Lifecycle 

<img width="2048" height="2048" alt="image" src="https://github.com/user-attachments/assets/de16b616-1ecc-476d-b23c-59e9b59379d4" />




---

## 🔄 Container Life Cycle Phases

### 🧱 1. Build

* Package your application and its dependencies into a Docker image.
* **Command:**

```bash
docker build -t myapp:1.0 .
```

### 🚢 2. Ship

* Push or pull your image from a container registry (like Docker Hub).
* **Commands:**

```bash
docker push myapp:1.0
docker pull myapp:1.0
```

### ▶️ 3. Run

* Create and start a container from an image.
* **Command:**

```bash
docker run -d --name myapp -p 8080:80 myapp:1.0
```

### ⏹️ 4. Stop

* Gracefully stop a running container.
* **Command:**

```bash
docker stop myapp
```

### 🗑️ 5. Remove

* Delete the container and free up resources.
* **Command:**

```bash
docker rm myapp
```



---

## 🧰 Quick Commands Summary

| Stage      | Description        | Command            |
| ---------- | ------------------ | ------------------ |
| **Build**  | Create an image    | `docker build`     |
| **Ship**   | Push or pull image | `docker push/pull` |
| **Run**    | Start container    | `docker run`       |
| **Stop**   | Stop container     | `docker stop`      |
| **Remove** | Delete container   | `docker rm`        |

---

### ✨ About

This repo visually and practically explains how Docker containers move through their **life cycle** — **Build → Ship → Run → Stop → Remove**.

Perfect for students, DevOps beginners, or professionals who want a visual and command-level understanding of Docker fundamentals.
