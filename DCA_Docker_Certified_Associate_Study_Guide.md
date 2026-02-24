# DCA — Docker Certified Associate Study Guide

> **Exam:** Docker Certified Associate (DCA)  
> **Format:** 55 questions · 90 minutes · Proctored  
> **Passing score:** 65 % (≈ 36 / 55)  
> **Validity:** 2 years  

---

## How to Use This Guide

1. **Read linearly first** — each domain builds on Docker fundamentals.
2. **Run every CLI command** on a lab machine or Docker Desktop — muscle memory matters.
3. **Mark pitfalls** — they are the exam's favourite traps.
4. **Answer every mini-quiz** before scrolling to the next section.
5. **Use the Cheat Sheet** as your final-hour reference.
6. **Take the 55-question practice exam** under timed conditions (90 min).
7. **Revisit any section** where you score < 65 % on the mini-quizzes.
8. **Orchestration (25 %)** is the heaviest domain — invest the most time there.
9. **Scenario Playbook** near the end simulates real-world tasks — do these last.
10. Check the **Final Revision Checklist** the night before the exam.

---

## Table of Contents

- [Cheat Sheet Quick Reference](#cheat-sheet-quick-reference)
- [Domain 1 — Orchestration (25 %)](#domain-1--orchestration-25-)
  - [1.1 Swarm Mode Basics](#11-swarm-mode-basics)
  - [1.2 Services vs Containers](#12-services-vs-containers)
  - [1.3 Stacks & Compose-to-Swarm](#13-stacks--compose-to-swarm)
  - [1.4 Scaling & Replicas](#14-scaling--replicas)
  - [1.5 Placement Constraints & Node Labels](#15-placement-constraints--node-labels)
  - [1.6 Rolling Updates & Rollbacks](#16-rolling-updates--rollbacks)
  - [1.7 Secrets & Configs](#17-secrets--configs)
  - [1.8 Troubleshooting Services](#18-troubleshooting-services)
- [Domain 2 — Image Creation, Management & Registry (20 %)](#domain-2--image-creation-management--registry-20-)
  - [2.1 Dockerfile Instructions](#21-dockerfile-instructions)
  - [2.2 Build Options & Multistage Builds](#22-build-options--multistage-builds)
  - [2.3 Tagging, Pushing & Pulling](#23-tagging-pushing--pulling)
  - [2.4 Private Registry](#24-private-registry)
  - [2.5 Inspecting & Pruning Images](#25-inspecting--pruning-images)
- [Domain 3 — Installation & Configuration (15 %)](#domain-3--installation--configuration-15-)
  - [3.1 Installing Docker Engine](#31-installing-docker-engine)
  - [3.2 Daemon Configuration](#32-daemon-configuration)
  - [3.3 Logging Drivers](#33-logging-drivers)
  - [3.4 Managing Docker Service](#34-managing-docker-service)
  - [3.5 Certificate-Based Auth](#35-certificate-based-auth)
  - [3.6 Namespaces & Cgroups](#36-namespaces--cgroups)
- [Domain 4 — Networking (15 %)](#domain-4--networking-15-)
  - [4.1 Network Drivers Overview](#41-network-drivers-overview)
  - [4.2 Bridge Networks](#42-bridge-networks)
  - [4.3 Overlay Networks](#43-overlay-networks)
  - [4.4 Host & Macvlan](#44-host--macvlan)
  - [4.5 Publishing Ports & DNS](#45-publishing-ports--dns)
  - [4.6 Service Discovery](#46-service-discovery)
- [Domain 5 — Security (15 %)](#domain-5--security-15-)
  - [5.1 Docker Content Trust](#51-docker-content-trust)
  - [5.2 User Namespaces & Capabilities](#52-user-namespaces--capabilities)
  - [5.3 Securing Containers](#53-securing-containers)
  - [5.4 Secrets Management in Swarm](#54-secrets-management-in-swarm)
  - [5.5 Security Best Practices](#55-security-best-practices)
- [Domain 6 — Storage & Volumes (10 %)](#domain-6--storage--volumes-10-)
  - [6.1 Bind Mounts vs Volumes vs tmpfs](#61-bind-mounts-vs-volumes-vs-tmpfs)
  - [6.2 Volume Drivers & Plugins](#62-volume-drivers--plugins)
  - [6.3 Persisting Data](#63-persisting-data)
  - [6.4 Backup & Restore](#64-backup--restore)
- [Scenario Playbook (6 Scenarios)](#scenario-playbook-6-scenarios)
- [Practice Exam (55 Questions)](#practice-exam-55-questions)
- [Answer Key & Explanations](#answer-key--explanations)
- [Final Revision Checklist](#final-revision-checklist)

---

## Cheat Sheet Quick Reference

### Essential Docker CLI

| Command | Purpose |
|---|---|
| `docker run -d --name web -p 80:80 nginx` | Run container detached with port mapping |
| `docker ps -a` | List all containers (including stopped) |
| `docker images` | List local images |
| `docker build -t myapp:v1 .` | Build image from Dockerfile |
| `docker pull nginx:alpine` | Pull image from registry |
| `docker push myrepo/myapp:v1` | Push image to registry |
| `docker exec -it web bash` | Execute interactive shell in running container |
| `docker logs -f web` | Follow container logs |
| `docker inspect web` | Detailed JSON info about container |
| `docker stop web && docker rm web` | Stop and remove container |
| `docker volume create mydata` | Create a named volume |
| `docker network create mynet` | Create a network |
| `docker system prune -a` | Remove all unused data |
| `docker swarm init` | Initialize swarm mode |
| `docker service create --name web nginx` | Create a swarm service |
| `docker stack deploy -c compose.yml mystack` | Deploy stack from compose file |
| `docker secret create dbpass secret.txt` | Create a swarm secret |
| `docker node ls` | List swarm nodes |
| `docker service ls` | List swarm services |
| `docker service logs web` | View service logs |
| `docker tag myapp:v1 myrepo/myapp:latest` | Tag an image |
| `docker login` | Authenticate to registry |
| `docker save -o image.tar myapp:v1` | Export image to tar |
| `docker load -i image.tar` | Import image from tar |
| `docker export web > container.tar` | Export container filesystem |
| `docker import container.tar myimage:v1` | Import container as image |
| `docker stats` | Live resource usage |
| `docker top web` | Running processes in container |
| `docker diff web` | Filesystem changes in container |
| `docker commit web myimage:snapshot` | Create image from container |
| `docker history myapp:v1` | Show image layer history |
| `docker network inspect bridge` | Inspect network details |
| `docker volume inspect mydata` | Inspect volume details |
| `docker info` | System-wide Docker information |
| `docker version` | Docker client/server versions |
| `docker compose up -d` | Start Compose services (v2) |
| `docker compose down -v` | Stop and remove Compose resources |
| `docker trust sign myrepo/myapp:v1` | Sign an image (DCT) |
| `docker trust inspect myrepo/myapp` | Inspect image signatures |
| `docker container prune` | Remove all stopped containers |
| `docker image prune -a` | Remove all unused images |

### Dockerfile Quick Reference

| Instruction | Purpose | Example |
|---|---|---|
| `FROM` | Base image | `FROM node:20-alpine` |
| `RUN` | Execute command during build | `RUN apt-get update && apt-get install -y curl` |
| `COPY` | Copy files from build context | `COPY . /app` |
| `ADD` | Copy + extract archives / URLs | `ADD app.tar.gz /app` |
| `WORKDIR` | Set working directory | `WORKDIR /app` |
| `CMD` | Default command (overridable) | `CMD ["node", "server.js"]` |
| `ENTRYPOINT` | Fixed command (args appended) | `ENTRYPOINT ["python"]` |
| `EXPOSE` | Document listening port | `EXPOSE 8080` |
| `ENV` | Set environment variable | `ENV NODE_ENV=production` |
| `ARG` | Build-time variable | `ARG VERSION=1.0` |
| `VOLUME` | Create mount point | `VOLUME /data` |
| `USER` | Set runtime user | `USER appuser` |
| `HEALTHCHECK` | Container health probe | `HEALTHCHECK CMD curl -f http://localhost/` |
| `LABEL` | Metadata key-value | `LABEL version="1.0"` |
| `STOPSIGNAL` | Signal to stop container | `STOPSIGNAL SIGTERM` |
| `SHELL` | Override default shell | `SHELL ["/bin/bash", "-c"]` |

### Key Facts Table

| Topic | Must-Know Fact |
|---|---|
| **Swarm ports** | 2377 (cluster mgmt), 7946 (node comms), 4789 (overlay/VXLAN) |
| **Manager quorum** | (N/2)+1 managers must be healthy; 3 managers tolerate 1 failure |
| **Raft consensus** | Swarm uses Raft; odd number of managers recommended (3, 5, 7) |
| **Max managers** | 7 recommended (more = slower consensus) |
| **Service modes** | `replicated` (default) vs `global` (one task per node) |
| **Default network** | `bridge` (standalone) / `ingress` + `docker_gwbridge` (swarm) |
| **Overlay** | Multi-host networking for swarm services |
| **Content Trust** | `DOCKER_CONTENT_TRUST=1` enforces image signing |
| **Storage drivers** | `overlay2` (recommended), `devicemapper`, `btrfs`, `zfs` |
| **Logging default** | `json-file` driver |
| **CMD vs ENTRYPOINT** | CMD = overridable default; ENTRYPOINT = fixed executable |
| **COPY vs ADD** | COPY is preferred; ADD auto-extracts tars and supports URLs |
| **Layer caching** | Each Dockerfile instruction = 1 layer; order matters for cache |
| **Volumes** | Managed by Docker; persist beyond container lifecycle |
| **Bind mounts** | Map host path directly; not managed by Docker |
| **tmpfs** | In-memory only; never written to disk |
| **Secrets** | Mounted at `/run/secrets/<name>` in containers |
| **Configs** | Mounted as files; not encrypted (unlike secrets) |
| **docker save/load** | Image → tar → image (preserves layers) |
| **docker export/import** | Container FS → tar → flat image (loses layers/metadata) |

---

## Domain 1 — Orchestration (25 %)

### What the Exam Tests Here

This is the **heaviest domain**. The exam tests your ability to initialize and manage a Swarm cluster, create and scale services, deploy stacks, perform rolling updates, and manage secrets/configs. Expect many CLI-based questions.

### Concept Map

```
Orchestration
├── Swarm Mode
│   ├── docker swarm init / join
│   ├── Manager vs Worker nodes
│   ├── Raft consensus & quorum
│   └── Node management (promote, demote, drain)
├── Services
│   ├── Replicated vs Global
│   ├── Create, update, scale, remove
│   ├── Placement constraints
│   └── Rolling updates & rollbacks
├── Stacks
│   ├── docker stack deploy
│   ├── Compose file (version 3+)
│   └── Stack services, networks, volumes
├── Secrets & Configs
│   ├── docker secret create / inspect
│   ├── docker config create / inspect
│   └── Granting to services
└── Troubleshooting
    ├── docker service ps / logs
    ├── Task states
    └── Node availability
```

---

### 1.1 Swarm Mode Basics

**What it is:** Docker's built-in container orchestration. Turns a group of Docker hosts into a single virtual host.

**Why it matters:** ~25 % of the exam. You must know how to init, join, manage nodes, and understand quorum.

**Initialize a swarm:**

```bash
# On the first manager node
docker swarm init --advertise-addr 192.168.1.10
```

This outputs a join token for workers and a separate one for managers.

**Join as a worker:**

```bash
docker swarm join --token SWMTKN-1-xxxxx 192.168.1.10:2377
```

**Join as a manager:**

```bash
# Get the manager join token
docker swarm join-token manager

# On the new manager node
docker swarm join --token SWMTKN-1-yyyyy 192.168.1.10:2377
```

**List nodes:**

```bash
docker node ls
# ID          HOSTNAME   STATUS   AVAILABILITY   MANAGER STATUS
# abc123 *   manager1   Ready    Active         Leader
# def456     worker1    Ready    Active
# ghi789     manager2   Ready    Active         Reachable
```

**Promote / demote nodes:**

```bash
docker node promote worker1      # worker → manager
docker node demote manager2      # manager → worker
```

**Drain a node (maintenance):**

```bash
docker node update --availability drain worker1
# Tasks are rescheduled to other nodes
# Restore with:
docker node update --availability active worker1
```

**Leave the swarm:**

```bash
# From the node
docker swarm leave           # worker
docker swarm leave --force   # manager (forced)
```

**Quorum rules:**

| Managers | Quorum (majority) | Fault tolerance |
|---|---|---|
| 1 | 1 | 0 |
| 2 | 2 | 0 |
| 3 | 2 | 1 |
| 5 | 3 | 2 |
| 7 | 4 | 3 |

**Pitfalls:**
- Always use an **odd** number of managers (3, 5, or 7).
- More than **7 managers** hurts performance — no benefit.
- Port **2377** must be open between all manager nodes.
- Port **7946** (TCP/UDP) for node discovery; **4789** (UDP) for overlay.
- If quorum is lost, the swarm cannot schedule new tasks until recovered.

**Mini-Quiz:**

1. What port is used for swarm cluster management? → **2377**
2. How many managers can fail in a 5-manager swarm? → **2** (quorum = 3)
3. What command drains a node? → **`docker node update --availability drain <node>`**

---

### 1.2 Services vs Containers

**What it is:** A **service** is the swarm-level abstraction. A **container** (task) is one instance of a service running on a node.

| Aspect | Container (`docker run`) | Service (`docker service create`) |
|---|---|---|
| Scope | Single host | Across swarm |
| Restart | Manual or `--restart` | Automatic by orchestrator |
| Scaling | N/A | `docker service scale` |
| Load balancing | Manual | Built-in (routing mesh) |
| Rolling updates | N/A | Built-in |

**Create a service:**

```bash
docker service create \
  --name web \
  --replicas 3 \
  --publish 80:80 \
  nginx:alpine
```

**List services:**

```bash
docker service ls
```

**Inspect a service:**

```bash
docker service inspect --pretty web
```

**View service tasks:**

```bash
docker service ps web
```

**Remove a service:**

```bash
docker service rm web
```

**Global service (one per node):**

```bash
docker service create \
  --name monitor \
  --mode global \
  datadog/agent
```

**Pitfalls:**
- `docker run` ≠ `docker service create` — services are swarm-only.
- A "task" is a slot; a "container" fills the task slot.
- If a task fails, the orchestrator schedules a **new** container (not restart the old one).

**Mini-Quiz:**

1. What is the difference between replicated and global mode? → Replicated runs N copies; global runs one per node.
2. What command shows individual tasks of a service? → **`docker service ps <service>`**
3. What happens when a service task's container crashes? → The orchestrator schedules a **new** container.

---

### 1.3 Stacks & Compose-to-Swarm

**What it is:** A **stack** is a group of services defined in a Compose file and deployed to a swarm.

**Deploy a stack:**

```bash
docker stack deploy -c docker-compose.yml mystack
```

**Example Compose file for swarm (version 3+):**

```yaml
version: "3.8"

services:
  web:
    image: nginx:alpine
    ports:
      - "80:80"
    deploy:
      replicas: 3
      update_config:
        parallelism: 1
        delay: 10s
      restart_policy:
        condition: on-failure
    networks:
      - frontend

  api:
    image: myapp/api:v1
    deploy:
      replicas: 2
      placement:
        constraints:
          - node.role == worker
    networks:
      - frontend
      - backend

  db:
    image: postgres:16
    volumes:
      - db-data:/var/lib/postgresql/data
    deploy:
      replicas: 1
      placement:
        constraints:
          - node.labels.storage == ssd
    networks:
      - backend
    secrets:
      - db_password

volumes:
  db-data:

networks:
  frontend:
  backend:

secrets:
  db_password:
    external: true
```

**Stack commands:**

```bash
docker stack ls                        # List stacks
docker stack services mystack          # List services in stack
docker stack ps mystack                # List tasks in stack
docker stack rm mystack                # Remove stack
```

**Pitfalls:**
- `docker stack deploy` only works in **swarm mode**.
- The `deploy:` key is **ignored** by `docker compose up` (standalone).
- `build:` is **ignored** by `docker stack deploy` — images must be pre-built.
- Stacks create an **overlay** network by default.
- Environment variable substitution works in stack files.

**Mini-Quiz:**

1. Can `docker stack deploy` build images? → **No** — images must be pre-built.
2. Is the `deploy:` key used by `docker compose up`? → **No** — it's swarm-only.
3. What is the default network type created by a stack? → **Overlay.**

---

### 1.4 Scaling & Replicas

**Scale a service:**

```bash
docker service scale web=5
# or
docker service update --replicas 5 web
```

**Scale multiple services:**

```bash
docker service scale web=5 api=3
```

**Pitfalls:**
- You cannot scale a **global** service — it automatically runs one task per node.
- Scaling down removes tasks from nodes based on the scheduler's decisions.
- The swarm load-balances requests across all replicas via the **routing mesh**.

**Mini-Quiz:**

1. Can you scale a global service with `docker service scale`? → **No.**
2. What distributes traffic across service replicas? → **Routing mesh.**

---

### 1.5 Placement Constraints & Node Labels

**Add a label to a node:**

```bash
docker node update --label-add env=production worker1
docker node update --label-add storage=ssd worker2
```

**Use placement constraints:**

```bash
docker service create \
  --name db \
  --constraint 'node.labels.storage == ssd' \
  --constraint 'node.role == worker' \
  postgres:16
```

**In a compose/stack file:**

```yaml
deploy:
  placement:
    constraints:
      - node.labels.env == production
      - node.role == manager
    preferences:
      - spread: node.labels.datacenter
```

**Available constraint fields:**

| Field | Examples |
|---|---|
| `node.id` | Specific node ID |
| `node.hostname` | `node.hostname == web-server-01` |
| `node.role` | `manager` or `worker` |
| `node.platform.os` | `linux` or `windows` |
| `node.platform.arch` | `x86_64`, `aarch64` |
| `node.labels.<key>` | Custom labels |
| `engine.labels.<key>` | Docker engine labels |

**Pitfalls:**
- Constraints use `==` and `!=` operators only.
- `node.labels` are set with `docker node update`; `engine.labels` are set in `daemon.json`.
- If no node matches the constraints, the task stays in **pending** state.

**Mini-Quiz:**

1. Where are `node.labels` set? → **`docker node update --label-add`**
2. What happens if no node matches a placement constraint? → Task stays **pending**.

---

### 1.6 Rolling Updates & Rollbacks

**Update a service image:**

```bash
docker service update --image nginx:1.25 web
```

**Configure update behavior:**

```bash
docker service create \
  --name web \
  --replicas 6 \
  --update-parallelism 2 \
  --update-delay 10s \
  --update-failure-action rollback \
  --update-max-failure-ratio 0.25 \
  --update-order start-first \
  nginx:1.24
```

| Parameter | Meaning |
|---|---|
| `--update-parallelism` | Tasks updated simultaneously |
| `--update-delay` | Wait between batches |
| `--update-failure-action` | `pause` (default), `continue`, or `rollback` |
| `--update-max-failure-ratio` | Tolerable failure ratio (0.0–1.0) |
| `--update-order` | `stop-first` (default) or `start-first` |

**Rollback a service:**

```bash
docker service rollback web
# or
docker service update --rollback web
```

**Configure rollback behavior:**

```bash
docker service update \
  --rollback-parallelism 1 \
  --rollback-delay 5s \
  --rollback-failure-action pause \
  web
```

**In compose file:**

```yaml
deploy:
  update_config:
    parallelism: 2
    delay: 10s
    failure_action: rollback
    order: start-first
  rollback_config:
    parallelism: 1
    delay: 5s
```

**Pitfalls:**
- Default `update-order` is `stop-first` — old container stops before new one starts (brief downtime).
- `start-first` starts the new container before stopping the old one (requires enough resources for both).
- Default `failure-action` is `pause` — not rollback.
- A rollback reverts to the **previous** spec, not the original spec.

**Mini-Quiz:**

1. What is the default `--update-failure-action`? → **`pause`**
2. What does `--update-order start-first` do? → Starts new task before stopping old one.
3. What does `docker service rollback` revert to? → The **previous** service spec.

---

### 1.7 Secrets & Configs

**Secrets — encrypted data for services:**

```bash
# Create from file
echo "MyS3cretP@ss" | docker secret create db_password -

# Create from file
docker secret create ssl_cert ./server.crt

# List secrets
docker secret ls

# Inspect (metadata only, not the value)
docker secret inspect db_password
```

**Grant a secret to a service:**

```bash
docker service create \
  --name db \
  --secret db_password \
  postgres:16
```

Inside the container, the secret is available at `/run/secrets/db_password`.

**Custom target path:**

```bash
docker service update \
  --secret-add source=db_password,target=/etc/myapp/db_pass,uid=1000,gid=1000,mode=0400 \
  db
```

**Configs — non-sensitive configuration data:**

```bash
# Create a config
docker config create nginx_conf ./nginx.conf

# Use in service
docker service create \
  --name web \
  --config source=nginx_conf,target=/etc/nginx/nginx.conf \
  nginx:alpine
```

**Differences between secrets and configs:**

| Aspect | Secrets | Configs |
|---|---|---|
| Encryption | Encrypted at rest and in transit | Not encrypted |
| Mount location | `/run/secrets/<name>` | Customizable (default `/`) |
| RAM-only | Yes (tmpfs mount) | No (regular file) |
| Use case | Passwords, keys, tokens | Config files, non-sensitive |

**Pitfalls:**
- Secrets and configs are **immutable** — you cannot update them in place. Create a new version, then rotate the service.
- Secrets are only available to **swarm services**, not standalone containers.
- To "update" a secret: create new secret → update service to use new secret → remove old secret.
- Configs can be mounted at any path (including replacing existing files).

**Rotating a secret:**

```bash
echo "NewP@ss" | docker secret create db_password_v2 -
docker service update \
  --secret-rm db_password \
  --secret-add source=db_password_v2,target=db_password \
  db
docker secret rm db_password
```

**Mini-Quiz:**

1. Where are secrets mounted inside a container? → **`/run/secrets/<name>`**
2. Are secrets encrypted at rest? → **Yes.**
3. Can you update a secret in place? → **No** — create a new one and rotate.

---

### 1.8 Troubleshooting Services

**View tasks and their states:**

```bash
docker service ps web --no-trunc
```

**Task states:**

| State | Meaning |
|---|---|
| `new` | Task initialized |
| `pending` | Waiting for slot/resources |
| `assigned` | Assigned to a node |
| `accepted` | Node accepted the task |
| `preparing` | Pulling image, setting up |
| `starting` | Container starting |
| `running` | Container running |
| `complete` | Container exited 0 |
| `failed` | Container exited non-zero |
| `shutdown` | Requested to shut down |
| `rejected` | Node rejected the task |
| `orphaned` | Node unavailable too long |

**View service logs:**

```bash
docker service logs web
docker service logs --follow --tail 100 web
docker service logs --since 5m web
```

**Inspect why a task failed:**

```bash
docker inspect <task_id>
# Look at "Status.Err" field
```

**Common issues:**

| Symptom | Likely Cause |
|---|---|
| Tasks stuck in `pending` | No node matches constraints / insufficient resources |
| Tasks restarting in loop | Container crashes on startup / bad image |
| Service 0/N replicas | Image pull failure / auth issue |
| Network unreachable | Overlay ports blocked (4789, 7946) |

**Mini-Quiz:**

1. What does `pending` state mean? → Waiting for a node with matching resources/constraints.
2. How do you view logs for a swarm service? → **`docker service logs <service>`**

---

## Domain 2 — Image Creation, Management & Registry (20 %)

### What the Exam Tests Here

Build efficient images, understand layer caching, multistage builds, Dockerfile best practices, and manage images with registries.

### Concept Map

```
Images & Registry
├── Dockerfile
│   ├── Instructions (FROM, RUN, COPY, CMD, ENTRYPOINT, etc.)
│   ├── Layer caching & order optimization
│   ├── Multistage builds
│   └── .dockerignore
├── Image Management
│   ├── Build, tag, push, pull
│   ├── Inspect layers (docker history)
│   ├── Save/load vs export/import
│   └── Prune unused images
└── Registry
    ├── Docker Hub
    ├── Private registry (docker registry)
    ├── Login / authentication
    └── Image signing (DTR / DCT)
```

---

### 2.1 Dockerfile Instructions

**Basic Dockerfile:**

```dockerfile
FROM node:20-alpine
WORKDIR /app
COPY package*.json ./
RUN npm ci --production
COPY . .
EXPOSE 3000
USER node
CMD ["node", "server.js"]
```

**CMD vs ENTRYPOINT:**

```dockerfile
# CMD — default command, overridable by docker run args
CMD ["npm", "start"]

# ENTRYPOINT — fixed executable, args appended
ENTRYPOINT ["python"]
CMD ["app.py"]       # default argument; docker run myimg test.py overrides CMD only
```

| | `docker run myimg` | `docker run myimg script.py` |
|---|---|---|
| CMD only | `npm start` | `script.py` (CMD replaced) |
| ENTRYPOINT + CMD | `python app.py` | `python script.py` (CMD replaced) |
| ENTRYPOINT only | `python` | `python script.py` |

**Shell form vs Exec form:**

```dockerfile
# Exec form (preferred) — no shell, signal handling works
CMD ["node", "server.js"]
ENTRYPOINT ["python", "app.py"]

# Shell form — runs in /bin/sh -c, PID 1 is the shell
CMD node server.js
ENTRYPOINT python app.py
```

**COPY vs ADD:**

```dockerfile
# COPY — straightforward copy (preferred)
COPY ./src /app/src

# ADD — also extracts archives and supports URLs (use sparingly)
ADD app.tar.gz /app/
ADD https://example.com/file.txt /app/
```

**HEALTHCHECK:**

```dockerfile
HEALTHCHECK --interval=30s --timeout=5s --start-period=10s --retries=3 \
  CMD curl -f http://localhost:3000/health || exit 1
```

**ARG vs ENV:**

```dockerfile
ARG NODE_VERSION=20          # build-time only
FROM node:${NODE_VERSION}-alpine
ENV NODE_ENV=production       # persists in container runtime
```

**VOLUME:**

```dockerfile
VOLUME /data                  # creates anonymous volume mount point
```

**USER:**

```dockerfile
RUN addgroup -S appgroup && adduser -S appuser -G appgroup
USER appuser
```

**LABEL:**

```dockerfile
LABEL maintainer="team@example.com"
LABEL version="1.0"
LABEL description="My application"
```

**STOPSIGNAL:**

```dockerfile
STOPSIGNAL SIGQUIT            # override default SIGTERM
```

**ONBUILD:**

```dockerfile
ONBUILD COPY . /app
ONBUILD RUN npm install
# Triggers run when another image uses this as a base (FROM)
```

**Pitfalls:**
- Each `RUN`, `COPY`, `ADD` creates a **new layer**; chain `RUN` commands to reduce layers.
- `CMD` only supports **one** entry — last `CMD` wins.
- `EXPOSE` does **not** publish the port — it's documentation; use `-p` at runtime.
- `VOLUME` in Dockerfile creates an **anonymous** volume — named volumes require runtime `-v`.
- `ARG` values are **not** available after the `FROM` line (unless redeclared).

**Dockerfile — optimized layer caching:**

```dockerfile
FROM node:20-alpine
WORKDIR /app

# Layer 1: Dependencies (cached if package files unchanged)
COPY package.json package-lock.json ./
RUN npm ci --production

# Layer 2: App code (invalidated on every code change)
COPY . .

EXPOSE 3000
CMD ["node", "server.js"]
```

**Mini-Quiz:**

1. Can you override `ENTRYPOINT` at runtime? → **Yes**, with `docker run --entrypoint`.
2. Does `EXPOSE` publish the port? → **No** — use `-p` flag at runtime.
3. What's the difference between exec and shell form? → Exec form runs directly (PID 1); shell form wraps in `/bin/sh -c`.

---

### 2.2 Build Options & Multistage Builds

**Build with options:**

```bash
docker build -t myapp:v1 .
docker build -t myapp:v1 -f Dockerfile.prod .
docker build --no-cache -t myapp:v1 .
docker build --build-arg VERSION=2.0 -t myapp:v2 .
docker build --target builder -t myapp:builder .
```

**Multistage build — Go application:**

```dockerfile
# Stage 1: Build
FROM golang:1.22 AS builder
WORKDIR /src
COPY go.mod go.sum ./
RUN go mod download
COPY . .
RUN CGO_ENABLED=0 go build -o /app

# Stage 2: Runtime (minimal image)
FROM alpine:3.19
RUN apk add --no-cache ca-certificates
COPY --from=builder /app /app
EXPOSE 8080
ENTRYPOINT ["/app"]
```

**Multistage build — Node.js application:**

```dockerfile
# Stage 1: Build
FROM node:20-alpine AS build
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build

# Stage 2: Production
FROM node:20-alpine
WORKDIR /app
COPY --from=build /app/dist ./dist
COPY --from=build /app/node_modules ./node_modules
COPY package*.json ./
EXPOSE 3000
USER node
CMD ["node", "dist/server.js"]
```

**Multistage — Python:**

```dockerfile
FROM python:3.12-slim AS builder
WORKDIR /app
COPY requirements.txt .
RUN pip install --user --no-cache-dir -r requirements.txt

FROM python:3.12-slim
WORKDIR /app
COPY --from=builder /root/.local /root/.local
COPY . .
ENV PATH=/root/.local/bin:$PATH
EXPOSE 8000
CMD ["gunicorn", "app:app", "--bind", "0.0.0.0:8000"]
```

**Multistage — Java (Maven):**

```dockerfile
FROM maven:3.9-eclipse-temurin-21 AS build
WORKDIR /app
COPY pom.xml .
RUN mvn dependency:go-offline
COPY src ./src
RUN mvn package -DskipTests

FROM eclipse-temurin:21-jre-alpine
COPY --from=build /app/target/*.jar /app/app.jar
EXPOSE 8080
ENTRYPOINT ["java", "-jar", "/app/app.jar"]
```

**Pitfalls:**
- Multistage builds produce a **smaller final image** — only the last stage is tagged.
- `COPY --from=<stage>` copies from a named build stage.
- `--target <stage>` builds **up to and including** that stage only.
- Build cache is **invalidated** when any layer above changes — order instructions from least to most changing.
- `.dockerignore` prevents unnecessary files from entering the build context.

**.dockerignore example:**

```
node_modules
.git
*.md
docker-compose*.yml
.env
```

**Mini-Quiz:**

1. What is the main benefit of multistage builds? → **Smaller final image** (build tools not included).
2. How do you copy from a previous stage? → **`COPY --from=<stage_name> ...`**
3. Does `--no-cache` ignore the build cache? → **Yes.**

---

### 2.3 Tagging, Pushing & Pulling

**Tagging:**

```bash
docker tag myapp:v1 myregistry.com/myapp:v1
docker tag myapp:v1 myregistry.com/myapp:latest
```

**Pushing:**

```bash
docker login myregistry.com
docker push myregistry.com/myapp:v1
docker push myregistry.com/myapp:latest
```

**Pulling:**

```bash
docker pull nginx:alpine
docker pull myregistry.com/myapp:v1
```

**Image naming format:**

```
[registry/][namespace/]repository[:tag|@digest]

Examples:
  nginx                          → docker.io/library/nginx:latest
  myuser/myapp:v2                → docker.io/myuser/myapp:v2
  gcr.io/project/app:v1          → gcr.io/project/app:v1
  myapp@sha256:abc123...         → by digest (immutable)
```

**Pitfalls:**
- If no tag is specified, Docker defaults to `:latest`.
- `:latest` is just a tag name — it doesn't necessarily mean "most recent."
- Pulling by **digest** (`@sha256:...`) is immutable — most secure.

**Mini-Quiz:**

1. What is the default tag if none is specified? → **`latest`**
2. Does `:latest` guarantee the most recent version? → **No** — it's just a tag name.

---

### 2.4 Private Registry

**Run a private registry:**

```bash
docker run -d \
  --name registry \
  -p 5000:5000 \
  -v registry-data:/var/lib/registry \
  registry:2
```

**Push to private registry:**

```bash
docker tag myapp:v1 localhost:5000/myapp:v1
docker push localhost:5000/myapp:v1
```

**Pull from private registry:**

```bash
docker pull localhost:5000/myapp:v1
```

**TLS-secured registry:**

```bash
docker run -d \
  --name registry \
  -p 443:443 \
  -v /certs:/certs \
  -e REGISTRY_HTTP_ADDR=0.0.0.0:443 \
  -e REGISTRY_HTTP_TLS_CERTIFICATE=/certs/domain.crt \
  -e REGISTRY_HTTP_TLS_KEY=/certs/domain.key \
  registry:2
```

**Insecure registry (for testing only):**

Add to `/etc/docker/daemon.json`:
```json
{
  "insecure-registries": ["myregistry.com:5000"]
}
```

**Registry API:**

```bash
# List repositories
curl http://localhost:5000/v2/_catalog

# List tags for a repo
curl http://localhost:5000/v2/myapp/tags/list
```

**Pitfalls:**
- Docker Hub is the **default** registry if none is specified.
- Private registries need TLS or must be added to `insecure-registries`.
- `docker login` stores credentials in `~/.docker/config.json`.
- For swarm services, all nodes must be able to pull from the registry.

**Mini-Quiz:**

1. What is the default port for Docker registry? → **5000**
2. Where are Docker login credentials stored? → **`~/.docker/config.json`**

---

### 2.5 Inspecting & Pruning Images

**Inspect image layers:**

```bash
docker history myapp:v1
docker history --no-trunc myapp:v1
```

**Inspect image metadata:**

```bash
docker inspect myapp:v1
docker inspect --format '{{.Config.Cmd}}' myapp:v1
docker inspect --format '{{.Config.Entrypoint}}' myapp:v1
```

**Image size:**

```bash
docker images myapp
docker system df              # disk usage overview
docker system df -v           # detailed
```

**Prune images:**

```bash
docker image prune            # remove dangling (untagged) images
docker image prune -a         # remove ALL unused images
docker system prune           # remove unused containers, networks, images
docker system prune -a --volumes  # remove everything including volumes
```

**Save & Load (preserves layers):**

```bash
docker save -o myapp.tar myapp:v1
docker load -i myapp.tar
```

**Export & Import (flat filesystem):**

```bash
docker export mycontainer > container.tar
docker import container.tar myimage:flat
```

| | `save / load` | `export / import` |
|---|---|---|
| Input | Image | Container |
| Preserves layers | Yes | No (flat) |
| Preserves metadata | Yes | No |
| Use case | Transfer images | Snapshot container FS |

**Mini-Quiz:**

1. What's the difference between `docker save` and `docker export`? → `save` works on images (preserves layers); `export` works on containers (flat FS).
2. What does `docker image prune` remove? → **Dangling (untagged) images.**
3. What does `docker system prune -a` do? → Removes all unused containers, networks, images, and optionally volumes.

---

## Domain 3 — Installation & Configuration (15 %)

### What the Exam Tests Here

Install Docker, configure the daemon, understand storage drivers, logging, certificates, namespaces, and cgroups.

### Concept Map

```
Installation & Configuration
├── Installation
│   ├── Linux (apt/yum)
│   ├── Windows (Docker Desktop / Server)
│   └── Post-install steps
├── Daemon Configuration
│   ├── daemon.json
│   ├── Storage drivers (overlay2)
│   ├── Root directory
│   └── Remote access (TLS)
├── Logging
│   ├── Logging drivers (json-file, syslog, journald, etc.)
│   ├── Per-container vs daemon default
│   └── Log rotation
├── Service Management
│   ├── systemctl start/stop/enable docker
│   └── Boot startup
├── TLS / Certificate Auth
│   ├── CA, server cert, client cert
│   └── Protecting the Docker socket
└── Kernel Features
    ├── Namespaces (pid, net, mnt, uts, ipc, user)
    └── Cgroups (resource limits)
```

---

### 3.1 Installing Docker Engine

**Ubuntu/Debian:**

```bash
# Update and install prerequisites
sudo apt-get update
sudo apt-get install -y ca-certificates curl gnupg

# Add Docker GPG key
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

# Add Docker repo
echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] \
  https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# Install
sudo apt-get update
sudo apt-get install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

# Post-install: add user to docker group
sudo usermod -aG docker $USER
```

**CentOS/RHEL:**

```bash
sudo yum install -y yum-utils
sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
sudo yum install -y docker-ce docker-ce-cli containerd.io
sudo systemctl start docker
sudo systemctl enable docker
```

**Verify installation:**

```bash
docker version
docker info
docker run hello-world
```

**Pitfalls:**
- The `docker` group grants **root-equivalent** privileges.
- On RHEL/CentOS, you may need to disable `firewalld` or configure it for Docker.
- Docker Desktop (Windows/Mac) includes Docker Engine, CLI, and Compose.

---

### 3.2 Daemon Configuration

**`/etc/docker/daemon.json`:**

```json
{
  "storage-driver": "overlay2",
  "data-root": "/var/lib/docker",
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "10m",
    "max-file": "3"
  },
  "default-address-pools": [
    { "base": "172.20.0.0/16", "size": 24 }
  ],
  "insecure-registries": [],
  "registry-mirrors": ["https://mirror.example.com"],
  "live-restore": true,
  "debug": false,
  "tls": true,
  "tlscacert": "/etc/docker/ca.pem",
  "tlscert": "/etc/docker/server-cert.pem",
  "tlskey": "/etc/docker/server-key.pem",
  "hosts": ["unix:///var/run/docker.sock", "tcp://0.0.0.0:2376"]
}
```

**Key settings:**

| Setting | Purpose |
|---|---|
| `storage-driver` | Filesystem driver (`overlay2` recommended) |
| `data-root` | Where Docker stores images/containers (`/var/lib/docker`) |
| `log-driver` | Default logging driver |
| `live-restore` | Keep containers running during daemon downtime |
| `debug` | Enable debug mode |
| `insecure-registries` | Allow HTTP registries |
| `registry-mirrors` | Pull-through cache registries |
| `default-address-pools` | IP ranges for bridge networks |

**Storage drivers:**

| Driver | Filesystem | Recommended? |
|---|---|---|
| `overlay2` | xfs, ext4 | ✅ Default/recommended |
| `devicemapper` | direct-lvm | Legacy |
| `btrfs` | btrfs | Specialized |
| `zfs` | zfs | Specialized |
| `vfs` | Any | Testing only (no CoW) |

**Restart daemon after config change:**

```bash
sudo systemctl daemon-reload
sudo systemctl restart docker
```

**Pitfalls:**
- `daemon.json` must be **valid JSON** — a syntax error prevents Docker from starting.
- `live-restore: true` lets containers keep running during Docker daemon restarts (not upgrades).
- `overlay2` requires a kernel ≥ 4.0 and xfs with `d_type=true`.
- **Never** expose the Docker socket over TCP without TLS.

**Mini-Quiz:**

1. What is the recommended storage driver? → **`overlay2`**
2. Where is the daemon config file? → **`/etc/docker/daemon.json`**
3. What does `live-restore` do? → Keeps containers running when the daemon restarts.

---

### 3.3 Logging Drivers

**Available logging drivers:**

| Driver | Description |
|---|---|
| `json-file` | Default; writes JSON logs to disk |
| `syslog` | Sends to syslog daemon |
| `journald` | Sends to systemd journal |
| `gelf` | Graylog Extended Log Format |
| `fluentd` | Sends to Fluentd |
| `awslogs` | Amazon CloudWatch |
| `splunk` | Splunk HTTP Event Collector |
| `none` | No logging |

**Set default in daemon.json:**

```json
{
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "10m",
    "max-file": "5"
  }
}
```

**Per-container override:**

```bash
docker run -d --log-driver syslog --log-opt syslog-address=udp://logserver:514 nginx
```

**Pitfalls:**
- `docker logs` only works with `json-file` and `journald` drivers.
- If using a remote logging driver, `docker logs` returns nothing.
- Log rotation requires `max-size` and `max-file` options.

**Mini-Quiz:**

1. What is the default logging driver? → **`json-file`**
2. Can `docker logs` read from all logging drivers? → **No** — only `json-file` and `journald`.

---

### 3.4 Managing Docker Service

```bash
sudo systemctl start docker       # Start
sudo systemctl stop docker        # Stop
sudo systemctl restart docker     # Restart
sudo systemctl enable docker      # Start on boot
sudo systemctl disable docker     # Disable on boot
sudo systemctl status docker      # Check status
```

**Check Docker info:**

```bash
docker info                        # Full system info
docker version                     # Version details
```

---

### 3.5 Certificate-Based Auth

**Protect the Docker daemon with TLS:**

Required files:
- `ca.pem` — Certificate Authority
- `server-cert.pem` + `server-key.pem` — Server certificate/key
- `cert.pem` + `key.pem` — Client certificate/key

**Server configuration (`daemon.json`):**

```json
{
  "tls": true,
  "tlsverify": true,
  "tlscacert": "/etc/docker/ca.pem",
  "tlscert": "/etc/docker/server-cert.pem",
  "tlskey": "/etc/docker/server-key.pem",
  "hosts": ["unix:///var/run/docker.sock", "tcp://0.0.0.0:2376"]
}
```

**Client connection:**

```bash
docker --tlsverify \
  --tlscacert=ca.pem \
  --tlscert=cert.pem \
  --tlskey=key.pem \
  -H=tcp://dockerhost:2376 \
  info
```

**Or use `DOCKER_HOST` and `DOCKER_TLS_VERIFY`:**

```bash
export DOCKER_HOST=tcp://dockerhost:2376
export DOCKER_TLS_VERIFY=1
export DOCKER_CERT_PATH=~/.docker
docker info
```

**Ports:**
- **2375** — unencrypted (NEVER use in production)
- **2376** — TLS encrypted

**Pitfalls:**
- Without `tlsverify`, any client with network access can control your Docker daemon.
- Client certs go in `~/.docker/` by default.

**Mini-Quiz:**

1. What port is used for TLS-secured Docker daemon? → **2376**
2. What does `tlsverify` enforce? → Mutual TLS — both client and server must present certificates.

---

### 3.6 Namespaces & Cgroups

**Linux namespaces (container isolation):**

| Namespace | Isolates |
|---|---|
| `pid` | Process IDs |
| `net` | Network interfaces, routing, iptables |
| `mnt` | Filesystem mount points |
| `uts` | Hostname and domain name |
| `ipc` | Inter-process communication |
| `user` | User and group IDs |
| `cgroup` | Cgroup root directory |

**Cgroups (resource limits):**

```bash
docker run -d \
  --name app \
  --cpus 2 \
  --memory 512m \
  --memory-swap 1g \
  --pids-limit 100 \
  myapp:v1
```

| Flag | Purpose |
|---|---|
| `--cpus` | CPU limit (e.g., `1.5` = 1.5 cores) |
| `--memory` / `-m` | Memory limit |
| `--memory-swap` | Memory + swap limit |
| `--memory-reservation` | Soft memory limit |
| `--pids-limit` | Max processes |
| `--cpu-shares` | Relative CPU weight (default 1024) |
| `--cpuset-cpus` | Pin to specific CPUs (`0,1`) |

**Pitfalls:**
- Without `--memory`, a container can consume **all** host memory.
- `--memory-swap` must be ≥ `--memory` (or `-1` for unlimited swap).
- Namespaces provide **isolation**; cgroups provide **resource limits**.

**Mini-Quiz:**

1. What do namespaces provide? → **Isolation.**
2. What do cgroups provide? → **Resource limits.**
3. What happens without `--memory` flag? → Container can use **all** available host memory.

---

## Domain 4 — Networking (15 %)

### What the Exam Tests Here

Docker network drivers, bridge vs overlay vs host vs macvlan, port publishing, DNS resolution, service discovery, and multi-host networking.

### Concept Map

```
Networking
├── Drivers
│   ├── bridge (default, single host)
│   ├── host (share host network)
│   ├── overlay (multi-host, swarm)
│   ├── macvlan (physical LAN appearance)
│   └── none (no networking)
├── Port Publishing
│   ├── -p host:container
│   ├── Routing mesh (swarm)
│   └── Host mode publishing
├── DNS
│   ├── Embedded DNS server (user-defined networks)
│   ├── Container name resolution
│   └── Service name resolution (swarm)
└── Troubleshooting
    ├── docker network inspect
    ├── docker network connect/disconnect
    └── Overlay port requirements
```

---

### 4.1 Network Drivers Overview

| Driver | Scope | Use Case |
|---|---|---|
| `bridge` | Single host | Default for standalone containers |
| `host` | Single host | Container shares host networking (no isolation) |
| `overlay` | Multi-host | Swarm services across nodes |
| `macvlan` | Single host | Container appears as physical device on LAN |
| `none` | N/A | No networking |

**List networks:**

```bash
docker network ls
```

**Create a network:**

```bash
docker network create mybridge                        # bridge (default)
docker network create -d overlay myoverlay            # overlay (swarm)
docker network create -d macvlan \
  --subnet=192.168.1.0/24 \
  --gateway=192.168.1.1 \
  -o parent=eth0 \
  mymacvlan
```

---

### 4.2 Bridge Networks

**Default bridge (`docker0`):**

```bash
# Containers on default bridge communicate by IP only (no DNS)
docker run -d --name c1 nginx
docker run -d --name c2 nginx
# c1 and c2 CANNOT resolve each other by name on default bridge
```

**User-defined bridge (recommended):**

```bash
docker network create mynet

docker run -d --name web --network mynet nginx
docker run -d --name api --network mynet myapp

# web and api CAN resolve each other by name
docker exec api ping web     # works!
```

**Key differences — default vs user-defined bridge:**

| Feature | Default bridge | User-defined bridge |
|---|---|---|
| DNS resolution | ❌ (IP only) | ✅ (container name) |
| Automatic connection | ✅ | ❌ (must specify) |
| Isolation | Shared | Better isolation |
| Live connect/disconnect | ❌ | ✅ |

**Connect/disconnect containers:**

```bash
docker network connect mynet existing-container
docker network disconnect mynet existing-container
```

**Pitfalls:**
- Default bridge does **NOT** provide DNS — use user-defined bridges.
- Containers on different user-defined bridges are **isolated** unless connected to both.
- `--link` is **legacy** — use user-defined networks instead.

**Mini-Quiz:**

1. Does the default bridge provide DNS resolution? → **No.**
2. How do you enable container name resolution? → Use a **user-defined bridge** network.

---

### 4.3 Overlay Networks

**What it is:** Enables communication between containers on **different Docker hosts** in a swarm.

**Create an overlay network:**

```bash
docker network create -d overlay my-overlay
```

**Attachable overlay (standalone containers can join):**

```bash
docker network create -d overlay --attachable my-overlay
```

**Use in a service:**

```bash
docker service create \
  --name web \
  --network my-overlay \
  --replicas 3 \
  nginx
```

**Required ports for overlay:**
- **4789/UDP** — VXLAN data plane
- **7946/TCP+UDP** — Control plane (gossip)
- **2377/TCP** — Cluster management (managers only)

**Encrypt overlay traffic:**

```bash
docker network create -d overlay --opt encrypted my-secure-overlay
```

**Pitfalls:**
- Overlay networks are created on **managers** and lazily distributed to **workers** when a service task is assigned.
- Non-attachable overlays can only be used by **swarm services**, not standalone containers.
- `--opt encrypted` adds IPsec encryption (performance cost).
- Overlay uses **VXLAN** encapsulation.

**Mini-Quiz:**

1. What protocol does overlay use? → **VXLAN (port 4789/UDP).**
2. Can standalone containers join an overlay? → **Only if `--attachable` is set.**
3. How do you encrypt overlay traffic? → **`--opt encrypted`**

---

### 4.4 Host & Macvlan

**Host networking:**

```bash
docker run -d --network host nginx
# nginx binds directly to host's port 80 — no port mapping needed
```

- No network isolation — container shares host's network namespace.
- No NAT overhead — best performance.
- Port conflicts with host services possible.
- **Not supported on Docker Desktop** (macOS/Windows).

**Macvlan:**

```bash
docker network create -d macvlan \
  --subnet=192.168.1.0/24 \
  --gateway=192.168.1.1 \
  -o parent=eth0 \
  my-macvlan

docker run -d --network my-macvlan --ip 192.168.1.100 nginx
```

- Container gets its own **MAC address** on the physical network.
- Appears as a physical device to the network.
- No port mapping needed — full network stack.
- Requires promiscuous mode on the host NIC.

**Mini-Quiz:**

1. Does host networking provide port isolation? → **No.**
2. What does macvlan give each container? → Its own **MAC address** on the physical network.

---

### 4.5 Publishing Ports & DNS

**Port publishing modes:**

```bash
# Map host port to container port
docker run -d -p 8080:80 nginx                  # host 8080 → container 80
docker run -d -p 80:80 -p 443:443 nginx         # multiple ports
docker run -d -p 127.0.0.1:8080:80 nginx        # bind to specific interface
docker run -d -P nginx                           # publish all EXPOSE'd ports (random host ports)
```

**Swarm port publishing:**

```bash
# Routing mesh mode (default) — all nodes listen on published port
docker service create --name web -p 80:80 nginx

# Host mode — only nodes running the task listen
docker service create --name web \
  --publish published=80,target=80,mode=host \
  nginx
```

**DNS resolution in user-defined networks:**

```bash
# Embedded DNS server at 127.0.0.11
docker run --network mynet myapp
# Inside container: nslookup web → resolves to container IP
```

**Pitfalls:**
- Routing mesh publishes the port on **every** swarm node, even those without tasks.
- `host` mode in swarm requires `--mode global` or `max_replicas_per_node: 1` to avoid port conflicts.
- `docker run -P` assigns **random** high ports for each `EXPOSE` port.

---

### 4.6 Service Discovery

**Swarm built-in DNS:**

- Services are discoverable by **service name**.
- DNS round-robin resolves to all healthy task IPs.
- **VIP (Virtual IP)** mode (default): single virtual IP load-balances to tasks.
- **DNSRR** mode: DNS returns all task IPs directly.

```bash
# VIP mode (default)
docker service create --name api --network my-overlay myapp

# DNSRR mode
docker service create --name api --network my-overlay --endpoint-mode dnsrr myapp
```

| Mode | How it works |
|---|---|
| **VIP** (default) | Single virtual IP → iptables load-balances to tasks |
| **DNSRR** | DNS returns all task IPs → client-side load balancing |

**Mini-Quiz:**

1. What is the default endpoint mode for swarm services? → **VIP.**
2. What IP does the embedded DNS server use? → **127.0.0.11**

---

## Domain 5 — Security (15 %)

### What the Exam Tests Here

Docker Content Trust, user namespaces, Linux capabilities, secrets, and security best practices.

### Concept Map

```
Security
├── Docker Content Trust (DCT)
│   ├── Image signing & verification
│   ├── DOCKER_CONTENT_TRUST=1
│   └── Notary
├── User Namespaces
│   ├── Remap container root to unprivileged host user
│   └── userns-remap in daemon.json
├── Capabilities
│   ├── --cap-add / --cap-drop
│   ├── --privileged (all capabilities)
│   └── Minimal capability sets
├── Container Hardening
│   ├── Read-only filesystem
│   ├── No-new-privileges
│   ├── Seccomp profiles
│   └── AppArmor / SELinux
├── Secrets (Swarm)
│   └── (covered in 1.7)
└── Best Practices
    ├── Non-root USER in Dockerfile
    ├── Minimal base images
    ├── Scan images for vulnerabilities
    └── Principle of least privilege
```

---

### 5.1 Docker Content Trust

**What it is:** Ensures image integrity and publisher authenticity using cryptographic signing.

**Enable DCT:**

```bash
export DOCKER_CONTENT_TRUST=1
```

With DCT enabled:
- `docker pull` only pulls **signed** images.
- `docker push` **signs** the image before pushing.
- Unsigned images are **rejected**.

**Sign an image:**

```bash
export DOCKER_CONTENT_TRUST=1
docker push myrepo/myapp:v1
# First push prompts for root key and repository key passphrases
```

**Inspect trust data:**

```bash
docker trust inspect --pretty myrepo/myapp
```

**Sign with delegation:**

```bash
docker trust signer add --key cert.pem myname myrepo/myapp
docker trust sign myrepo/myapp:v1
```

**Revoke trust:**

```bash
docker trust revoke myrepo/myapp:v1
```

**Key hierarchy:**
- **Root key** — offline, protects all other keys (KEEP SAFE).
- **Repository key** — per-repo signing key.
- **Delegation keys** — delegated signing authority.

**Pitfalls:**
- The **root key** must be stored **offline** securely — if lost, you cannot rotate other keys.
- DCT is enforced on the **client side** — the daemon doesn't enforce it.
- DCT works with Docker Hub and Docker Trusted Registry (DTR) / private registries with Notary.
- `DOCKER_CONTENT_TRUST=0` disables it (default).

**Mini-Quiz:**

1. What env var enables Docker Content Trust? → **`DOCKER_CONTENT_TRUST=1`**
2. Is DCT enforced on the client or server? → **Client.**
3. What key must be stored offline? → **Root key.**

---

### 5.2 User Namespaces & Capabilities

**User namespaces — remap container root to unprivileged host user:**

```json
// /etc/docker/daemon.json
{
  "userns-remap": "default"
}
```

This creates a `dockremap` user/group and maps container UID 0 to an unprivileged host UID.

**Verify:**

```bash
# Inside container
id           # uid=0(root)

# On host, the process runs as high UID
ps aux | grep <container_process>   # e.g., UID 100000
```

**Linux capabilities:**

```bash
# Drop all, add only what's needed
docker run --cap-drop ALL --cap-add NET_BIND_SERVICE nginx

# Common capabilities
# NET_BIND_SERVICE — bind to ports < 1024
# SYS_PTRACE       — debugging
# CHOWN             — change file ownership
# SETUID/SETGID    — change UID/GID
```

**Drop dangerous capabilities:**

```bash
docker run --cap-drop SYS_ADMIN --cap-drop NET_RAW myapp
```

**Privileged mode (use with extreme caution):**

```bash
docker run --privileged myapp
# Grants ALL capabilities + access to all devices
# NEVER use in production unless absolutely necessary
```

**Pitfalls:**
- `--privileged` gives the container **full host access** — security nightmare.
- User namespaces may break volume permissions (container root ≠ host root).
- Not all applications work with all capabilities dropped.

**Mini-Quiz:**

1. What does `userns-remap` do? → Maps container root (UID 0) to an unprivileged host UID.
2. What does `--privileged` grant? → **All** Linux capabilities and device access.
3. How do you drop all capabilities? → **`--cap-drop ALL`**

---

### 5.3 Securing Containers

**Read-only filesystem:**

```bash
docker run --read-only --tmpfs /tmp myapp
```

**No new privileges:**

```bash
docker run --security-opt no-new-privileges myapp
# Prevents gaining additional privileges via setuid/setgid binaries
```

**Seccomp profile:**

```bash
# Use default profile (blocks ~44 syscalls)
docker run myapp   # default seccomp applied automatically

# Custom profile
docker run --security-opt seccomp=custom-profile.json myapp

# Disable seccomp (NOT recommended)
docker run --security-opt seccomp=unconfined myapp
```

**AppArmor:**

```bash
docker run --security-opt apparmor=docker-default myapp
```

**PID limits:**

```bash
docker run --pids-limit 50 myapp    # prevent fork bombs
```

**Pitfalls:**
- `--read-only` prevents writing to the container filesystem — use `--tmpfs` for writable temp directories.
- Default seccomp profile blocks dangerous syscalls like `mount`, `reboot`, `kexec_load`.
- `no-new-privileges` is critical for preventing privilege escalation.

---

### 5.4 Secrets Management in Swarm

*(Covered in detail in [1.7 Secrets & Configs](#17-secrets--configs))*

**Key security points:**
- Secrets are encrypted at rest (Raft log) and in transit.
- Mounted as **tmpfs** (RAM-only) at `/run/secrets/<name>`.
- Only accessible by services explicitly granted access.
- Cannot be read via `docker secret inspect` (metadata only).

---

### 5.5 Security Best Practices

1. **Use non-root USER** in Dockerfile:
   ```dockerfile
   RUN adduser -D appuser
   USER appuser
   ```

2. **Use minimal base images** (`alpine`, `distroless`, `slim`):
   ```dockerfile
   FROM gcr.io/distroless/static-debian12
   ```

3. **Scan images for vulnerabilities:**
   ```bash
   docker scout cves myapp:v1
   # or use Trivy, Snyk, etc.
   ```

4. **Pin base image versions:**
   ```dockerfile
   FROM node:20.11-alpine3.19    # specific, not :latest
   ```

5. **Don't store secrets in images:**
   ```dockerfile
   # BAD
   ENV API_KEY=secret123
   
   # GOOD — use runtime secrets/env vars
   ```

6. **Use `.dockerignore`** to exclude sensitive files from build context.

7. **Limit container resources** (`--memory`, `--cpus`, `--pids-limit`).

8. **Use read-only containers** when possible.

9. **Enable Docker Content Trust** for image verification.

10. **Keep Docker up to date** — security patches.

**Mini-Quiz:**

1. Why use `distroless` or `alpine`? → Smaller attack surface, fewer vulnerabilities.
2. Should you store secrets in `ENV` instructions? → **No** — they're baked into image layers.

---

## Domain 6 — Storage & Volumes (10 %)

### What the Exam Tests Here

Understand the three storage options (volumes, bind mounts, tmpfs), volume drivers, data persistence, and backup strategies.

### Concept Map

```
Storage & Volumes
├── Storage Types
│   ├── Volumes (managed by Docker)
│   ├── Bind mounts (host path)
│   └── tmpfs (in-memory)
├── Volume Management
│   ├── docker volume create / ls / inspect / rm
│   ├── Named vs anonymous volumes
│   └── Volume drivers (local, NFS, cloud)
├── Persistence
│   ├── Data survives container removal (volumes)
│   ├── Shared data between containers
│   └── VOLUME instruction in Dockerfile
└── Backup & Restore
    ├── docker run --volumes-from
    ├── tar-based backup
    └── Volume migration
```

---

### 6.1 Bind Mounts vs Volumes vs tmpfs

| Feature | Volumes | Bind Mounts | tmpfs |
|---|---|---|---|
| Managed by Docker | ✅ | ❌ | ❌ |
| Host path required | ❌ | ✅ | ❌ |
| Portable | ✅ | ❌ | ❌ |
| Persists after container removal | ✅ | ✅ (host file) | ❌ |
| Performance | Good | Host-dependent | Fastest (RAM) |
| Use case | Databases, app data | Development (code sharing) | Sensitive temp data |
| Swarm support | ✅ | ❌ (node-local) | ✅ |

**Volume:**

```bash
# Create and use a named volume
docker volume create mydata
docker run -d -v mydata:/app/data myapp

# Or use --mount (more explicit)
docker run -d --mount source=mydata,target=/app/data myapp
```

**Bind mount:**

```bash
# Mount host directory into container
docker run -d -v /host/path:/container/path myapp

# Read-only bind mount
docker run -d -v /host/path:/container/path:ro myapp

# --mount syntax (explicit)
docker run -d --mount type=bind,source=/host/path,target=/container/path myapp
```

**tmpfs:**

```bash
docker run -d --tmpfs /tmp myapp

# --mount syntax
docker run -d --mount type=tmpfs,target=/tmp,tmpfs-size=100m myapp
```

**`-v` vs `--mount`:**

| Feature | `-v` / `--volume` | `--mount` |
|---|---|---|
| Syntax | `src:dst[:opts]` | `key=value` pairs |
| Create missing dir | ✅ (auto-creates) | ❌ (error if missing for bind) |
| Clarity | Less explicit | More explicit |
| Recommended for | Quick usage | Production / Swarm |

**Pitfalls:**
- Bind mounts with `-v` **auto-create** missing host directories; `--mount` for bind mounts throws an error.
- Volumes persist data even after `docker rm` — you must explicitly `docker volume rm`.
- Anonymous volumes (no name) are harder to manage — always use named volumes.
- `tmpfs` data is **lost** when the container stops.
- Bind mounts don't work in swarm mode across nodes (data is node-local).

**Mini-Quiz:**

1. Which storage type is managed by Docker? → **Volumes.**
2. Does data in tmpfs persist after container stop? → **No.**
3. Can bind mounts be used across swarm nodes? → **No** — they're node-local.

---

### 6.2 Volume Drivers & Plugins

**Default driver (local):**

```bash
docker volume create --driver local mydata
```

**NFS volume:**

```bash
docker volume create \
  --driver local \
  --opt type=nfs \
  --opt o=addr=nfsserver.example.com,rw \
  --opt device=:/path/to/share \
  nfs-data
```

**Third-party volume drivers:**

| Driver | Storage Backend |
|---|---|
| `local` | Local filesystem (default) |
| `rexray` | AWS EBS, Azure Disk, etc. |
| `portworx` | Distributed storage |
| `flocker` | Multi-host volumes (deprecated) |
| `convoy` | Snapshot-based backups |

**Use in compose:**

```yaml
volumes:
  db-data:
    driver: local
    driver_opts:
      type: nfs
      o: addr=nfsserver.example.com,rw
      device: ":/data/db"
```

---

### 6.3 Persisting Data

**Named volume for database:**

```bash
docker volume create pgdata

docker run -d \
  --name postgres \
  -v pgdata:/var/lib/postgresql/data \
  -e POSTGRES_PASSWORD=secret \
  postgres:16
```

**Share volume between containers:**

```bash
docker run -d --name writer -v shared:/data mywriter
docker run -d --name reader -v shared:/data:ro myreader
```

**VOLUME instruction in Dockerfile:**

```dockerfile
VOLUME /data
# Creates an anonymous volume at /data
# Best practice: use named volumes at runtime instead
```

**Inspect a volume:**

```bash
docker volume inspect mydata
# Shows: mountpoint, driver, labels, etc.
```

**List and clean volumes:**

```bash
docker volume ls
docker volume prune              # remove unused volumes
docker volume rm mydata          # remove specific volume
```

---

### 6.4 Backup & Restore

**Backup a volume:**

```bash
docker run --rm \
  -v mydata:/source:ro \
  -v $(pwd):/backup \
  alpine \
  tar czf /backup/mydata-backup.tar.gz -C /source .
```

**Restore a volume:**

```bash
docker volume create mydata-restored

docker run --rm \
  -v mydata-restored:/target \
  -v $(pwd):/backup:ro \
  alpine \
  tar xzf /backup/mydata-backup.tar.gz -C /target
```

**Copy from container (alternative):**

```bash
docker cp mycontainer:/data ./backup/
docker cp ./backup/ newcontainer:/data
```

**`--volumes-from` (share volumes from another container):**

```bash
docker run -d --name db -v dbdata:/var/lib/mysql mysql:8

# Backup container using volumes from db
docker run --rm --volumes-from db -v $(pwd):/backup alpine \
  tar czf /backup/db-backup.tar.gz /var/lib/mysql
```

**Pitfalls:**
- `docker volume prune` removes **all unused** volumes — data loss if not careful.
- `docker rm -v container` removes anonymous volumes with the container.
- Named volumes are NOT removed with `docker rm` (only anonymous ones with `-v`).
- Always test restore procedures — not just backups.

**Mini-Quiz:**

1. How do you back up a Docker volume? → Mount it in a temp container and `tar` it.
2. Does `docker rm` remove named volumes? → **No** — only anonymous volumes with `-v` flag.
3. What does `--volumes-from` do? → Mounts all volumes from another container.

---

## Scenario Playbook (6 Scenarios)

### Scenario 1 — Deploy a 3-Node Swarm & Scale Services

**Requirement:** Initialize a 3-node swarm (1 manager, 2 workers), deploy nginx with 5 replicas.

```bash
# On manager node
docker swarm init --advertise-addr 192.168.1.10

# On worker nodes (use token from init output)
docker swarm join --token SWMTKN-1-xxxxx 192.168.1.10:2377

# Verify nodes
docker node ls

# Deploy service
docker service create \
  --name web \
  --replicas 5 \
  --publish 80:80 \
  --update-parallelism 2 \
  --update-delay 10s \
  nginx:alpine

# Verify
docker service ps web

# Scale up
docker service scale web=10

# Drain a node for maintenance
docker node update --availability drain worker2

# Restore
docker node update --availability active worker2
```

---

### Scenario 2 — Multi-Stage Dockerfile for a Go App

**Requirement:** Build a Go web app with the smallest possible production image.

```dockerfile
# Build stage
FROM golang:1.22-alpine AS builder
RUN apk add --no-cache git
WORKDIR /src
COPY go.mod go.sum ./
RUN go mod download
COPY . .
RUN CGO_ENABLED=0 GOOS=linux go build -ldflags="-s -w" -o /app ./cmd/server

# Production stage
FROM scratch
COPY --from=builder /etc/ssl/certs/ca-certificates.crt /etc/ssl/certs/
COPY --from=builder /app /app
EXPOSE 8080
ENTRYPOINT ["/app"]
```

```bash
docker build -t mygo-app:v1 .
docker images mygo-app   # ~ 10 MB
docker run -d -p 8080:8080 mygo-app:v1
```

**Why `scratch`?** No OS, no shell, no package manager = smallest possible image with minimal attack surface.

---

### Scenario 3 — Overlay Networking for Multi-Service Apps

**Requirement:** Deploy a web + API + database stack with proper network isolation.

```yaml
# docker-compose.yml
version: "3.8"
services:
  web:
    image: nginx:alpine
    ports:
      - "80:80"
    networks:
      - frontend
    deploy:
      replicas: 3

  api:
    image: myapp/api:v1
    networks:
      - frontend
      - backend
    deploy:
      replicas: 2

  db:
    image: postgres:16
    volumes:
      - db-data:/var/lib/postgresql/data
    networks:
      - backend
    deploy:
      replicas: 1
      placement:
        constraints:
          - node.labels.storage == ssd
    secrets:
      - db_password

volumes:
  db-data:

networks:
  frontend:
    driver: overlay
  backend:
    driver: overlay
    internal: true       # no external access

secrets:
  db_password:
    external: true
```

```bash
docker stack deploy -c docker-compose.yml myapp
```

**Explanation:** `web` can reach `api` via `frontend`. `api` can reach `db` via `backend`. `web` **cannot** reach `db` directly. `backend` is `internal: true` — no outbound internet.

---

### Scenario 4 — Secure Images with Docker Content Trust

**Requirement:** Enforce that only signed images are pulled and pushed.

```bash
# Enable DCT
export DOCKER_CONTENT_TRUST=1

# Tag and push (signing occurs automatically)
docker tag myapp:v1 myregistry.com/myapp:v1
docker push myregistry.com/myapp:v1
# Prompted for root key passphrase (first time) and repo key passphrase

# Pull (only signed images accepted)
docker pull myregistry.com/myapp:v1

# Verify signatures
docker trust inspect --pretty myregistry.com/myapp

# Try pulling unsigned image → FAILS
docker pull someuser/unsigned-image:latest
# Error: remote trust data does not exist

# Temporarily disable DCT for specific pull
DOCKER_CONTENT_TRUST=0 docker pull someuser/unsigned-image:latest
```

---

### Scenario 5 — Persist App Data Across Containers

**Requirement:** Run a MySQL database with persistent storage that survives container recreation.

```bash
# Create named volume
docker volume create mysql-data

# Run MySQL with volume
docker run -d \
  --name mysql \
  -v mysql-data:/var/lib/mysql \
  -e MYSQL_ROOT_PASSWORD=secret \
  -e MYSQL_DATABASE=myapp \
  -p 3306:3306 \
  mysql:8

# Verify data directory
docker volume inspect mysql-data

# Stop and remove container
docker stop mysql && docker rm mysql

# Recreate — data persists!
docker run -d \
  --name mysql \
  -v mysql-data:/var/lib/mysql \
  -e MYSQL_ROOT_PASSWORD=secret \
  -p 3306:3306 \
  mysql:8

# Backup
docker run --rm \
  -v mysql-data:/source:ro \
  -v $(pwd):/backup \
  alpine tar czf /backup/mysql-backup.tar.gz -C /source .
```

---

### Scenario 6 — Troubleshoot Unreachable Containers on Overlay Network

**Requirement:** Services on an overlay network cannot communicate. Diagnose and fix.

```bash
# Step 1: Check network exists
docker network ls | grep my-overlay

# Step 2: Inspect network — verify services are connected
docker network inspect my-overlay

# Step 3: Check service tasks are running
docker service ps web
docker service ps api

# Step 4: Check required ports are open between nodes
# 2377/TCP — cluster management
# 7946/TCP+UDP — node communication
# 4789/UDP — overlay data (VXLAN)
# On each node:
sudo ss -tulnp | grep -E '2377|7946|4789'

# Step 5: Check firewall rules
sudo iptables -L -n | grep -E '2377|7946|4789'

# Step 6: Test DNS resolution from within a service container
docker exec <task_container> nslookup api
docker exec <task_container> ping api

# Step 7: If network is corrupted, recreate
docker network rm my-overlay
docker network create -d overlay my-overlay
docker service update --network-add my-overlay web
docker service update --network-add my-overlay api

# Step 8: Check for IP address exhaustion
docker network inspect my-overlay --format '{{json .IPAM}}'
```

**Common causes:**
1. Firewall blocking ports 4789, 7946.
2. Service not attached to the overlay network.
3. Container DNS not resolving (try embedded DNS 127.0.0.11).
4. Network subnet conflict with host network.

---

## Practice Exam (55 Questions)

### Multiple Choice Questions

**Q1.** What port is used for Docker Swarm cluster management?  
A) 4789  
B) 7946  
C) 2377  
D) 2376  

**Q2.** What is the recommended storage driver for Docker?  
A) devicemapper  
B) aufs  
C) overlay2  
D) btrfs  

**Q3.** Which Dockerfile instruction creates a new layer?  
A) `CMD`  
B) `EXPOSE`  
C) `RUN`  
D) `ENV`  

**Q4.** How many managers can fail in a 7-manager swarm and still maintain quorum?  
A) 2  
B) 3  
C) 4  
D) 1  

**Q5.** What does `DOCKER_CONTENT_TRUST=1` enforce?  
A) Image encryption  
B) Image signing and verification  
C) Registry authentication  
D) Container isolation  

**Q6.** Where are Docker secrets mounted inside a container?  
A) `/etc/secrets/<name>`  
B) `/var/secrets/<name>`  
C) `/run/secrets/<name>`  
D) `/tmp/secrets/<name>`  

**Q7.** Which network driver enables multi-host communication?  
A) bridge  
B) host  
C) overlay  
D) macvlan  

**Q8.** What is the default service mode in Docker Swarm?  
A) global  
B) replicated  
C) host  
D) distributed  

**Q9.** What command rolls back a service to its previous configuration?  
A) `docker service revert`  
B) `docker service rollback`  
C) `docker service undo`  
D) `docker service restore`  

**Q10.** Which storage type is in-memory only?  
A) Volumes  
B) Bind mounts  
C) tmpfs  
D) NFS  

**Q11.** What is the difference between `CMD` and `ENTRYPOINT`?  
A) CMD cannot be overridden  
B) ENTRYPOINT is overridable, CMD is not  
C) CMD provides defaults that can be overridden; ENTRYPOINT is the fixed executable  
D) They are identical  

**Q12.** What does `docker system prune -a` remove?  
A) Only stopped containers  
B) All unused containers, networks, and images  
C) Running containers  
D) Docker configuration files  

