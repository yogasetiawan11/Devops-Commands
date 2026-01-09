Below is a **command-by-command, practical Docker cheat sheet**. I keep it **clear, direct, and production-oriented**.

---

## Intro

No command. This is overview of Docker operational tips.

---

## Build Docker Image **Without Cache**

Use this when you want a clean build and ignore layer cache.

```bash
docker build --no-cache -t myapp:latest .
```

Why:

* Force fresh install of packages
* Avoid stale dependencies

---

## Dockerfile in **Different Directory**

Use `-f` flag.

```bash
docker build -f docker/Dockerfile -t myapp:latest .
```

If context also different:

```bash
docker build -f docker/Dockerfile -t myapp:latest docker/
```

---

## Git Commit ID as Docker Tag

Best practice for traceability.

```bash
GIT_COMMIT=$(git rev-parse --short HEAD)
docker build -t myapp:$GIT_COMMIT .
```

Verify:

```bash
docker images
```

---

## Date as Docker Tag

Useful for releases and rollback.

```bash
DATE=$(date +%Y%m%d)
docker build -t myapp:$DATE .
```

Or include time:

```bash
date +%Y%m%d%H%M
```

---

## **RAM & CPU** for Docker Container

### Limit memory

```bash
docker run -m 512m myapp
```

### Limit CPU

```bash
docker run --cpus="1.5" myapp
```

### Both combined

```bash
docker run -m 512m --cpus="1.0" myapp
```

---

## Run Commands as **Non-root User**

### Dockerfile

```dockerfile
RUN adduser --disabled-password appuser
USER appuser
```

### Runtime override

```bash
docker run --user 1001:1001 myapp
```

Security benefit:

* Reduce container breakout risk

---

## Running Container as **Read Only**

Filesystem becomes immutable.

```bash
docker run --read-only myapp
```

Allow temp write:

```bash
docker run --read-only --tmpfs /tmp myapp
```

---

## Setup **Automatic Container Restart**

```bash
docker run --restart always myapp
```

Other policies:

```bash
--restart on-failure
--restart unless-stopped
```

---

## **HealthCheck** Of Container

### Dockerfile

```dockerfile
HEALTHCHECK CMD curl -f http://localhost:8080 || exit 1
```

### Check status

```bash
docker inspect --format='{{.State.Health.Status}}' container_name
```

---

## **Tailing Logs** Of Container

```bash
docker logs -f container_name
```

Last 50 lines:

```bash
docker logs --tail 50 container_name
```

---

## Check If Specific Process Running in Container

```bash
docker exec container_name ps aux
```

Filter:

```bash
docker exec container_name ps aux | grep nginx
```

---

## Running Containers In **Restricted Mode**

Highly secure container.

```bash
docker run \
  --read-only \
  --cap-drop ALL \
  --security-opt no-new-privileges \
  myapp
```

---

## Get Container Logs On **Host Machine**

Docker stores logs here (default):

```bash
/var/lib/docker/containers/<container-id>/<container-id>-json.log
```

Find container ID:

```bash
docker inspect --format='{{.Id}}' container_name
```

---

## Docker Prune (**Delete Unused Resources**)

Remove everything unused:

```bash
docker system prune
```

Aggressive:

```bash
docker system prune -a
```

---

## Delete **Dangling Images**

```bash
docker image prune
```

---

## Delete **All Containers**

Stop all:

```bash
docker stop $(docker ps -aq)
```

Delete all:

```bash
docker rm $(docker ps -aq)
```

---

## List Images Created **After Specific Image**

```bash
docker images --filter since=myapp:1.0
```

---

## Rename Containers

```bash
docker rename old_name new_name
```

---

## Restart Policy Setup (Again – Verify)

```bash
docker inspect --format='{{.HostConfig.RestartPolicy.Name}}' container_name
```

---

## **Pause & Unpause** Containers

Pause:

```bash
docker pause container_name
```

Unpause:

```bash
docker unpause container_name
```

---

## Docker **Diff** Command

See filesystem changes.

```bash
docker diff container_name
```

Output meanings:

* `A` added
* `C` changed
* `D` deleted

---

## **Export Docker Images**

```bash
docker save myapp:latest -o myapp.tar
```

---

## Import Docker Image From **tar Files**

```bash
docker load -i myapp.tar
```

---

## Copy Files From Container → Host

```bash
docker cp container_name:/app/logs/app.log ./app.log
```

Host → Container:

```bash
docker cp local.txt container_name:/app/local.txt
```

---

## Real-time Container **RAM & CPU Usage**

```bash
docker stats
```

Single container:

```bash
docker stats container_name
```

---

