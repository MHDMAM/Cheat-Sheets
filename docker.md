# 🐳 Docker & Docker Compose Cheat Sheet

[← Back to Main Index](./README.md)

A comprehensive guide covering everyday development operations, from container lifecycles to multi-stage builds and Docker Compose workflows.

---

## 📦 Container Lifecycle & Operations

| Command | Action |
| :--- | :--- |
| `docker run <image>` | Create and start a container from an image. |
| `docker run -d --name <name> <image>` | Run a named container in the background (detached mode). |
| `docker run -p <host_port>:<container_port> <image>` | Run a container and map host ports to container ports. |
| `docker run -it <image> sh` | Run a container interactively with a shell (use `bash` if the image has it). |
| `docker run --rm <image>` | Run a container and remove it automatically when it exits. |
| `docker run -e KEY=value --env-file .env <image>` | Pass environment variables into a container. |
| `docker run -v <volume>:<container_path> <image>` | Mount a named volume into a container. |
| `docker run -v "$(pwd)":<container_path> <image>` | Bind-mount the current directory into a container. |
| `docker run --restart unless-stopped <image>` | Restart the container automatically unless you stop it. |
| `docker ps` | List all **running** containers. |
| `docker ps -a` | List **all** containers (running and stopped). |
| `docker stop <container>` | Gracefully stop a running container. |
| `docker start <container>` | Start a stopped container. |
| `docker restart <container>` | Restart a container. |
| `docker rm <container>` | Remove a stopped container. |
| `docker rm -f <container>` | Force-remove a running container. |

---

## 🔍 Inspection & Debugging

| Command | Action |
| :--- | :--- |
| `docker logs <container>` | View the logs of a container. |
| `docker logs -f --tail 100 <container>` | Follow container logs in **real time**, starting from the last 100 lines. |
| `docker exec -it <container> sh` | Open an interactive shell **inside** a running container. |
| `docker inspect <object>` | Get low-level, detailed JSON info on any Docker object. |
| `docker inspect -f '{{.NetworkSettings.IPAddress}}' <container>` | Extract a single field from `inspect` output. |
| `docker stats` | Monitor live CPU, memory, and network usage of containers. |
| `docker top <container>` | List the processes running inside a container. |
| `docker port <container>` | Show the port mappings of a container. |
| `docker cp <container>:<path> <local_path>` | Copy files out of (or into) a container. |
| `docker diff <container>` | Inspect changes made to the container file system. |

---

## 🖼️ Image Management

| Command | Action |
| :--- | :--- |
| `docker images` | List all local images. |
| `docker build -t <name>:<tag> .` | Build an image from the Dockerfile in the current directory. |
| `docker build -f <path/to/Dockerfile> -t <name> .` | Build using a Dockerfile at a custom path. |
| `docker build --no-cache -t <name> .` | Build an image from scratch without using cached layers. |
| `docker build --target <stage> -t <name> .` | Build only up to a specific stage of a multi-stage Dockerfile. |
| `docker tag <image> <username>/<image>:<tag>` | Tag an image for pushing to a registry. |
| `docker login` | Authenticate to Docker Hub (or `docker login <registry>`). |
| `docker pull <image>` | Download an image from a registry. |
| `docker push <username>/<image>:<tag>` | Upload a built image to a remote registry. |
| `docker rmi <image>` | Delete a local image. |
| `docker history <image>` | View the history and layers of a local image. |

---

## 🏗️ Multi-Stage Builds

Build in a heavy image, ship only the output in a slim one. Keep a `.dockerignore` (e.g. `node_modules`, `.git`, `.env`) next to the Dockerfile to keep the build context small.

<details markdown="block">
<summary>🔍 Example multi-stage Dockerfile (Node.js)...</summary>

```dockerfile
# ---- Build stage ----
FROM node:20-alpine AS build
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build

# ---- Runtime stage ----
FROM node:20-alpine
WORKDIR /app
ENV NODE_ENV=production
COPY package*.json ./
RUN npm ci --omit=dev
COPY --from=build /app/dist ./dist
USER node
EXPOSE 3000
CMD ["node", "dist/index.js"]
```

</details>

---

## 🐙 Docker Compose Operations

| Command | Action |
| :--- | :--- |
| `docker compose up` | Create and start all services defined in `compose.yaml` / `docker-compose.yml`. |
| `docker compose up -d` | Start services in the background (detached mode). |
| `docker compose up -d --build` | Rebuild images and then start containers in the background. |
| `docker compose up -d <service>` | Start (or recreate) only a specific service. |
| `docker compose down` | Stop and remove containers and networks (volumes are **kept**). |
| `docker compose down -v` | Also remove named volumes declared in the file and anonymous volumes. |
| `docker compose ps` | List all containers managed by the current Compose stack. |
| `docker compose logs -f <service>` | Tail logs for one service (omit `<service>` for all). |
| `docker compose exec <service> <cmd>` | Execute a command inside a specific running service container. |
| `docker compose run --rm <service> <cmd>` | Run a one-off command in a new container for a service. |
| `docker compose restart` | Restart all services in the stack. |
| `docker compose pull` | Pull the latest images for all services. |
| `docker compose config` | Validate and print the fully resolved Compose file. |
| `docker compose -f <file> up -d` | Use a specific Compose file. |

<details markdown="block">
<summary>🔍 Minimal <code>compose.yaml</code> template...</summary>

```yaml
services:
  app:
    build: .
    ports:
      - "3000:3000"
    env_file: .env
    depends_on:
      db:
        condition: service_healthy
    restart: unless-stopped

  db:
    image: postgres:16-alpine
    environment:
      POSTGRES_PASSWORD: example
    volumes:
      - db-data:/var/lib/postgresql/data
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U postgres"]
      interval: 5s
      retries: 5

volumes:
  db-data:
```

</details>

---

## 🌐 Networking & 💾 Volumes

| Command | Action |
| :--- | :--- |
| `docker network ls` | List all available networks. |
| `docker network create <name>` | Create a new user-defined network. |
| `docker network connect <network> <container>` | Connect an existing container to a network. |
| `docker network inspect <network>` | Show the containers and config of a network. |
| `docker volume ls` | List all persistent volumes. |
| `docker volume create <name>` | Create a named data volume. |
| `docker volume inspect <name>` | Show where a volume lives on the host and its metadata. |
| `docker volume rm <name>` | Delete a volume (must not be in use). |

> Containers on the same user-defined network can reach each other by container/service name (e.g. `http://db:5432`).

---

## 🧹 System Cleanup

Docker can quickly eat up disk space with dangling images and stopped containers.

| Command | Action |
| :--- | :--- |
| `docker system df` | Show how much disk space images, containers, volumes and build cache use. |
| `docker container prune` | Remove all stopped containers. |
| `docker image prune` | Remove dangling (untagged) images. |
| `docker image prune -a` | Remove **all** images not used by any container. |
| `docker volume prune` | Remove unused anonymous volumes (add `-a` to include named ones). |
| `docker builder prune` | Clear the build cache. |
| `docker system prune` | Remove stopped containers, unused networks, dangling images and build cache. |
| `docker system prune -a --volumes` | ⚠️ Wipe everything unused, including all unused images and volumes. |