**Q13.** Which flag grants a container ALL Linux capabilities?  
A) `--cap-add ALL`  
B) `--capabilities`  
C) `--privileged`  
D) `--full-access`  

**Q14.** What is the default logging driver?  
A) syslog  
B) journald  
C) json-file  
D) fluentd  

**Q15.** Can `docker logs` work with the `syslog` logging driver?  
A) Yes  
B) No  
C) Only in swarm mode  
D) Only with `--follow`  

**Q16.** What does `--userns-remap` do?  
A) Maps container ports to host ports  
B) Remaps container root UID to unprivileged host UID  
C) Remaps container filesystems  
D) Enables network namespaces  

**Q17.** Which command shows image layer history?  
A) `docker inspect`  
B) `docker layers`  
C) `docker history`  
D) `docker image ls`  

**Q18.** What happens when you push with `DOCKER_CONTENT_TRUST=1`?  
A) Image is encrypted  
B) Image is compressed  
C) Image is signed  
D) Image is cached  

**Q19.** What is the difference between `docker save` and `docker export`?  
A) `save` is for containers; `export` is for images  
B) `save` preserves layers; `export` creates flat filesystem  
C) They are identical  
D) `export` preserves layers; `save` creates flat filesystem  

**Q20.** Which Dockerfile instruction is preferred: `COPY` or `ADD`?  
A) `ADD`  
B) `COPY` (unless you need auto-extraction or URL support)  
C) They are interchangeable  
D) Neither — use `RUN wget`  

