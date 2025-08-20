# Docker Volumes — Named, Bind, and Anonymous (Deep Dive + Hands‑On Tasks)

**Updated:** 2025-08-20

This guide explains Docker volumes end‑to‑end with clear examples, use cases, and step‑by‑step tasks you can run locally. It focuses on the three most common data‑persist strategies:

- **Named volumes** (Docker‑managed)
- **Bind mounts** (host‑managed)
- **Anonymous volumes** (auto‑created, unnamed)

> Works on Linux, macOS, and Windows (Docker Desktop). Platform‑specific notes and tips are included.

---

## Table of Contents

1. [What problem are volumes solving?](#what-problem-are-volumes-solving)
2. [Quick comparison](#quick-comparison)
3. [Named volumes](#named-volumes)
   - [Concept](#concept)
   - [Create & use](#create--use)
   - [Inspect & manage](#inspect--manage)
   - [Use cases](#use-cases)
   - [Pros / Cons](#pros--cons)
   - [Task: Named volume](#task-named-volume)
4. [Bind mounts](#bind-mounts)
   - [Concept](#concept-1)
   - [Create & use](#create--use-1)
   - [Read‑only bind](#read-only-bind)
   - [Use cases](#use-cases-1)
   - [Pros / Cons](#pros--cons-1)
   - [Task: Bind mount](#task-bind-mount)
5. [Anonymous volumes](#anonymous-volumes)
   - [Concept](#concept-2)
   - [Create & use](#create--use-2)
   - [Use cases](#use-cases-2)
   - [Pros / Cons](#pros--cons-2)
   - [Task: Anonymous volume](#task-anonymous-volume)
6. [Backups & restore](#backups--restore)
7. [Sharing data across containers](#sharing-data-across-containers)
8. [Docker Compose examples](#docker-compose-examples)
9. [Permissions & ownership](#permissions--ownership)
10. [Performance notes](#performance-notes)
11. [Common pitfalls & troubleshooting](#common-pitfalls--troubleshooting)
12. [`-v` vs `--mount` syntax](#-v-vs---mount-syntax)
13. [Cleanup](#cleanup)

---

## What problem are volumes solving?

Containers have a **writable layer** that disappears when the container is removed. For stateless workloads that’s fine. For **stateful data** (databases, uploads, logs you care about), you need storage **outside** the container’s ephemeral layer. **Volumes** provide that durable storage.

**ASCII snapshot**

```
Host
└── Docker
    ├── Container A (ephemeral writable layer)
    ├── Container B (ephemeral writable layer)
    └── Volumes (persist across container restarts/removals)
```

---

## Quick comparison

| Aspect | Named Volume | Bind Mount | Anonymous Volume |
|---|---|---|---|
| Who manages | Docker | You (host filesystem) | Docker (auto) |
| Path on host | Docker decides | Exact host path you choose | Docker decides |
| Portability | Medium (migrate via backup) | High (it’s just files on host) | Low (hard to identify later) |
| Typical use | Databases, app data | Live‑edit code, config, logs | Temporary persistence with minimal setup |
| Lifecycle | Survives container removal | Survives container removal | Survives container removal but unnamed |
| Best for | Safety & simplicity | Dev workflows & tight host integration | Quick demos; ephemeral data |

> All three persist beyond the container’s lifecycle; difference is **management and discoverability**.

---

## Named volumes

### Concept
A **named volume** is created and managed by Docker (driver `local` by default). You refer to it by name. Data is stored under Docker’s storage area (e.g., `/var/lib/docker/volumes/...` or inside Docker Desktop’s VM).

### Create & use

**CLI (`-v`):**
```bash
docker volume create app_data
docker run --rm -it -v app_data:/data alpine:3.20 sh -lc 'echo "hello" > /data/hello.txt && ls -l /data && cat /data/hello.txt'
```

**CLI (`--mount`, verbose & explicit):**
```bash
docker run --rm -it --mount type=volume,source=app_data,target=/data alpine:3.20 sh
```

Stop and start new containers with the same volume name to **see persistence**.

### Inspect & manage
```bash
docker volume ls
docker volume inspect app_data
docker volume rm app_data      # remove when not in use by any container
```

### Use cases
- Databases (PostgreSQL, MySQL, Redis) storage directories.
- Application assets that must persist across deployments but don’t need to be human‑edited on the host.
- Safer default than bind mounts for production.

### Pros / Cons
**Pros**
- Simple; Docker handles location & permissions scaffolding.
- Portable via `docker run`/Compose without hard‑coding host paths.
- Less risk of accidental host path clobbering.

**Cons**
- Not automatically human‑readable on the host (hidden under Docker’s paths).
- Needs backup/restore commands to move across machines.

### Task: Named volume

**Goal:** Create a named volume, write files, verify persistence across containers, back it up.

1. Create volume and write data:
   ```bash
   docker volume create notes_vol
   docker run --rm -v notes_vol:/data alpine:3.20 sh -lc 'uname -a > /data/notes.txt && date >> /data/notes.txt && ls -l /data'
   ```
2. Verify with a new container:
   ```bash
   docker run --rm -v notes_vol:/data alpine:3.20 sh -lc 'cat /data/notes.txt'
   ```
3. Backup the volume to a tarball in your current directory:
   ```bash
   docker run --rm      -v notes_vol:/data      -v "$PWD":/backup      alpine:3.20 sh -lc 'tar czf /backup/notes_vol.tgz -C /data . && ls -l /backup'
   ```
4. (Optional) Restore to a new volume and verify:
   ```bash
   docker volume create notes_restored
   docker run --rm -v notes_restored:/data -v "$PWD":/backup alpine:3.20 sh -lc 'tar xzf /backup/notes_vol.tgz -C /data && ls -l /data && cat /data/notes.txt'
   ```

Success criteria: `notes.txt` is present and identical in steps 2 and 4.

---

## Bind mounts

### Concept
A **bind mount** maps a **specific directory or file from your host** into the container. You fully control the host path. It’s perfect when you need to **edit files on the host** and see changes instantly inside the container.

- Linux/macOS:
  - Use `$(pwd)` or `$PWD` for the current directory.
- Windows (PowerShell):
  - Use a Windows path like `C:\Users\you\project` (note double backslashes in PowerShell).
- Windows (Git Bash/MINGW):
  - Use `/c/Users/you/project` or `$(pwd)`.

### Create & use

**Map a host folder into the container:**
```bash
mkdir -p ./site
echo "Hello from host @ $(date)" > ./site/index.html

# Linux/macOS:
docker run --rm -p 8080:80 -v "$PWD/site":/usr/share/nginx/html:rw nginx:1.27

# Windows PowerShell (example path):
# docker run --rm -p 8080:80 -v "C:\Users\you\site":/usr/share/nginx/html:rw nginx:1.27
```

Open `http://localhost:8080` and you’ll see your file served by NGINX. Edit `./site/index.html` on your host and refresh the page — changes appear immediately.

### Read‑only bind
```bash
docker run --rm -p 8080:80 -v "$PWD/site":/usr/share/nginx/html:ro nginx:1.27
```
Good for serving content without allowing container processes to modify your host files.

### Use cases
- Live development: mount source code into app containers.
- Mount configuration files (`.env`, `nginx.conf`, etc.).
- Collect logs directly to the host for analysis.

### Pros / Cons
**Pros**
- Full transparency and control; it’s just files on your host.
- Best DX (developer experience) for live editing.

**Cons**
- Can accidentally overwrite container paths if you mount the wrong host folder.
- Platform nuances (permissions, path formats, SELinux on Fedora/RHEL).
- Not as “portable” because paths are machine‑specific.

### Task: Bind mount

**Goal:** Serve static content from a host directory via NGINX and enforce read‑only in production.

1. Create content and start NGINX:
   ```bash
   mkdir -p ./web
   echo "<h1>Hello from bind mount</h1>" > ./web/index.html
   docker run --name webdev -d -p 8081:80 -v "$PWD/web":/usr/share/nginx/html:rw nginx:1.27
   ```
2. Verify in browser: open `http://localhost:8081`.
3. Edit `./web/index.html` and refresh — confirm live update.
4. Recreate container **read‑only** to simulate prod:
   ```bash
   docker rm -f webdev
   docker run --name webprod -d -p 8081:80 -v "$PWD/web":/usr/share/nginx/html:ro nginx:1.27
   ```
5. Attempt to write from inside the container (should fail):
   ```bash
   docker exec -it webprod sh -lc 'echo "should fail" > /usr/share/nginx/html/attempt.txt || echo "Write blocked (as expected)"'
   ```

Success criteria: RO mount blocks writes; live edits worked in RW mode.

---

## Anonymous volumes

### Concept
An **anonymous volume** is created **implicitly** when you mount a volume **without naming it**. Docker generates a random name (ID). The data persists, but it’s **harder to discover and manage** later.

Typical trigger patterns:
```bash
# Using -v with just a target path creates an anonymous volume:
docker run -d -v /data postgres:16

# Or in Dockerfile: VOLUME /data  (containers created from this image will attach anonymous volumes unless overridden)
```

### Create & use
```bash
docker run --name scratch -d -v /appdata alpine:3.20 sleep 600
docker inspect scratch --format '{{ json .Mounts }}' | python -m json.tool
docker volume ls   # you will see a randomly named volume
```

### Use cases
- Quick demos or throwaway experiments where you don’t care about the exact volume name.
- Images that declare `VOLUME` in the Dockerfile (e.g., databases) to avoid accidental writes to the container layer.

### Pros / Cons
**Pros**
- Zero setup; comes for free with some images.
- Data persists across container restarts.

**Cons**
- Hard to identify and reattach later (name is random).
- Easy to accumulate and forget; requires `docker volume prune` to clean up.

### Task: Anonymous volume

**Goal:** Observe how anonymous volumes appear and how to find them.

1. Run a container that creates an anonymous volume:
   ```bash
   docker run --name anon1 -d -v /cache alpine:3.20 sh -lc 'echo $$$$ > /cache/pid && sleep 300'
   ```
2. Inspect mounts and capture the volume name:
   ```bash
   docker inspect anon1 --format '{{ json .Mounts }}' | python -m json.tool
   docker volume ls | head
   ```
3. Remove the container and **re‑attach** the anonymous volume by its generated name:
   ```bash
   VOL=$(docker inspect anon1 --format '{{ (index .Mounts 0).Name }}')
   docker rm -f anon1
   docker run --rm -v "$VOL":/cache alpine:3.20 sh -lc 'echo "still there:" && cat /cache/pid'
   ```

Success criteria: The value written to `/cache/pid` survives and is readable after reattaching by name.

---

## Backups & restore

**Backup a named volume to a tarball on your host:**
```bash
docker run --rm -v myvol:/data -v "$PWD":/backup alpine:3.20 sh -lc 'tar czf /backup/myvol.tgz -C /data .'
```

**Restore tarball into a (new or empty) named volume:**
```bash
docker volume create myvol_restored
docker run --rm -v myvol_restored:/data -v "$PWD":/backup alpine:3.20 sh -lc 'tar xzf /backup/myvol.tgz -C /data'
```

---

## Sharing data across containers

Mount the **same named volume** into multiple containers to share state:

```bash
docker volume create shared
docker run -d --name writer -v shared:/data alpine:3.20 sh -lc 'i=0; while true; do echo $i >> /data/seq.txt; i=$((i+1)); sleep 1; done'
docker run --name reader --rm -it -v shared:/data alpine:3.20 sh -lc 'tail -f /data/seq.txt'
```

Stop with `docker rm -f writer`.

---

## Docker Compose examples

**Named volume (PostgreSQL):**
```yaml
# compose.yml
services:
  db:
    image: postgres:16
    environment:
      POSTGRES_PASSWORD: example
    volumes:
      - pgdata:/var/lib/postgresql/data
volumes:
  pgdata: { }
```

**Bind mount for app code (Node.js dev):**
```yaml
services:
  api:
    image: node:20-alpine
    working_dir: /app
    command: sh -lc "npm install && npm run dev"
    ports: ["3000:3000"]
    volumes:
      - ./:/app:rw
```

**Anonymous volume (implicit VOLUME in image or target‑only):**
```yaml
services:
  cachey:
    image: redis:7
    volumes:
      - /data  # anonymous unless named on the left side
```

> In Compose, **left side** names a volume; `- name:target` or `- name:target:ro`. If no left side is given, Docker creates an anonymous volume.

---

## Permissions & ownership

- Containers run with a **user ID** (often `root` in common images). Files created in mounted volumes adopt that UID/GID.
- For non‑root containers, ensure the directory is writable by that UID on the host (bind mounts) or `chown` after mount.
- On SELinux systems (Fedora/RHEL), add `:z` or `:Z` to bind mounts to relabel content:
  - `:z` — shared among containers
  - `:Z` — private to a single container
- Example with UID/GID fix:
  ```bash
  docker run --rm -u 1000:1000 -v "$PWD/appdata":/data alpine:3.20 sh -lc 'id && touch /data/ok.txt || (echo "no perms" && exit 1)'
  ```

---

## Performance notes

- Volumes (Docker‑managed) are generally **faster** than writing to the container layer.
- Bind mounts on macOS/Windows may be **slower** due to host ↔ VM filesystem sync. Consider cached/consistent options (where available) or use named volumes for performance‑critical paths.
- Databases in production commonly use **named volumes** for reliability and performance.

---

## Common pitfalls & troubleshooting

1. **Masking container paths**: a bind mount on a target path hides any files there from the image. Ensure you mount the **right folder**.
2. **Wrong host path**: empty directory mapped accidentally → app can’t find config. Double‑check paths.
3. **Permissions**: container user cannot write to host path (bind mounts). Adjust ownership or use named volumes.
4. **Anonymous clutter**: many anonymous volumes accumulate. Use `docker volume prune` cautiously.
5. **Windows path quoting**: In PowerShell, quote and escape backslashes. In Git Bash, use `/c/...` style paths.
6. **SELinux denials**: Use `:z` or `:Z` on bind mounts on SELinux hosts.

---

## `-v` vs `--mount` syntax

- `-v, --volume` is short and common: `-v name:/target[:ro]` or `-v /host:/target[:options]`.
- `--mount` is more explicit and safer for complex cases:
  - `--mount type=volume,source=name,target=/target,readonly`
  - `--mount type=bind,source=/host,target=/target,readonly`
- Prefer `--mount` for clarity; `-v` is fine for quick runs.

---

## Cleanup

```bash
# Stop and remove containers
docker ps -a
docker rm -f <container1> <container2>

# Remove specific volumes (must not be in use)
docker volume rm notes_vol notes_restored app_data shared || true

# Remove all dangling (unused) volumes (careful!)
docker volume prune
```

---

### You’re done!

You now know when to use **named**, **bind**, and **anonymous** volumes — and how to back them up, share them between containers, and avoid pitfalls. Keep these tasks handy and adapt them to your projects.