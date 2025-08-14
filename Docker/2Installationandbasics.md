
# 🚀 Docker Installation & Basics

---

## 1️⃣ Uninstall Conflicting Packages

Before installing Docker, remove old or conflicting versions.

```bash
for pkg in docker.io docker-doc docker-compose docker-compose-v2 podman-docker containerd runc; do 
    sudo apt-get remove $pkg; 
done
```

**Why?**
Older packages like `docker.io` (Ubuntu’s repo version) can conflict with Docker’s official version.

**Check if Docker is removed:**
```bash
docker --version
```
If removed, you should see **command not found**.

---

## 2️⃣ Set Up Docker’s Official Repository

### Step 1: Install Required Packages
```bash
sudo apt-get update
sudo apt-get install ca-certificates curl
```

- `ca-certificates`: Enables secure HTTPS connections.
- `curl`: Used to download the GPG key.

### Step 2: Add Docker’s GPG Key
```bash
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc
```

**Why?**
The GPG key ensures Docker packages are verified and not tampered with.

### Step 3: Add Docker Repository
```bash
echo   "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu   $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}") stable" |   sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```

**Verify Repository:**
```bash
apt-cache policy docker-ce
```

---

## 3️⃣ Install Docker
```bash
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

**Components:**
- `docker-ce`: Docker Engine (Community Edition)
- `docker-ce-cli`: Command line interface
- `containerd.io`: Container runtime
- `docker-buildx-plugin`: Advanced build support
- `docker-compose-plugin`: Compose v2 support

**Check Installation:**
```bash
docker --version
```

---

## 4️⃣ Check Docker Status
```bash
sudo systemctl status docker
```
- Shows if Docker daemon is running.

Start Docker if stopped:
```bash
sudo systemctl start docker
sudo systemctl enable docker
```

---

## 5️⃣ Official vs Custom Images

- **Official Images**: Maintained by Docker or verified vendors. Example: `nginx`, `mysql`.
- **Custom Images**: Built by you for specific applications.

**Example:**
```bash
docker pull nginx       # Official image
docker build -t mynginx .  # Custom image
```

---

## 6️⃣ Creating & Running Containers

Run Nginx:
```bash
docker run -d --name mynginx -p 8080:80 nginx
```
- `-d`: Detached mode
- `--name`: Assigns container name
- `-p 8080:80`: Maps host port 8080 to container’s port 80

Check logs:
```bash
docker logs mynginx
```

---

## 7️⃣ Managing Containers

| Action                | Command                          |
|-----------------------|----------------------------------|
| List running          | `docker ps`                      |
| List all              | `docker ps -a`                   |
| Start                 | `docker start <id>`              |
| Stop                  | `docker stop <id>`               |
| Pause                 | `docker pause <id>`              |
| Unpause               | `docker unpause <id>`            |
| Remove                | `docker rm <id>`                 |
| Only container IDs    | `docker ps -q`                   |
| Only image IDs        | `docker images -q`               |
| Force remove          | `docker rm -f <id>`              |

---

## 8️⃣ Checking CPU & Memory Usage
```bash
docker stats
```
Displays live CPU, memory, and network usage.

---

## 9️⃣ Docker Info
```bash
docker info
```
Shows:
- Total containers & images
- Storage driver
- OS info

---

## 🔟 Docker Networking
```bash
docker network ls
docker network inspect bridge
```

---

## ✅ Best Practices
- Use **official images** for security.
- Tag images with version numbers (`myapp:v1.0`).
- Use `.dockerignore` to reduce build size.
- Clean unused containers/images:  
```bash
docker system prune
```

---