**Q21.** What does the `--read-only` flag do?  
A) Makes the container image read-only  
B) Makes the container filesystem read-only  
C) Makes volumes read-only  
D) Makes the network read-only  

**Q22.** Which port is used for overlay VXLAN data?  
A) 2377/TCP  
B) 7946/TCP  
C) 4789/UDP  
D) 443/TCP  

**Q23.** What does `docker node update --availability drain` do?  
A) Removes the node from the swarm  
B) Stops Docker on the node  
C) Reschedules tasks away from the node  
D) Deletes all containers on the node  

**Q24.** How do you create an overlay network accessible to standalone containers?  
A) `docker network create -d overlay mynet`  
B) `docker network create -d overlay --attachable mynet`  
C) `docker network create -d overlay --standalone mynet`  
D) Not possible  

**Q25.** What is the default `--update-failure-action` for swarm services?  
A) rollback  
B) continue  
C) pause  
D) stop  

### Scenario-Based Questions

**Q26.** You have a 5-manager swarm. Two managers fail simultaneously. Can the swarm still schedule tasks?  
A) No — quorum is lost  
B) Yes — 3 of 5 is a majority  
C) Only if `force-new-cluster` is used  
D) Only for global services  

**Q27.** A developer builds an image but the final image is 2 GB. What technique most effectively reduces image size?  
A) Use `docker image prune`  
B) Use multistage builds  
C) Add more `RUN` layers  
D) Use `VOLUME` instruction  

