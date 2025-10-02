# 🐳 Docker Commands Cheat Sheet

Your one-stop reference for **Docker CLI commands & flags** 🚀  
This repo covers **all major Docker commands**, categorized for easy lookup.  

---

## 📌 1. Container Management

| Command | Description |
|---------|-------------|
| `docker run` | Create & start a new container. |
| `docker start` | Start a stopped container. |
| `docker stop` | Stop a running container. |
| `docker restart` | Restart a container. |
| `docker pause` | Pause processes in a container. |
| `docker unpause` | Resume paused container. |
| `docker kill` | Kill container immediately. |
| `docker rm` | Remove container(s). |
| `docker ps` | List running containers. |
| `docker ps -a` | List all containers (running + stopped). |
| `docker logs` | Fetch container logs. |
| `docker exec` | Run command inside running container. |
| `docker attach` | Attach terminal to container. |
| `docker inspect` | Show detailed info about container. |
| `docker top` | Show running processes in container. |
| `docker stats` | Show resource usage of container(s). |
| `docker cp` | Copy files between container & host. |
| `docker wait` | Block until container stops & print exit code. |
| `docker rename` | Rename a container. |
| `docker update` | Update container resource limits. |

---

## 📌 2. Image Management

| Command | Description |
|---------|-------------|
| `docker pull` | Download image from registry. |
| `docker push` | Upload image to registry. |
| `docker build` | Build image from Dockerfile. |
| `docker images` | List images. |
| `docker rmi` | Remove image(s). |
| `docker tag` | Tag image with new name. |
| `docker save` | Save image to tar archive. |
| `docker load` | Load image from tar archive. |
| `docker history` | Show history of image layers. |
| `docker inspect` | Show metadata of image. |
| `docker export` | Export container’s filesystem to tar. |
| `docker import` | Create image from tarball. |

---

## 📌 3. Network Management

| Command | Description |
|---------|-------------|
| `docker network ls` | List networks. |
| `docker network inspect` | Show details of a network. |
| `docker network create` | Create custom network. |
| `docker network rm` | Remove network(s). |
| `docker network connect` | Connect container to a network. |
| `docker network disconnect` | Disconnect container from a network. |

---

## 📌 4. Volume Management

| Command | Description |
|---------|-------------|
| `docker volume ls` | List volumes. |
| `docker volume inspect` | Show details of a volume. |
| `docker volume create` | Create new volume. |
| `docker volume rm` | Remove volume(s). |
| `docker volume prune` | Remove unused volumes. |

---

## 📌 5. System Commands

| Command | Description |
|---------|-------------|
| `docker info` | Show system-wide info. |
| `docker version` | Show Docker version. |
| `docker system df` | Show disk usage of Docker. |
| `docker system prune` | Remove unused data (containers, images, volumes, networks). |
| `docker events` | Stream real-time Docker events. |

---

## 📌 6. Registry (Docker Hub)

| Command | Description |
|---------|-------------|
| `docker login` | Login to registry. |
| `docker logout` | Logout from registry. |
| `docker search` | Search images in Docker Hub. |

---

## 📌 7. Docker Compose (Multi-Container Apps)

| Command | Description |
|---------|-------------|
| `docker compose up` | Start services defined in `docker-compose.yml`. |
| `docker compose down` | Stop and remove services. |
| `docker compose ps` | List running compose containers. |
| `docker compose logs` | View logs of compose services. |
| `docker compose exec` | Execute command in service container. |
| `docker compose build` | Build/rebuild services. |
| `docker compose restart` | Restart services. |
| `docker compose stop` | Stop services without removing. |
| `docker compose start` | Start stopped services. |

---

## 📌 8. Docker Swarm (Orchestration)

| Command | Description |
|---------|-------------|
| `docker swarm init` | Initialize a swarm cluster. |
| `docker swarm join` | Join a swarm as node/worker. |
| `docker swarm leave` | Leave the swarm. |
| `docker node ls` | List swarm nodes. |
| `docker service create` | Deploy new service in swarm. |
| `docker service ls` | List services. |
| `docker service ps` | List tasks of a service. |
| `docker service rm` | Remove service. |
| `docker stack deploy` | Deploy an app from compose file. |
| `docker stack ls` | List stacks. |
| `docker stack rm` | Remove stack. |

---

## 📌 9. Builder (Advanced Image Builds)

| Command | Description |
|---------|-------------|
| `docker builder build` | Build image using BuildKit. |
| `docker builder prune` | Clean up build cache. |
| `docker buildx` | Extended build (multi-arch, caching). |

---

## 📌 10. Commonly Used Flags (Quick Reference)

| Flag | Description |
|------|-------------|
| `-d` | Run in detached mode (background). |
| `-it` | Interactive terminal mode. |
| `-p` | Map container port to host port (`host:container`). |
| `-P` | Map all exposed ports randomly. |
| `-e` | Set environment variable. |
| `--env-file` | Load environment variables from file. |
| `--name` | Assign container a name. |
| `--rm` | Auto-remove container after exit. |
| `-v` | Mount volume or bind host dir. |
| `--mount` | Advanced mount (bind, volume, tmpfs). |
| `--network` | Connect container to network. |
| `--hostname` | Set container hostname. |
| `--workdir` | Set working directory. |
| `-u` | Run as specific user (UID:GID). |
| `--entrypoint` | Override default entrypoint. |
| `--cpu-shares` | Limit CPU priority. |
| `--memory` | Limit RAM usage. |
| `--restart` | Set restart policy. |
| `--privileged` | Give extended privileges (not safe). |

---

## ✨ Author
Made with ❤️ by **Saime Shaikh**  
🚀 DevOps Enthusiast | Linux • Docker • Kubernetes • AWS • Terraform  
