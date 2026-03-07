# Docker & Docker Compose Cheat Sheet

A quick reference for common Docker commands.

---

## Image Management

**Build an image from a Dockerfile**

```bash
# docker build -t <image_name>:<tag> <path_to_dockerfile_directory>
docker build -t my-app:1.0 .
```

**List all local images**

```bash
docker images
```

**Remove an image**

```bash
# docker rmi <image_id_or_name>
docker rmi my-app:1.0
```

---

## Container Management

**Run a container from an image**

```bash
# -d: detached (background) mode
# -p: map host port to container port
# --name: assign a name to the container
docker run -d -p 8080:5000 --name web-server my-app:1.0
```

**List running containers**

```bash
docker ps
```

**List all containers (running and stopped)**

```bash
docker ps -a
```

**Stop a running container**

```bash
docker stop <container_id_or_name>
```

**Remove a stopped container**

```bash
docker rm <container_id_or_name>
```

**View logs from a container**

```bash
# -f: follow the log output
docker logs -f <container_id_or_name>
```

**Execute a command inside a running container**

```bash
# -it: interactive terminal
docker exec -it <container_id_or_name> /bin/bash
```

---

## Docker Compose

**Build and start services**

```bash
# --build: force a rebuild of the image
# -d: run in detached mode
docker compose up --build -d
```

**Stop and remove services, networks, and volumes**

```bash
docker compose down
```

---

## System Cleanup

**Remove all unused containers, networks, images, and build cache**

```bash
docker system prune -a
```