**Q28.** A container running `myapp` has `ENTRYPOINT ["python"]` and `CMD ["app.py"]`. What happens when you run `docker run myapp test.py`?  
A) `python app.py test.py`  
B) `python test.py`  
C) `test.py`  
D) Error  

**Q29.** A service task shows `pending` state. What is the most likely cause?  
A) The image is being pulled  
B) No node matches the placement constraints  
C) The service is being rolled back  
D) The container is starting  

**Q30.** You need to update a swarm secret. What is the correct procedure?  
A) `docker secret update dbpass --value newpass`  
B) Create a new secret, update the service to use it, remove the old secret  
C) Edit the secret in place  
D) Restart the service  

**Q31.** Which command restricts a container to 512 MB of memory and 1.5 CPU cores?  
A) `docker run --memory 512m --cpus 1.5 myapp`  
B) `docker run --mem 512 --cpu 1.5 myapp`  
C) `docker run --resources memory=512m,cpu=1.5 myapp`  
D) `docker run --limit-memory 512m --limit-cpu 1.5 myapp`  

**Q32.** On the default bridge network, container A cannot resolve container B by name. Why?  
A) DNS is disabled globally  
B) Default bridge does not provide DNS resolution — use a user-defined bridge  
C) Container B is not running  
D) Port mapping is required  

