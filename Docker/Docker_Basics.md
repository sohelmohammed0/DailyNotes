
# Docker Complete Guide (Beginner to Interview-Ready)

This document covers all important Docker concepts, definitions, differences, commands, best practices, and interview questions with answers.

---

## 1. What is Docker?
Docker is an **open-source containerization platform** that allows you to build, package, and run applications in isolated environments called **containers**.

---

## 2. Docker Engine
The core component of Docker, responsible for creating and managing containers.
- **Docker Daemon (`dockerd`)**: Background service that manages Docker objects.
- **Docker CLI**: Command-line interface to interact with the daemon.
- **Docker REST API**: Enables communication between CLI, daemon, and external tools.

---

## 3. Docker Image
A **read-only blueprint** containing application code, dependencies, libraries, environment variables, and configurations.

**Example:** `python:3.10-slim` contains Python 3.10 runtime with minimal OS packages.

---

## 4. Docker Container
A **running instance** of a Docker image. Containers are:
- Lightweight
- Portable
- Isolated from the host OS but share its kernel

**Example:**
```bash
docker run -d -p 80:80 nginx
```

---

## 5. Docker Registry
A repository for Docker images.
- **Public Registry**: Docker Hub
- **Private Registry**: Self-hosted registry for organizations

---

## 6. Docker Hub
A cloud-based registry maintained by Docker Inc., containing official and community images.

---

## 7. Dockerfile
A text file containing instructions for building Docker images.
**Common instructions:**
- `FROM`: Base image
- `WORKDIR`: Working directory inside container
- `COPY` / `ADD`: Copy files
- `RUN`: Execute commands
- `CMD`: Default command
- `ENTRYPOINT`: Main executable
- `EXPOSE`: Port
- `ENV`: Environment variables

---

## 8. Docker Volume
Method for persisting data beyond the container lifecycle.
- **Named Volumes**
- **Bind Mounts**
- **tmpfs** (in-memory storage)

Example:
```bash
docker run -v myvolume:/app/data nginx
```

---

## 9. Docker Network
Allows containers to communicate.
- **Bridge**: Default, internal communication
- **Host**: Shares host network
- **Overlay**: Multi-host networking

Example:
```bash
docker network create mynet
docker run --network=mynet nginx
```

---

## 10. Docker Compose
A tool for running multi-container applications using `docker-compose.yml`.

Example:
```yaml
version: '3'
services:
  web:
    image: nginx
    ports:
      - "80:80"
  db:
    image: mysql
    environment:
      MYSQL_ROOT_PASSWORD: rootpass
```
Run:
```bash
docker-compose up -d
```

---

## 11. Differences

### Image vs Container
| Image | Container |
|-------|-----------|
| Read-only template | Running instance |
| Immutable | Mutable |
| Stored in registry | Runs on host system |

### Virtualization vs Containerization
| Virtualization | Containerization |
|----------------|------------------|
| Full OS per VM | Shares host OS kernel |
| Heavy | Lightweight |
| Slow startup | Fast startup |

### Monolith vs Microservices
| Monolith | Microservices |
|----------|---------------|
| Single large app | Multiple independent services |
| Hard to scale | Easily scalable |

---

## 12. Dockerfile Optimization Best Practices
- Use small base images (e.g., `alpine`).
- Combine RUN commands to reduce layers.
- Use `.dockerignore` to exclude unnecessary files.
- Apply multi-stage builds.

Example:
```dockerfile
FROM golang:1.20-alpine as builder
WORKDIR /app
COPY . .
RUN go build -o myapp

FROM alpine:latest
COPY --from=builder /app/myapp .
CMD ["./myapp"]
```

---

## 13. Common Docker Commands
```bash
docker --version           # Check Docker version
docker pull nginx          # Download image
docker images              # List images
docker run nginx           # Run container
docker ps                  # List running containers
docker stop <id>           # Stop container
docker rm <id>             # Remove container
docker rmi <image_id>      # Remove image
docker exec -it <id> bash  # Access container shell
```

---

## 14. Deploying Applications with Docker
1. Create app code
2. Write Dockerfile
3. Build image:
   ```bash
   docker build -t myapp .
   ```
4. Run container:
   ```bash
   docker run -p 5000:5000 myapp
   ```
5. Push to registry:
   ```bash
   docker tag myapp user/myapp
   docker push user/myapp
   ```

---

## 15. Interview Questions and Answers

**Q1: Difference between CMD and ENTRYPOINT?**
- CMD: Default arguments, can be overridden.
- ENTRYPOINT: Defines executable, harder to override.

**Q2: How to optimize image size?**
- Use `alpine` base images
- Multi-stage builds
- `.dockerignore`
- Combine RUN commands

**Q3: docker run vs docker start?**
- run: Creates + starts new container
- start: Starts stopped container

**Q4: How to share data between containers?**
- Volumes, bind mounts, networks

**Q5: COPY vs ADD?**
- COPY: Local copy only
- ADD: COPY + URL support + tar extraction

**Q6: docker ps vs docker ps -a?**
- ps: Running containers
- ps -a: All containers

---

## 16. Summary
- Docker simplifies application deployment.
- Containers are portable, lightweight, and fast.
- Understanding Docker architecture, networking, and volumes is key for interviews.
- Practice building custom images and running multi-container setups.