**Q33.** You deploy a stack and the `deploy:` key is ignored. What went wrong?  
A) You used `docker compose up` instead of `docker stack deploy`  
B) The YAML version is wrong  
C) The swarm is not initialized  
D) Both A and C could be the issue  

**Q34.** A Docker daemon won't start after editing `daemon.json`. What is the most likely cause?  
A) Permission denied  
B) Invalid JSON syntax  
C) Docker service is disabled  
D) Port conflict  

**Q35.** You want containers to appear as physical devices on the LAN with their own MAC addresses. Which network driver?  
A) bridge  
B) overlay  
C) macvlan  
D) host  

**Q36.** Which flag prevents a fork bomb inside a container?  
A) `--memory`  
B) `--pids-limit`  
C) `--cap-drop ALL`  
D) `--read-only`  

**Q37.** `docker run -P nginx` — what happens?  
A) Port 80 is published on host port 80  
B) All `EXPOSE`d ports are published on random host ports  
C) No ports are published  
D) Error — `-P` is invalid  

**Q38.** How do you encrypt overlay network traffic?  
A) `--tls` flag  
B) `--opt encrypted`  
C) Enable DCT  
D) Use macvlan instead  

**Q39.** What does the `internal: true` option on a Docker network do?  
A) Makes it accessible only to swarm managers  
B) Disables external (outbound) connectivity  
C) Encrypts all traffic  
D) Limits to a single node  

**Q40.** A Dockerfile has `ARG VERSION=1.0` before `FROM node:${VERSION}`. After `FROM`, is `VERSION` still available?  
A) Yes  
B) No — ARGs before FROM are only for the FROM instruction; redeclare after FROM to use  
C) Only in RUN commands  
D) Only if ENV is also set  

### CLI-Based Questions

**Q41.** Write the command to create a swarm service named `api` with 4 replicas, publishing port 8080, using image `myapp:v2`.

**Q42.** Write the command to add a label `zone=us-east` to node `worker3`.

**Q43.** Write the command to update service `web` to use image `nginx:1.25` with 2 tasks updated in parallel and a 15-second delay.

**Q44.** Write the command to create a Docker secret named `api_key` from a file `key.txt`.

**Q45.** Write the command to back up a volume named `appdata` to a tar file.

**Q46.** Write the command to build an image from `Dockerfile.prod` tagged as `myapp:production`.

**Q47.** Write the command to run a container with a read-only filesystem, limited to 256 MB memory, dropping all capabilities.

**Q48.** Write the command to create an attachable overlay network named `app-net`.

**Q49.** Write the command to inspect the logging configuration of a running container named `web`.

**Q50.** Write the command to remove all stopped containers, unused networks, and dangling images.

### Multi-Select Questions

**Q51.** Which THREE are valid Docker network drivers? *(Select 3)*  
A) bridge  
B) overlay  
C) vlan  
D) macvlan  
E) mesh  

**Q52.** Which TWO Dockerfile instructions create filesystem layers? *(Select 2)*  
A) `CMD`  
B) `COPY`  
C) `EXPOSE`  
D) `RUN`  
E) `ENV`  

**Q53.** Which THREE are valid methods to persist data in Docker? *(Select 3)*  
A) Volumes  
B) Bind mounts  
C) Container filesystem  
D) tmpfs  
E) Environment variables  

**Q54.** Which TWO are swarm-only features? *(Select 2)*  
A) Bridge networks  
B) Secrets  
C) Volumes  
D) Configs  
E) Port mapping  

**Q55.** Which THREE ports must be open for Docker Swarm? *(Select 3)*  
A) 2377/TCP  
B) 4789/UDP  
C) 8080/TCP  
D) 7946/TCP+UDP  
E) 443/TCP  

---

## Answer Key & Explanations

| # | Answer | Explanation |
|---|---|---|
| 1 | **C** | Port 2377/TCP is for swarm cluster management. |
| 2 | **C** | `overlay2` is the recommended and default storage driver. |
| 3 | **C** | `RUN` executes commands and creates a new filesystem layer. `CMD` and `EXPOSE` add metadata only. |
| 4 | **B** | 7 managers: quorum = 4, fault tolerance = 3. |
| 5 | **B** | DCT enforces cryptographic signing and verification of images. |
| 6 | **C** | Secrets are mounted at `/run/secrets/<name>`. |
| 7 | **C** | Overlay networks enable multi-host communication in swarm. |
| 8 | **B** | Default service mode is `replicated`. |
| 9 | **B** | `docker service rollback <service>` reverts to previous spec. |
| 10 | **C** | `tmpfs` is in-memory only — not written to disk. |
| 11 | **C** | CMD provides overridable defaults; ENTRYPOINT is the fixed executable. |
| 12 | **B** | Removes all unused containers, networks, and images (not running ones). |
| 13 | **C** | `--privileged` grants all capabilities and device access. |
| 14 | **C** | `json-file` is the default logging driver. |
| 15 | **B** | `docker logs` only works with `json-file` and `journald` drivers. |
| 16 | **B** | Remaps container UID 0 to an unprivileged host UID for security. |
| 17 | **C** | `docker history` shows the layer-by-layer build history. |
| 18 | **C** | DCT signs the image during push. |
| 19 | **B** | `save` works on images (preserves layers); `export` works on containers (flat). |
| 20 | **B** | `COPY` is preferred; `ADD` only when you need tar extraction or URL download. |
| 21 | **B** | Makes the container's root filesystem read-only. |
| 22 | **C** | VXLAN data plane uses port 4789/UDP. |
| 23 | **C** | `drain` reschedules all tasks to other available nodes. |
| 24 | **B** | `--attachable` allows standalone containers to join the overlay. |
| 25 | **C** | Default failure action is `pause` (not rollback). |
| 26 | **B** | 3 of 5 managers survive = majority maintained. Quorum intact. |
| 27 | **B** | Multistage builds separate build-time tools from the final runtime image. |
| 28 | **B** | `docker run` args replace `CMD`; ENTRYPOINT stays → `python test.py`. |
| 29 | **B** | `pending` usually means no node matches constraints or resource requirements. |
| 30 | **B** | Secrets are immutable. Create new → update service → remove old. |
| 31 | **A** | `--memory 512m --cpus 1.5` is the correct syntax. |
| 32 | **B** | Default bridge lacks embedded DNS. Use user-defined bridge for name resolution. |
| 33 | **D** | `docker compose up` ignores `deploy:`, and `docker stack deploy` requires swarm mode. |
| 34 | **B** | Invalid JSON in `daemon.json` is the most common cause of daemon startup failure. |
| 35 | **C** | Macvlan assigns real MAC addresses, making containers appear as physical devices. |
| 36 | **B** | `--pids-limit` caps the number of processes, preventing fork bombs. |
| 37 | **B** | `-P` publishes all `EXPOSE`d ports on random high host ports. |
| 38 | **B** | `--opt encrypted` enables IPsec encryption on overlay networks. |
| 39 | **B** | `internal: true` prevents containers on the network from accessing external networks. |
| 40 | **B** | ARGs declared before FROM are scoped only to the FROM instruction. Redeclare after FROM to use in subsequent instructions. |

### CLI Answers

| # | Answer |
|---|---|
| 41 | `docker service create --name api --replicas 4 --publish 8080:8080 myapp:v2` |
| 42 | `docker node update --label-add zone=us-east worker3` |
| 43 | `docker service update --image nginx:1.25 --update-parallelism 2 --update-delay 15s web` |
| 44 | `docker secret create api_key key.txt` |
| 45 | `docker run --rm -v appdata:/source:ro -v $(pwd):/backup alpine tar czf /backup/appdata-backup.tar.gz -C /source .` |
| 46 | `docker build -t myapp:production -f Dockerfile.prod .` |
| 47 | `docker run --read-only --memory 256m --cap-drop ALL myapp` |
| 48 | `docker network create -d overlay --attachable app-net` |
| 49 | `docker inspect --format '{{.HostConfig.LogConfig}}' web` |
| 50 | `docker system prune` |

### Multi-Select Answers

| # | Answer | Explanation |
|---|---|---|
| 51 | **A, B, D** | `bridge`, `overlay`, and `macvlan` are valid drivers. `vlan` and `mesh` are not Docker network driver names. |
| 52 | **B, D** | `COPY` and `RUN` create filesystem layers. `CMD`, `EXPOSE`, and `ENV` add metadata. |
| 53 | **A, B, D** | Volumes, bind mounts, and tmpfs are the three Docker storage mount types. Container filesystem is ephemeral; env vars are not storage. |
| 54 | **B, D** | Secrets and configs are swarm-only features. |
| 55 | **A, B, D** | 2377/TCP (management), 4789/UDP (VXLAN), 7946/TCP+UDP (gossip). |

---

## Final Revision Checklist

- [ ] I can initialize a swarm and join manager/worker nodes
- [ ] I understand quorum rules (odd managers, (N/2)+1)
- [ ] I know the difference between replicated and global services
- [ ] I can deploy a stack from a compose file (`docker stack deploy`)
- [ ] I understand rolling updates and rollback configuration
- [ ] I know how to create, grant, and rotate secrets and configs
- [ ] I can troubleshoot service tasks (`docker service ps`, states)
- [ ] I understand all major Dockerfile instructions
- [ ] I know CMD vs ENTRYPOINT and exec vs shell form
- [ ] I can write multistage Dockerfiles
- [ ] I understand layer caching and instruction ordering
- [ ] I can tag, push, pull, save, load, export, and import images
- [ ] I can set up and use a private registry
- [ ] I know the `daemon.json` structure and key settings
- [ ] I understand storage drivers (overlay2 recommended)
- [ ] I know all logging drivers and their `docker logs` compatibility
- [ ] I can configure TLS for the Docker daemon (ports 2375 vs 2376)
- [ ] I understand namespaces (isolation) and cgroups (resource limits)
- [ ] I know all network drivers: bridge, overlay, host, macvlan, none
- [ ] I understand default bridge vs user-defined bridge (DNS!)
- [ ] I can create and troubleshoot overlay networks
- [ ] I know the required swarm ports (2377, 7946, 4789)
- [ ] I understand routing mesh vs host-mode publishing
- [ ] I know VIP vs DNSRR endpoint modes
- [ ] I can enable and use Docker Content Trust
- [ ] I understand user namespaces and `userns-remap`
- [ ] I know Linux capabilities (`--cap-add`, `--cap-drop`, `--privileged`)
- [ ] I can harden containers (read-only, no-new-privileges, seccomp)
- [ ] I understand volumes, bind mounts, and tmpfs differences
- [ ] I can back up and restore Docker volumes
- [ ] I know `-v` vs `--mount` syntax differences
- [ ] I understand volume drivers and NFS volumes
- [ ] I can use resource limits (`--memory`, `--cpus`, `--pids-limit`)
- [ ] I've taken the practice exam and scored ≥ 65 %

---

> **Good luck on the DCA exam!** 🐳  
> Run every command, understand every flag, and trust your preparation.
