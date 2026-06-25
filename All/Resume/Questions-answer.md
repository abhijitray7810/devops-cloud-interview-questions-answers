# 🚀 Complete Interview Preparation Guide
## Abhijit Ray — DevOps / Cloud / Platform Engineer
 
--- 
 
## 📌 Table of Contents 

1. [Self Introduction](#self-introduction)
2. [HR & Behavioral Questions](#hr--behavioral-questions)
3. [Linux & Bash Scripting](#linux--bash-scripting)
4. [Networking Fundamentals](#networking-fundamentals) 
5. [Git & Version Control](#git--version-control) 
6. [Docker & Containerization](#docker--containerization) 
7. [Kubernetes (K8s)](#kubernetes-k8s)
8. [Terraform & IaC](#terraform--iac) 
9. [AWS Cloud](#aws-cloud)
10. [CI/CD — GitHub Actions, Jenkins, ArgoCD, Tekton](#cicd--github-actions-jenkins-argocd-tekton)
11. [Observability — Prometheus, Grafana, Loki](#observability--prometheus-grafana-loki)
12. [Security — Vault, OPA, Kyverno, Falco, Cosign](#security--vault-opa-kyverno-falco-cosign)
13. [Service Mesh — Istio](#service-mesh--istio)
14. [YAML Deep Dive](#yaml-deep-dive)
15. [Python for DevOps](#python-for-devops)
16. [System Design Questions](#system-design-questions)
17. [Scenario-Based Troubleshooting](#scenario-based-troubleshooting)
18. [Project-Based Discussion](#project-based-discussion)
19. [Coding & Scripting Challenges](#coding--scripting-challenges)
20. [Real-World Production Issues](#real-world-production-issues)
21. [STAR Method Answers](#star-method-answers)

---

## Self Introduction

### 🎯 Short Version (30 seconds — Phone Screen)

> "Hi, I'm Abhijit Ray, a Computer Science undergraduate from Sister Nivedita University, Kolkata, graduating in June 2026. I specialize in DevOps and cloud-native infrastructure, with hands-on experience in AWS, Kubernetes, Terraform, and CI/CD pipelines. I've independently built and shipped three production-grade projects — Aegis Stack, PodPlate, and RoutineOps — covering Kubernetes security, GitOps, and observability. I'm currently pursuing the CKA and AWS Solutions Architect certifications, and I'm excited to bring this real-world infrastructure experience to a professional environment."

---

### 🎯 Medium Version (2 minutes — Technical Round)

> "I'm Abhijit Ray, a final-year B.Tech Computer Science student from Sister Nivedita University, Kolkata, with a CGPA of 8.1.
>
> My core focus is DevOps, cloud infrastructure, and Kubernetes-native platforms. Over the past year, I've independently built three end-to-end cloud-native projects, all published on GitHub, that reflect real-world production scenarios.
>
> The first is **Aegis Stack**, a full infrastructure automation and security platform on AWS EKS. I provisioned everything — VPC, subnets, IAM, EKS — with Terraform, and layered in admission-control policies using OPA Gatekeeper and Kyverno. I also added a 7-stage CI/CD pipeline with image scanning using Trivy, SBOM generation via Syft, and image signing via Cosign. On the security side, I deployed Istio for mTLS, moved secrets into HashiCorp Vault, and set up Falco for runtime threat detection with alerts under 30 seconds.
>
> The second is **PodPlate**, a microservices platform on EKS with Istio circuit breakers that cut inter-service latency by 35%, a GitOps pipeline with ArgoCD and Tekton reducing deployment time to under 8 minutes, and full observability with Prometheus, Grafana, and Loki tracking 15+ SLIs.
>
> The third is **RoutineOps**, a DevOps automation platform where I built modular Terraform configs, containerized services with rolling updates and readiness probes achieving 99.9% uptime, and automated CI/CD with health-check rollbacks reducing outages by 60%.
>
> I hold certifications from AWS, KodeKloud, Cisco, Google Cloud, and Anthropic, and I'm actively working toward CKA and AWS Solutions Architect Associate.
>
> I'm looking for a role where I can apply this hands-on experience to real production systems, contribute to platform reliability, and grow within a team working on large-scale cloud infrastructure."

---

### 🎯 Long Version (3–4 minutes — Senior Panel / CTO Round)

> "Good [morning/afternoon]. I'm Abhijit Ray, finishing my B.Tech in Computer Science from Sister Nivedita University in June 2026. My academic background covers Computer Networks, Operating Systems, Distributed Systems, and Cloud Computing — but what defines me most is the practical work I've done independently in the DevOps and cloud-native space.
>
> I've built three complete infrastructure platforms from scratch, each addressing different production concerns:
>
> **Aegis Stack** is my most comprehensive project — an infrastructure automation and security platform on AWS EKS. The core premise was: how do you build a Kubernetes platform that's production-safe from Day 0? I used Terraform to provision a full AWS stack — VPC, subnets, IAM roles with least privilege, and EKS clusters — reducing setup time from days to under 20 minutes. I then focused heavily on security: OPA Gatekeeper and Kyverno enforce admission policies so non-compliant workloads never even get scheduled. The CI/CD pipeline has 7 stages including Trivy vulnerability scanning, SBOM generation using Syft, and cryptographic image signing with Cosign to prevent supply chain attacks. At runtime, Istio handles mTLS between all services, all secrets live in HashiCorp Vault — never in environment variables or ConfigMaps — and Falco detects anomalous container behavior with mean time to alert under 30 seconds.
>
> **PodPlate** tackled reliability and observability. I built a self-healing EKS platform where Istio circuit breakers prevent cascade failures, reducing inter-service latency by 35%. GitOps through ArgoCD and Tekton ensures every deployment is version-controlled and auditable, with deploy time dropping to under 8 minutes. I also built a complete observability stack tracking over 15 SLIs with Prometheus and Grafana dashboards, while Loki handles structured logs — cutting incident resolution time in half.
>
> **RoutineOps** was about automation at scale — modular Terraform for full AWS stacks with zero environment drift across dev, staging, and production, containerized workloads on Kubernetes with rolling deployments and readiness probes achieving 99.9% uptime, and GitHub Actions pipelines with automated rollback on health-check failure.
>
> On certifications: I've cleared AWS Well-Architected Proficient, KodeKloud's 100 Days of DevOps, Networking Basics from Cisco, Google Cloud's Gemini Enterprise App, and Anthropic's Claude 101. I'm actively working on CKA and AWS Solutions Architect Associate.
>
> My technical stack covers Python, Bash, YAML, HCL; AWS, Terraform, Kubernetes, Docker, Helm, Istio; GitHub Actions, Jenkins, ArgoCD, Tekton; and Prometheus, Grafana, Loki.
>
> I'm passionate about platform engineering — specifically the intersection of automation, security, and reliability. I want to work on systems where decisions matter, where I can contribute to reducing toil through automation, and where I can keep learning from experienced engineers in production environments."

---

## HR & Behavioral Questions

### Q1: Tell me about yourself.
> *Use the Self Introduction above based on the interview round.*

---

### Q2: Why do you want to work in DevOps?
**Answer:**
> "I've always been drawn to the operational side of software — not just writing code, but understanding how systems run, scale, and fail. DevOps sits at the intersection of development, infrastructure, and reliability, which is exactly where I want to be. My project work showed me how much impact automation, proper CI/CD, and observability have on system health. I want to work on platforms that thousands of users depend on, and that requires the mindset DevOps enforces — ownership from code commit to production."

---

### Q3: What is your biggest strength?
**Answer:**
> "My biggest strength is that I can take an ambiguous problem — like 'build a secure Kubernetes platform' — and independently research, design, and implement a working solution. I don't wait for step-by-step guidance. With Aegis Stack, I started from scratch, learned OPA, Kyverno, Cosign, Falco, and Vault by building with them, not just reading about them. I'm also a fast learner who documents well."

---

### Q4: What is your biggest weakness?
**Answer:**
> "Early on, I tended to over-engineer — I'd reach for the most sophisticated tool even when a simpler solution would work. I've been consciously working on this. For example, in RoutineOps, I resisted the temptation to add a service mesh and kept the architecture straightforward. I now ask 'do we actually need this?' before adding complexity."

---

### Q5: Where do you see yourself in 3–5 years?
**Answer:**
> "In 3 years, I want to be a senior platform or SRE engineer who owns the reliability and scalability of a production Kubernetes platform. In 5 years, I'd like to be in a principal or staff engineer role, influencing infrastructure architecture decisions and mentoring junior engineers. I'm also deeply interested in the security side — so a role that bridges platform engineering and cloud security long-term is where I see myself heading."

---

### Q6: Why should we hire you?
**Answer:**
> "Most fresh graduates have theoretical knowledge. I've independently built, broken, debugged, and fixed production-grade cloud infrastructure. I've worked with tools your team likely uses daily — Terraform, ArgoCD, Prometheus, EKS, Vault — not in a tutorial but in an end-to-end project context. I bring the mindset, hands-on experience, and hunger to learn that turns a fresh hire into a productive team member fast."

---

### Q7: Tell me about a challenge you faced and how you overcame it.
**Answer (STAR):**
> **Situation:** While building Aegis Stack, I needed to implement image signing with Cosign as part of the CI/CD pipeline.
> **Task:** Integrate Cosign into a 7-stage GitHub Actions pipeline and enforce admission verification via Kyverno.
> **Action:** I researched the Cosign signing workflow, set up a keyless signing approach using GitHub OIDC, wrote the Kyverno ClusterPolicy to verify image signatures before admission, and tested it end-to-end by pushing both signed and unsigned images to verify enforcement.
> **Result:** Every unsigned or tampered image is now rejected at admission, giving the platform cryptographic supply chain security.

---

### Q8: How do you handle disagreements with teammates?
**Answer:**
> "I focus on the technical merits. If a teammate and I disagree — say, on whether to use Helm or Kustomize — I first ask them to walk me through their reasoning. Then I share mine. We usually find we're optimizing for different things, and once we align on the shared goal, the right answer is often clear. If we're still stuck, I'll propose we prototype both and measure."

---

### Q9: Describe a time you failed.
**Answer (STAR):**
> **Situation:** In an early iteration of PodPlate, I misconfigured Istio's VirtualService and DestinationRule.
> **Task:** Services were intermittently failing but I couldn't pinpoint the cause.
> **Action:** I spent two hours assuming it was a pod-level issue before realizing the issue was at the mesh layer. I learned to check `istioctl analyze` and `kiali` dashboards first for service mesh issues.
> **Result:** The bug was fixed, but more importantly, I built a personal troubleshooting checklist: application → container → pod → service → mesh → ingress.

---

### Q10: What motivates you?
**Answer:**
> "Watching a system work reliably under load. Seeing an alert fire within 30 seconds of a real event. Knowing a pipeline that took hours now takes 8 minutes because of something I built. The tangible impact of automation and good infrastructure design is what drives me."

---

## Linux & Bash Scripting

### Q11: What is the difference between a process and a thread?
**Answer:**
> A **process** is an independent program in execution with its own memory space, file descriptors, and PID. A **thread** is a unit of execution within a process that shares the process's memory space. Processes are isolated — a crash in one doesn't directly affect another. Threads share memory, making communication faster but also risky (race conditions).

---

### Q12: Explain the Linux boot process.
**Answer:**
> 1. **BIOS/UEFI** — Firmware initializes hardware, runs POST.
> 2. **Bootloader (GRUB2)** — Loads the kernel from disk into memory.
> 3. **Kernel** — Initializes hardware drivers, mounts root filesystem.
> 4. **initramfs** — Temporary root filesystem for early boot.
> 5. **systemd (PID 1)** — Starts all services and targets.
> 6. **Login shell** — getty / SSH / display manager.

---

### Q13: What are inodes?
**Answer:**
> An **inode** is a data structure on a filesystem that stores metadata about a file: permissions, owner, size, timestamps, and pointers to data blocks. Crucially, it does **not** store the filename — filenames are stored in directory entries that point to inodes.
```bash
ls -i file.txt        # Show inode number
stat file.txt         # Full inode metadata
df -i                 # Show inode usage per filesystem
```

---

### Q14: What is the difference between `>` and `>>` in bash?
**Answer:**
> `>` **overwrites** the file. `>>` **appends** to the file.
```bash
echo "hello" > file.txt    # Creates/overwrites
echo "world" >> file.txt   # Appends
```

---

### Q15: How do you find all files larger than 100MB?
```bash
find / -type f -size +100M 2>/dev/null
```

---

### Q16: Explain `chmod 755` vs `chmod 644`.
**Answer:**
> - `755` → Owner: rwx (7), Group: r-x (5), Others: r-x (5). Used for executables and directories.
> - `644` → Owner: rw- (6), Group: r-- (4), Others: r-- (4). Used for regular files (configs, scripts not meant to be executed by others).
```bash
chmod 755 /usr/local/bin/myscript.sh
chmod 644 /etc/app/config.yaml
```

---

### Q17: What is a zombie process and how do you handle it?
**Answer:**
> A **zombie process** is one that has finished execution but still has an entry in the process table because the parent hasn't called `wait()` to read its exit status. You can't kill a zombie directly — kill the **parent process** to clean it up.
```bash
ps aux | grep 'Z'            # Find zombies
kill -9 <parent_pid>         # Kill the parent
```

---

### Q18: Write a Bash script to check disk usage and alert if above 80%.
```bash
#!/bin/bash
THRESHOLD=80
df -H | grep -vE '^Filesystem|tmpfs|udev' | while read -r line; do
  USAGE=$(echo "$line" | awk '{print $5}' | sed 's/%//')
  MOUNT=$(echo "$line" | awk '{print $6}')
  if [ "$USAGE" -gt "$THRESHOLD" ]; then
    echo "ALERT: $MOUNT is at ${USAGE}% usage"
    # Could also send a Slack webhook here
  fi
done
```

---

### Q19: What is `ulimit` and when would you change it?
**Answer:**
> `ulimit` controls resource limits for processes spawned by the shell — max open files (`nofile`), max processes (`nproc`), max memory, etc. In Kubernetes and container environments, you may raise `nofile` limits for applications like Elasticsearch or databases that open many file descriptors.
```bash
ulimit -n 65535        # Set max open files for current session
ulimit -a              # Show all limits
```

---

### Q20: Explain the difference between `kill`, `kill -9`, and `kill -15`.
**Answer:**
> - `kill <pid>` or `kill -15 <pid>` sends **SIGTERM** — a graceful shutdown request. The process can catch it and clean up.
> - `kill -9 <pid>` sends **SIGKILL** — immediate termination. The kernel kills it; the process cannot intercept or ignore this signal.

Always try SIGTERM first; use SIGKILL only if the process doesn't respond.

---

### Q21: How does `awk` work? Write an example.
**Answer:**
> `awk` is a text processing tool that processes input line by line, splitting each line into fields (`$1`, `$2`, etc.) by a delimiter (default: whitespace).
```bash
# Print CPU usage of all processes
ps aux | awk '{print $1, $3, $11}'

# Sum the 3rd column of a CSV
awk -F',' '{sum += $3} END {print sum}' data.csv

# Print lines where 5th field > 80
df -H | awk '$5+0 > 80 {print $0}'
```

---

### Q22: What is `cron`? Write a cron job to run a script every day at midnight.
```bash
# Edit crontab
crontab -e

# Run at midnight every day
0 0 * * * /home/abhijit/scripts/backup.sh >> /var/log/backup.log 2>&1

# Format: minute hour day month weekday command
```

---

### Q23: Explain `systemd` unit files.
**Answer:**
> A systemd unit file defines how a service is started, stopped, and managed. Key sections:
```ini
[Unit]
Description=My App
After=network.target

[Service]
Type=simple
ExecStart=/usr/local/bin/myapp
Restart=always
RestartSec=5s
User=appuser

[Install]
WantedBy=multi-user.target
```
```bash
systemctl daemon-reload
systemctl enable myapp
systemctl start myapp
systemctl status myapp
```

---

## Networking Fundamentals

### Q24: Explain the OSI model.
| Layer | Name | Example Protocols |
|-------|------|-------------------|
| 7 | Application | HTTP, DNS, SMTP, gRPC |
| 6 | Presentation | TLS/SSL, JSON encoding |
| 5 | Session | NetBIOS, RPC |
| 4 | Transport | TCP, UDP |
| 3 | Network | IP, ICMP, BGP |
| 2 | Data Link | Ethernet, MAC |
| 1 | Physical | Cables, fiber, radio waves |

---

### Q25: What happens when you type `https://google.com` in a browser?
**Answer:**
> 1. **DNS resolution** — OS checks cache → `/etc/hosts` → recursive DNS resolver → authoritative DNS returns IP.
> 2. **TCP 3-way handshake** — SYN → SYN-ACK → ACK to establish connection on port 443.
> 3. **TLS handshake** — Client hello, server certificate, key exchange, cipher negotiation, session established.
> 4. **HTTP GET request** — Browser sends `GET / HTTP/1.1` with headers.
> 5. **Server responds** — HTML/CSS/JS returned, browser renders.
> 6. **TCP teardown** — FIN/ACK sequence closes connection.

---

### Q26: What is the difference between TCP and UDP?
| Feature | TCP | UDP |
|---------|-----|-----|
| Connection | Connection-oriented | Connectionless |
| Reliability | Guaranteed delivery, ordering | No guarantee |
| Speed | Slower (overhead) | Faster |
| Use cases | HTTP, SSH, databases | DNS, video streaming, metrics (StatsD) |

---

### Q27: What is a subnet mask and CIDR notation?
**Answer:**
> A subnet mask defines which part of an IP address is the network and which is the host.
> - `10.0.0.0/24` → 256 addresses, 254 usable hosts (network + broadcast reserved)
> - `10.0.0.0/16` → 65,536 addresses
> - `192.168.1.0/28` → 16 addresses, 14 usable hosts
>
> In Terraform VPC configs:
```hcl
cidr_block = "10.0.0.0/16"
# Subnets:
"10.0.1.0/24"  # Public subnet AZ-1
"10.0.2.0/24"  # Public subnet AZ-2
"10.0.11.0/24" # Private subnet AZ-1
```

---

### Q28: What is NAT and why is it used?
**Answer:**
> **Network Address Translation (NAT)** allows multiple private IP addresses to share a single public IP. In AWS, a **NAT Gateway** allows private subnet instances to reach the internet (for package installs, API calls) without being directly reachable from the internet. This is critical for EKS worker nodes in private subnets.

---

### Q29: What is the difference between a Load Balancer and a Reverse Proxy?
**Answer:**
> - **Load Balancer**: Distributes traffic across multiple backend servers to ensure availability and scalability.
> - **Reverse Proxy**: Sits in front of servers to handle SSL termination, caching, compression, and routing. Can also do load balancing.
> - **In practice**: Nginx/HAProxy act as reverse proxies; AWS ALB/NLB are load balancers. Istio's Envoy proxy acts as both.

---

### Q30: What is DNS TTL and why does it matter in Kubernetes?
**Answer:**
> **TTL (Time-To-Live)** is how long a DNS record is cached by resolvers. In Kubernetes, CoreDNS resolves service names. Short TTLs help during rolling deployments so clients pick up new endpoints quickly. However, very short TTLs increase DNS query load. In Kubernetes pod specs:
```yaml
spec:
  dnsConfig:
    options:
      - name: ndots
        value: "2"
  dnsPolicy: ClusterFirst
```

---

## Git & Version Control

### Q31: What is the difference between `git merge` and `git rebase`?
**Answer:**
> - `git merge` creates a new **merge commit** that joins two branch histories. History is preserved, non-linear.
> - `git rebase` **replays** commits from one branch on top of another. Produces a clean, linear history.
>
> **Rule of thumb**: Use `rebase` for local/feature branches to keep history clean. Use `merge` for integrating into `main` in team environments (preserves audit trail).
```bash
# Rebase feature onto main
git checkout feature/login
git rebase main

# Merge feature into main
git checkout main
git merge feature/login
```

---

### Q32: What is `git cherry-pick`?
**Answer:**
> Applies a specific commit from one branch to another without merging the entire branch.
```bash
git cherry-pick <commit-hash>
# Useful for hotfixes: apply a fix from feature branch directly to main
```

---

### Q33: How do you undo the last commit without losing changes?
```bash
git reset --soft HEAD~1   # Undo commit, keep changes staged
git reset --mixed HEAD~1  # Undo commit, keep changes unstaged (default)
git reset --hard HEAD~1   # Undo commit AND discard all changes (DANGER)
```

---

### Q34: What is a `.gitignore` and what should you put in it for a DevOps project?
```gitignore
# Terraform state (sensitive)
*.tfstate
*.tfstate.backup
.terraform/
.terraform.lock.hcl

# Environment variables
.env
*.env

# Credentials
credentials.json
*.pem
*.key

# OS files
.DS_Store
Thumbs.db

# IDE
.vscode/
.idea/
```

---

### Q35: Explain GitOps.
**Answer:**
> **GitOps** is an operational model where Git is the single source of truth for both application and infrastructure state. Desired state is declared in Git; a GitOps operator (like ArgoCD) continuously reconciles the live state in the cluster with the Git-declared state. Benefits:
> - Full audit trail (who changed what, when)
> - Easy rollback (revert a commit)
> - No manual `kubectl apply` in production
> - Pull-based model (operator pulls from Git) vs push-based (CI pushes to cluster)

---

## Docker & Containerization

### Q36: What is the difference between a Docker image and a container?
**Answer:**
> An **image** is a read-only layered filesystem snapshot — the blueprint. A **container** is a running instance of that image with its own writable layer, process namespace, and network interface. Multiple containers can share the same image.

---

### Q37: Explain Docker layers and how they relate to build speed.
**Answer:**
> Every `RUN`, `COPY`, `ADD` instruction in a Dockerfile creates a new layer. Docker caches layers — if a layer hasn't changed, it reuses the cache. To optimize:
```dockerfile
# WRONG — cache busted every time code changes
FROM python:3.11-slim
COPY . /app
RUN pip install -r /app/requirements.txt

# CORRECT — dependencies cached separately
FROM python:3.11-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install -r requirements.txt  # This layer is cached unless requirements.txt changes
COPY . .
```

---

### Q38: What is a multi-stage Docker build?
```dockerfile
# Stage 1: Build
FROM golang:1.21 AS builder
WORKDIR /app
COPY go.mod go.sum ./
RUN go mod download
COPY . .
RUN CGO_ENABLED=0 GOOS=linux go build -o main .

# Stage 2: Runtime
FROM gcr.io/distroless/base-debian12
COPY --from=builder /app/main /main
ENTRYPOINT ["/main"]
```
> **Benefit**: Final image only contains the binary, not the entire Go toolchain. Reduces image size from ~1GB to ~20MB.

---

### Q39: What is the difference between `CMD` and `ENTRYPOINT`?
**Answer:**
> - `ENTRYPOINT` defines the fixed executable that always runs.
> - `CMD` provides default arguments to `ENTRYPOINT` or is the default command if `ENTRYPOINT` isn't set.
```dockerfile
ENTRYPOINT ["nginx"]
CMD ["-g", "daemon off;"]
# docker run myimage              → runs: nginx -g "daemon off;"
# docker run myimage -t           → runs: nginx -t
```

---

### Q40: How do you troubleshoot a container that keeps crashing?
```bash
docker ps -a                          # See exit codes
docker logs <container_id>            # Check logs
docker inspect <container_id>         # Check config, env, mounts
docker run -it --entrypoint /bin/sh <image>  # Override entrypoint to debug
docker exec -it <running_container> /bin/sh  # Shell into running container
```
> Also check: OOMKilled (memory limit), permission issues, missing environment variables, bad entrypoint.

---

### Q41: What is Docker networking? Explain bridge, host, and none modes.
**Answer:**
> - **bridge** (default): Container gets its own network namespace, connected to a virtual bridge. Containers can communicate via container name on the same user-defined network.
> - **host**: Container shares the host's network namespace directly. No network isolation — fastest performance, used for high-throughput use cases.
> - **none**: No network interface. Completely isolated.
```bash
docker network create mynet
docker run --network mynet --name api my-api-image
docker run --network mynet --name db postgres
# 'api' can reach 'db' via hostname 'db'
```

---

## Kubernetes (K8s)

### Q42: Explain the Kubernetes architecture.
**Answer:**

**Control Plane:**
- **kube-apiserver** — The REST API frontend. All kubectl commands go here.
- **etcd** — Distributed key-value store. Source of truth for all cluster state.
- **kube-scheduler** — Assigns pods to nodes based on resource requests, taints, tolerations, affinity.
- **kube-controller-manager** — Runs controllers (Deployment, ReplicaSet, Node, etc.).
- **cloud-controller-manager** — Handles cloud-provider-specific logic (AWS load balancers, volumes).

**Worker Nodes:**
- **kubelet** — Node agent. Ensures pods are running. Communicates with API server.
- **kube-proxy** — Maintains iptables/IPVS rules for Services networking.
- **Container runtime** — containerd/CRI-O runs the actual containers.

---

### Q43: What is the difference between a Deployment and a StatefulSet?
| Feature | Deployment | StatefulSet |
|---------|------------|-------------|
| Pod identity | Random names (pod-xyz) | Stable names (pod-0, pod-1) |
| Storage | Shared/ephemeral | Per-pod PersistentVolumeClaims |
| Scaling order | Parallel | Sequential (ordered) |
| Use case | Stateless apps (API, web) | Stateful apps (databases, Kafka) |

---

### Q44: Explain Kubernetes Services types.
**Answer:**
> - **ClusterIP** (default): Internal cluster IP. Only accessible within the cluster.
> - **NodePort**: Exposes service on each node's IP at a static port (30000–32767).
> - **LoadBalancer**: Provisions a cloud load balancer (AWS ALB/NLB). External access.
> - **ExternalName**: Maps a service to an external DNS name.
> - **Headless** (`clusterIP: None`): Returns pod IPs directly via DNS. Used by StatefulSets.

---

### Q45: What are readiness and liveness probes?
```yaml
livenessProbe:
  httpGet:
    path: /healthz
    port: 8080
  initialDelaySeconds: 30
  periodSeconds: 10
  failureThreshold: 3
  # If this fails 3 times, kubelet RESTARTS the container

readinessProbe:
  httpGet:
    path: /ready
    port: 8080
  initialDelaySeconds: 5
  periodSeconds: 5
  # If this fails, pod is REMOVED from Service endpoints (no traffic sent)
  # Container is NOT restarted
```
> **Key difference**: Liveness = restart the container. Readiness = stop sending traffic to it.

---

### Q46: What is a PodDisruptionBudget (PDB)?
```yaml
apiVersion: policy/v1
kind: PodDisruptionBudget
metadata:
  name: api-pdb
spec:
  minAvailable: 2   # At least 2 pods must be available at all times
  selector:
    matchLabels:
      app: api
```
> PDB prevents voluntary disruptions (node drain, rolling updates) from taking down more pods than you can tolerate. Critical for high-availability workloads.

---

### Q47: Explain Kubernetes RBAC.
```yaml
# ServiceAccount
apiVersion: v1
kind: ServiceAccount
metadata:
  name: ci-deployer
  namespace: production

# Role (namespace-scoped)
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: deployer-role
  namespace: production
rules:
  - apiGroups: ["apps"]
    resources: ["deployments"]
    verbs: ["get", "list", "update", "patch"]

# RoleBinding
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: ci-deployer-binding
  namespace: production
subjects:
  - kind: ServiceAccount
    name: ci-deployer
    namespace: production
roleRef:
  kind: Role
  name: deployer-role
  apiGroup: rbac.authorization.k8s.io
```

---

### Q48: What is a Kubernetes Operator and how does it work?
**Answer:**
> An **Operator** extends Kubernetes with custom domain knowledge using **Custom Resource Definitions (CRDs)** and **controllers**. Example: a PostgreSQL Operator watches for `PostgresCluster` CRDs and automates provisioning, failover, backups, and upgrades.
>
> Pattern: CRD defines the desired state → Controller watches for changes → Reconcile loop adjusts actual state to match desired state.
```bash
kubectl get crd                          # List all CRDs in cluster
kubectl get postgresclusters -n db-ns   # Example custom resource
```

---

### Q49: Explain Horizontal Pod Autoscaler (HPA) and Vertical Pod Autoscaler (VPA).
```yaml
# HPA — scales number of replicas
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: api-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: api
  minReplicas: 2
  maxReplicas: 10
  metrics:
    - type: Resource
      resource:
        name: cpu
        target:
          type: Utilization
          averageUtilization: 70
```
> **HPA** adds/removes pods. **VPA** adjusts CPU/memory requests per pod. They should NOT be used together on CPU/memory — use HPA for CPU and VPA in `Off` mode for recommendations only.

---

### Q50: What is the difference between ConfigMap and Secret?
**Answer:**
> - **ConfigMap**: Stores non-sensitive configuration (env vars, config files). Base64 is NOT used.
> - **Secret**: Stores sensitive data (passwords, tokens, certs). Values are base64-encoded **but not encrypted by default** in etcd.
>
> **Best practice (like in Aegis Stack)**: Don't store secrets in Kubernetes Secrets at all — use **External Secrets Operator** with **HashiCorp Vault** or AWS Secrets Manager.
```bash
# Bad practice
kubectl create secret generic db-creds \
  --from-literal=password=mysecretpassword

# Better: Reference from Vault via ExternalSecret CRD
```

---

### Q51: A pod is stuck in `Pending` state. How do you debug?
```bash
kubectl describe pod <pod-name> -n <namespace>
# Look for: Events section at bottom

# Common causes:
# 1. Insufficient resources → "0/3 nodes are available: 3 Insufficient memory"
#    Fix: Scale node group, reduce resource requests, or check LimitRange

# 2. No matching nodes (taints) → "node(s) had taints that the pod didn't tolerate"
#    Fix: Add tolerations to pod spec

# 3. PVC not bound → "persistentvolumeclaim not found"
#    Fix: kubectl get pvc -n <namespace>

# 4. Image pull failure → "ErrImagePull"
#    Fix: Check image name, tag, imagePullSecrets
```

---

### Q52: What is etcd and why is it critical?
**Answer:**
> `etcd` is the distributed key-value store that backs all Kubernetes state — every pod, deployment, secret, configmap, service, and node is stored there. If etcd goes down or loses data, your entire cluster state is lost. This is why:
> - etcd should always run in a **3 or 5 node cluster** (quorum: majority must be healthy)
> - Regular **etcd snapshots** must be taken (`etcdctl snapshot save`)
> - etcd data should be **encrypted at rest**
```bash
etcdctl snapshot save /backup/etcd-$(date +%Y%m%d).db \
  --endpoints=https://127.0.0.1:2379 \
  --cacert=/etc/kubernetes/pki/etcd/ca.crt \
  --cert=/etc/kubernetes/pki/etcd/server.crt \
  --key=/etc/kubernetes/pki/etcd/server.key
```

---

## Terraform & IaC

### Q53: What is the difference between `terraform plan` and `terraform apply`?
**Answer:**
> - `terraform plan` shows what Terraform **will** do without making any changes. Always review this.
> - `terraform apply` executes the plan and makes actual infrastructure changes.
> - `terraform plan -out=tfplan && terraform apply tfplan` — save the plan and apply exactly that plan (used in CI/CD).

---

### Q54: What is Terraform state and why is it important?
**Answer:**
> Terraform **state** (`terraform.tfstate`) is the mapping between your configuration and the real-world resources. Terraform uses it to know what already exists vs what to create/update/destroy.
>
> **Always use remote state in teams:**
```hcl
terraform {
  backend "s3" {
    bucket         = "my-terraform-state"
    key            = "prod/eks/terraform.tfstate"
    region         = "ap-south-1"
    dynamodb_table = "terraform-state-lock"  # Prevents concurrent applies
    encrypt        = true
  }
}
```

---

### Q55: What is `terraform import` and when would you use it?
**Answer:**
> `terraform import` brings an **existing** infrastructure resource under Terraform management without destroying and recreating it.
```bash
# Import an existing S3 bucket
terraform import aws_s3_bucket.logs my-existing-bucket-name

# Import an existing EKS cluster
terraform import aws_eks_cluster.main my-cluster-name
```
> Use when you have manually-created infrastructure you want to manage with Terraform going forward.

---

### Q56: Explain Terraform modules.
```hcl
# Root module calling a child module
module "eks" {
  source          = "./modules/eks"
  cluster_name    = "production-cluster"
  cluster_version = "1.29"
  vpc_id          = module.vpc.vpc_id
  subnet_ids      = module.vpc.private_subnet_ids
}

# modules/eks/main.tf
variable "cluster_name" {}
variable "cluster_version" {}
variable "vpc_id" {}
variable "subnet_ids" { type = list(string) }

resource "aws_eks_cluster" "this" {
  name     = var.cluster_name
  version  = var.cluster_version
  # ...
}

output "cluster_endpoint" {
  value = aws_eks_cluster.this.endpoint
}
```

---

### Q57: What is `terraform taint` and `terraform untaint`?
**Answer:**
> `terraform taint <resource>` marks a resource for **destruction and recreation** on the next `apply`, even if nothing in the config changed. Useful when a resource is in a degraded state.
```bash
terraform taint aws_instance.web
terraform plan    # Shows: -/+ aws_instance.web (tainted)
terraform apply
```
> In newer Terraform versions, use `terraform apply -replace="aws_instance.web"` instead.

---

### Q58: Explain the difference between `count` and `for_each` in Terraform.
```hcl
# count — integer-based, resources named by index [0], [1], etc.
resource "aws_subnet" "public" {
  count      = 3
  cidr_block = "10.0.${count.index}.0/24"
  # Removing middle element shifts all indices → destroys & recreates!
}

# for_each — map/set-based, resources named by key — more stable
resource "aws_subnet" "public" {
  for_each   = toset(["us-east-1a", "us-east-1b", "us-east-1c"])
  availability_zone = each.key
  cidr_block = lookup(var.subnet_cidrs, each.key)
  # Removing one key only removes that specific resource
}
```
> **Best practice**: Use `for_each` for resources where ordering matters or when you'll add/remove individual items.

---

### Q59: What is `terraform workspace` and when is it useful?
**Answer:**
> Workspaces allow you to manage **multiple state files** from the same configuration. Often used to manage dev/staging/prod from one codebase.
```bash
terraform workspace new dev
terraform workspace new prod
terraform workspace select prod
terraform apply

# Inside config:
resource "aws_eks_cluster" "main" {
  name = "cluster-${terraform.workspace}"  # cluster-dev, cluster-prod
}
```
> **Alternative**: Many teams prefer separate directories/repos per environment for clearer isolation.

---

## AWS Cloud

### Q60: Explain the difference between IAM Roles and IAM Users.
**Answer:**
> - **IAM User**: A person or service with long-lived access keys. Avoid for applications.
> - **IAM Role**: A set of permissions that can be **assumed** by users, services, or AWS resources. Provides temporary credentials.
>
> **Best practice**: EC2 instances and EKS pods should use **IAM Roles**, never hardcoded access keys.
```hcl
# EKS Pod Identity / IRSA (IAM Roles for Service Accounts)
resource "aws_iam_role" "pod_role" {
  name = "pod-s3-reader"
  assume_role_policy = jsonencode({
    Statement = [{
      Effect    = "Allow"
      Principal = { Federated = aws_iam_openid_connect_provider.eks.arn }
      Action    = "sts:AssumeRoleWithWebIdentity"
      Condition = {
        StringEquals = {
          "${local.oidc_issuer}:sub" = "system:serviceaccount:app:my-sa"
        }
      }
    }]
  })
}
```

---

### Q61: What is the difference between SGs (Security Groups) and NACLs?
| Feature | Security Group | NACL |
|---------|---------------|------|
| Level | Instance (ENI) | Subnet |
| State | Stateful | Stateless |
| Rules | Allow only | Allow and Deny |
| Rule evaluation | All rules evaluated | Rules evaluated in order (numbered) |
| Return traffic | Automatically allowed | Must explicitly allow |

---

### Q62: Explain VPC Peering vs Transit Gateway.
**Answer:**
> - **VPC Peering**: Direct 1-to-1 private connection between two VPCs. Simple but doesn't scale (N VPCs = N*(N-1)/2 peering connections).
> - **Transit Gateway**: Hub-and-spoke model — all VPCs connect to a central TGW. Scales to thousands of VPCs, supports routing policies and bandwidth control.

---

### Q63: What is AWS EKS managed node groups vs self-managed nodes vs Fargate?
| Feature | Managed Node Groups | Self-Managed | Fargate |
|---------|-------------------|--------------|---------|
| Node management | AWS handles OS, patches | You handle everything | Serverless — no nodes |
| Customization | Limited | Full | Very limited |
| Cost | Medium | Lower | Higher per pod |
| Use case | Most workloads | Custom AMIs, GPUs | Batch/event-driven |

---

### Q64: What is the difference between ALB and NLB?
**Answer:**
> - **ALB (Application Load Balancer)**: Layer 7. HTTP/HTTPS routing, path-based and host-based routing, WebSocket, WAF integration. Used for web applications.
> - **NLB (Network Load Balancer)**: Layer 4. TCP/UDP/TLS pass-through. Ultra-low latency, handles millions of requests/second. Used for gaming, IoT, databases.

---

### Q65: How does S3 versioning work and when would you enable it?
**Answer:**
> S3 versioning keeps all versions of an object. If you overwrite or delete an object, the previous version is preserved. You can restore to any version.
>
> Enable for:
> - Terraform state buckets (rollback on corrupted state)
> - CI/CD artifact buckets
> - Any data you cannot afford to lose permanently
```bash
aws s3api put-bucket-versioning \
  --bucket my-bucket \
  --versioning-configuration Status=Enabled
```

---

## CI/CD — GitHub Actions, Jenkins, ArgoCD, Tekton

### Q66: Walk me through your 7-stage CI/CD pipeline in Aegis Stack.
**Answer:**
> 1. **Code Lint & Test** — Static analysis and unit tests run on PR.
> 2. **Build Docker Image** — Multi-stage Dockerfile build.
> 3. **Vulnerability Scan (Trivy)** — Scan image for CVEs; fail if critical vulnerabilities found.
> 4. **SBOM Generation (Syft)** — Generate Software Bill of Materials listing all dependencies and their versions.
> 5. **Image Signing (Cosign)** — Cryptographically sign the image using keyless signing via GitHub OIDC.
> 6. **Push to Registry** — Push signed image to ECR (Amazon Elastic Container Registry).
> 7. **GitOps Update** — Update image tag in the Helm values file in the GitOps repo. ArgoCD detects the change and syncs to the cluster.

---

### Q67: Write a GitHub Actions workflow for a Docker build and push to ECR.
```yaml
name: Build and Push to ECR

on:
  push:
    branches: [main]

env:
  AWS_REGION: ap-south-1
  ECR_REPOSITORY: my-app

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Configure AWS credentials
        uses: aws-actions/configure-aws-credentials@v4
        with:
          role-to-assume: ${{ secrets.AWS_ROLE_ARN }}
          aws-region: ${{ env.AWS_REGION }}

      - name: Login to Amazon ECR
        id: login-ecr
        uses: aws-actions/amazon-ecr-login@v2

      - name: Build and push Docker image
        env:
          ECR_REGISTRY: ${{ steps.login-ecr.outputs.registry }}
          IMAGE_TAG: ${{ github.sha }}
        run: |
          docker build -t $ECR_REGISTRY/$ECR_REPOSITORY:$IMAGE_TAG .
          docker push $ECR_REGISTRY/$ECR_REPOSITORY:$IMAGE_TAG

      - name: Scan image with Trivy
        uses: aquasecurity/trivy-action@master
        with:
          image-ref: ${{ steps.login-ecr.outputs.registry }}/${{ env.ECR_REPOSITORY }}:${{ github.sha }}
          severity: CRITICAL
          exit-code: 1
```

---

### Q68: What is the difference between ArgoCD push-based vs pull-based GitOps?
**Answer:**
> - **Push-based (traditional CI/CD)**: CI pipeline pushes (`kubectl apply`, `helm upgrade`) directly to the cluster. Requires cluster credentials in CI.
> - **Pull-based (ArgoCD/Flux)**: An operator **inside** the cluster watches a Git repo and pulls changes. Cluster credentials never leave the cluster. More secure.
>
> ArgoCD's reconciliation loop: watches Git repo every 3 minutes (configurable) or on webhook trigger → compares live state with desired state → syncs differences.

---

### Q69: How do you handle secrets in CI/CD pipelines?
**Answer:**
> **Never** hardcode secrets in pipeline YAML files or commit them to Git.
> - **GitHub Actions**: Use `secrets.MY_SECRET` — stored encrypted in GitHub Settings.
> - **Jenkins**: Use Jenkins Credentials plugin → reference as `$JENKINS_SECRET_ID`.
> - **Best practice for Kubernetes**: Use External Secrets Operator to sync from Vault/AWS Secrets Manager to Kubernetes Secrets at runtime.
> - **OIDC/Workload Identity**: Use GitHub Actions OIDC to assume an AWS IAM role — no long-lived AWS keys needed at all.

---

### Q70: Explain ArgoCD Application Sync Policies.
```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: my-app
  namespace: argocd
spec:
  source:
    repoURL: https://github.com/abhijitray7810/app-gitops
    targetRevision: HEAD
    path: k8s/overlays/production
  destination:
    server: https://kubernetes.default.svc
    namespace: production
  syncPolicy:
    automated:
      prune: true      # Delete resources removed from Git
      selfHeal: true   # Revert manual changes made outside Git
    syncOptions:
      - CreateNamespace=true
      - ApplyOutOfSyncOnly=true
```

---

## Observability — Prometheus, Grafana, Loki

### Q71: Explain the Prometheus data model and PromQL.
**Answer:**
> Prometheus stores time series identified by a **metric name** and **labels** (key-value pairs).
> Format: `metric_name{label1="value1", label2="value2"} value timestamp`

```promql
# CPU usage across all pods in a namespace
rate(container_cpu_usage_seconds_total{namespace="production"}[5m])

# Memory usage above 80%
(container_memory_working_set_bytes / container_spec_memory_limit_bytes) > 0.8

# HTTP error rate
rate(http_requests_total{status=~"5.."}[5m]) / rate(http_requests_total[5m])

# 99th percentile latency
histogram_quantile(0.99, rate(http_request_duration_seconds_bucket[5m]))

# Alert: Pod restarts > 5 in 15 minutes
increase(kube_pod_container_status_restarts_total[15m]) > 5
```

---

### Q72: What is the difference between Prometheus `rate()` and `irate()`?
**Answer:**
> - `rate()`: Average per-second rate over the entire time range. Smoothed — good for alerting and dashboards.
> - `irate()`: Rate between the last two data points. More responsive to sudden spikes but noisier.
>
> Use `rate()` for alerting (avoid false alarms). Use `irate()` for real-time debugging dashboards.

---

### Q73: Explain the Loki log query language (LogQL).
```logql
# All logs from a pod
{namespace="production", pod=~"api-.*"}

# Filter for errors
{namespace="production"} |= "ERROR"

# Structured JSON logs — extract and filter a field
{namespace="production"} | json | status_code >= 500

# Count error rate over time
rate({namespace="production"} |= "ERROR" [5m])

# Alert: More than 10 errors per minute
sum(rate({namespace="production"} |= "ERROR" [1m])) > 10
```

---

### Q74: What is an Alertmanager and how does routing work?
**Answer:**
> Alertmanager receives alerts from Prometheus and handles: grouping, deduplication, inhibition, silencing, and **routing** to notification receivers (Slack, PagerDuty, email).
```yaml
route:
  group_by: ['alertname', 'namespace']
  group_wait: 30s
  group_interval: 5m
  repeat_interval: 4h
  receiver: 'slack-critical'
  routes:
    - match:
        severity: critical
      receiver: 'pagerduty'
    - match:
        severity: warning
      receiver: 'slack-warning'
```

---

## Security — Vault, OPA, Kyverno, Falco, Cosign

### Q75: How does HashiCorp Vault work? What secret engines have you used?
**Answer:**
> Vault is a secrets management platform. Key concepts:
> - **Secret Engines**: KV (key-value), AWS (dynamic cloud creds), PKI (certificates), Database (dynamic DB passwords), Kubernetes auth
> - **Auth Methods**: Kubernetes, AWS IAM, OIDC, AppRole
> - **Leases**: Vault secrets can have TTLs — they expire and must be renewed or re-fetched, limiting blast radius of leaks.
>
> In Aegis Stack, pods authenticate to Vault using their **Kubernetes ServiceAccount JWT** via the Kubernetes auth method and receive dynamic secrets.
```bash
# Application inside a pod authenticating to Vault
vault write auth/kubernetes/login \
  role=my-app-role \
  jwt=$(cat /var/run/secrets/kubernetes.io/serviceaccount/token)
```

---

### Q76: What is the difference between OPA Gatekeeper and Kyverno?
| Feature | OPA Gatekeeper | Kyverno |
|---------|---------------|---------|
| Policy language | Rego | YAML |
| Learning curve | Steep | Gentle |
| Mutation | Via ConstraintTemplate | Native, easier |
| Audit mode | Yes (ConstraintTemplate) | Yes |
| Generate policies | No | Yes (can generate resources) |

**Example Kyverno policy** — require all pods to have resource limits:
```yaml
apiVersion: kyverno.io/v1
kind: ClusterPolicy
metadata:
  name: require-resource-limits
spec:
  validationFailureAction: Enforce
  rules:
    - name: check-container-resources
      match:
        any:
          - resources:
              kinds: [Pod]
      validate:
        message: "CPU and memory limits are required."
        pattern:
          spec:
            containers:
              - resources:
                  limits:
                    cpu: "?*"
                    memory: "?*"
```

---

### Q77: How does Falco detect runtime threats?
**Answer:**
> Falco monitors **Linux system calls** via eBPF or a kernel module. It compares syscall activity against a ruleset and fires alerts when suspicious behavior is detected.
>
> Example rule — shell in a container (common post-exploitation indicator):
```yaml
- rule: Terminal shell in container
  desc: Alert when a shell is spawned in a container
  condition: >
    spawned_process and container and shell_procs
    and not user_known_shell_spawn_binaries
  output: >
    Shell spawned in container (user=%user.name container=%container.name
    image=%container.image.repository:%container.image.tag
    shell=%proc.name cmdline=%proc.cmdline)
  priority: WARNING
  tags: [container, shell, mitre_execution]
```

---

### Q78: What is Cosign and how does it prevent supply chain attacks?
**Answer:**
> **Cosign** is a tool for signing and verifying OCI container images. In Aegis Stack:
> 1. CI pipeline builds image → Trivy scans it → Cosign signs it using **keyless signing** (OIDC token from GitHub Actions proves who signed it).
> 2. The **Kyverno ClusterPolicy** checks that every pod's image has a valid Cosign signature before admitting it to the cluster.
> 3. An attacker who bypasses CI and pushes an unsigned image will have it rejected at admission.
```bash
# Sign an image (keyless, CI context)
cosign sign --yes $IMAGE_URI

# Verify a signature
cosign verify \
  --certificate-identity-regexp="https://github.com/abhijitray7810" \
  --certificate-oidc-issuer="https://token.actions.githubusercontent.com" \
  $IMAGE_URI
```

---

## Service Mesh — Istio

### Q79: What problem does Istio solve?
**Answer:**
> Istio adds a **sidecar proxy (Envoy)** to every pod, creating a **service mesh** that handles:
> - **mTLS** — mutual TLS between all services (zero-trust networking)
> - **Traffic management** — circuit breakers, retries, timeouts, canary routing
> - **Observability** — automatic distributed tracing, metrics, access logs without app changes
> - **Authorization policies** — which service can talk to which service
>
> The app doesn't need to know about any of this — it's handled at the network layer by Envoy.

---

### Q80: What is an Istio VirtualService and DestinationRule?
```yaml
# VirtualService — traffic routing rules
apiVersion: networking.istio.io/v1beta1
kind: VirtualService
metadata:
  name: reviews
spec:
  hosts: [reviews]
  http:
    - match:
        - headers:
            x-user-role:
              exact: beta
      route:
        - destination:
            host: reviews
            subset: v2          # Beta users get v2
    - route:
        - destination:
            host: reviews
            subset: v1          # Everyone else gets v1
          weight: 100

---
# DestinationRule — defines subsets and load balancing
apiVersion: networking.istio.io/v1beta1
kind: DestinationRule
metadata:
  name: reviews
spec:
  host: reviews
  trafficPolicy:
    connectionPool:
      tcp:
        maxConnections: 100
    outlierDetection:              # Circuit breaker
      consecutive5xxErrors: 5
      interval: 10s
      baseEjectionTime: 30s
  subsets:
    - name: v1
      labels:
        version: v1
    - name: v2
      labels:
        version: v2
```

---

## YAML Deep Dive

### Q81: What are YAML anchors and aliases?
```yaml
# Define anchor
defaults: &defaults
  restart: always
  resources:
    limits:
      cpu: "500m"
      memory: "256Mi"

# Use alias (reuse the anchor)
services:
  api:
    <<: *defaults        # Merge — equivalent to copying all fields
    image: api:latest

  worker:
    <<: *defaults
    image: worker:latest
    resources:           # Override the merged value
      limits:
        cpu: "1000m"
        memory: "512Mi"
```

---

### Q82: What is the difference between `|` and `>` in YAML?
```yaml
# | (literal block scalar) — preserves newlines
script: |
  #!/bin/bash
  echo "Hello"
  echo "World"
# Becomes: "#!/bin/bash\necho \"Hello\"\necho \"World\"\n"

# > (folded block scalar) — newlines become spaces
description: >
  This is a long description
  that spans multiple lines
  but will be joined into one line.
# Becomes: "This is a long description that spans multiple lines but will be joined into one line.\n"
```

---

## Python for DevOps

### Q83: Write a Python script to check if all pods in a namespace are Running.
```python
#!/usr/bin/env python3
from kubernetes import client, config

def check_pods_running(namespace: str) -> bool:
    config.load_kube_config()  # or load_incluster_config() in-cluster
    v1 = client.CoreV1Api()
    
    pods = v1.list_namespaced_pod(namespace=namespace)
    not_running = []
    
    for pod in pods.items:
        phase = pod.status.phase
        if phase != "Running":
            not_running.append(f"{pod.metadata.name}: {phase}")
    
    if not_running:
        print(f"⚠️  Non-running pods in '{namespace}':")
        for p in not_running:
            print(f"  - {p}")
        return False
    
    print(f"✅ All {len(pods.items)} pods in '{namespace}' are Running.")
    return True

if __name__ == "__main__":
    check_pods_running("production")
```

---

### Q84: Write a Python script to parse a Prometheus alert payload (webhook).
```python
import json
from flask import Flask, request

app = Flask(__name__)

@app.route('/webhook', methods=['POST'])
def handle_alert():
    data = request.json
    for alert in data.get('alerts', []):
        status = alert['status']          # 'firing' or 'resolved'
        name   = alert['labels'].get('alertname', 'Unknown')
        ns     = alert['labels'].get('namespace', 'N/A')
        msg    = alert['annotations'].get('summary', 'No summary')
        print(f"[{status.upper()}] {name} in {ns}: {msg}")
        
        if status == 'firing' and name == 'PodCrashLooping':
            # Auto-remediation: restart the deployment
            pod = alert['labels'].get('pod', '')
            print(f"Auto-restarting deployment for pod: {pod}")
    
    return {"status": "received"}, 200

if __name__ == '__main__':
    app.run(port=5000)
```

---

## System Design Questions

### Q85: Design a CI/CD pipeline for a microservices application on EKS.
**Answer:**

```
Developer → git push → GitHub

GitHub → triggers GitHub Actions workflow:
  1. Checkout code
  2. Run unit tests + linting
  3. Build Docker image (multi-stage)
  4. Scan with Trivy (fail on CRITICAL CVEs)
  5. Generate SBOM with Syft
  6. Sign image with Cosign (keyless OIDC)
  7. Push to ECR with SHA tag
  8. Update image tag in GitOps repo (Helm values.yaml)
  9. Open PR or auto-merge to GitOps repo

GitOps Repo (separate) → ArgoCD watches:
  - Detects new image tag
  - Syncs to EKS cluster
  - Kyverno admits pod only if image is signed
  - Rolling update with readiness probes
  - HPA scales based on CPU

Monitoring:
  - Prometheus alerts on error rate / latency SLOs
  - Grafana dashboard
  - Loki for log aggregation
  - PagerDuty for P1 alerts
```

---

### Q86: Design a multi-region, high-availability AWS architecture.
**Answer:**

```
Route 53 (Global DNS with health checks and latency-based routing)
    ↓
CloudFront (CDN for static assets)
    ↓
Region: ap-south-1 (Primary)              Region: us-east-1 (Secondary)
  ALB                                         ALB
  ↓                                           ↓
EKS Cluster (3 AZs)                       EKS Cluster (3 AZs)
  ↓                                           ↓
RDS Multi-AZ (Primary) ←── replication ──→ RDS Read Replica / Aurora Global
  ↓
ElastiCache (Redis cluster mode)

Storage:
  S3 (cross-region replication enabled)

Secrets:
  AWS Secrets Manager (replicated)

IaC:
  Terraform with remote state in S3
  Module-based: one module per region, shared vars

Key considerations:
  - RPO: < 1 hour (RDS automated backups every hour)
  - RTO: < 15 minutes (Route53 failover + warm standby)
  - Cost: ~40% higher than single-region — justified for critical production
```

---

## Scenario-Based Troubleshooting

### Q87: Production is down. API pods are all in CrashLoopBackOff. What do you do?
**Answer — Systematic approach:**

```bash
# Step 1: Get the current state
kubectl get pods -n production
# → All pods: CrashLoopBackOff

# Step 2: Look at the most recent pod's logs
kubectl logs <pod-name> -n production --previous
# '--previous' shows logs from the LAST (crashed) container instance

# Step 3: Describe pod for events and exit code
kubectl describe pod <pod-name> -n production
# Look at: Exit Code, Last State, Events

# Exit Code 1: Application error (check app logs)
# Exit Code 137: OOM Killed (increase memory limit or fix memory leak)
# Exit Code 143: SIGTERM not handled (check graceful shutdown)

# Step 4: Check recent deployments
kubectl rollout history deployment/api -n production

# Step 5: If recent deployment caused this, rollback immediately
kubectl rollout undo deployment/api -n production

# Step 6: If not a deployment issue, check dependencies
kubectl get pods -n production  # Is database/Redis also down?
kubectl logs -l app=db -n production

# Step 7: Check resource pressure on nodes
kubectl top nodes
kubectl describe node <node-name> | grep -A5 "Conditions:"

# Step 8: Postmortem after recovery
```

---

### Q88: A Terraform `apply` is hanging. What do you do?
**Answer:**

```bash
# Step 1: Check if there's a state lock
terraform show    # May time out if lock held

# Check DynamoDB lock table directly
aws dynamodb scan --table-name terraform-state-lock

# Step 2: If stale lock (previous apply crashed), force-unlock
# Get lock ID from DynamoDB or error message
terraform force-unlock <LOCK_ID>

# Step 3: If it's a real operation hanging, check AWS console
# Are resources being created? (EKS can take 15-20 minutes)
# Look at CloudTrail for what API calls are being made

# Step 4: If truly stuck, CTRL+C to cancel
# Then run: terraform plan to see current state
# Manually remove partially created resources if needed
# Then re-apply
```

---

### Q89: Alert fires: Pod restarts > 10 in the last hour for `payment-service`. What's your investigation path?
```bash
# 1. Check restart count and reason
kubectl describe pod payment-service-xxx -n production
# Look for: OOMKilled, Error, or specific exit codes

# 2. Check recent logs
kubectl logs payment-service-xxx -n production --previous --tail=100

# 3. Check Prometheus
# rate(kube_pod_container_status_restarts_total{pod=~"payment-service.*"}[1h])
# Also check: memory usage vs limit, CPU throttling

# 4. Was there a deployment recently?
kubectl rollout history deployment/payment-service

# 5. Check if it's a dependency issue
# Is the DB connection pool exhausted?
# Is a downstream service timing out?
# Check Istio service graph in Kiali

# 6. Check if OOMKilled:
# → Increase memory limit or fix memory leak
# kubectl top pod payment-service-xxx --containers

# 7. Temporary mitigation while investigating:
kubectl scale deployment payment-service --replicas=5 -n production
# More replicas = each handles less load = fewer OOMs

# 8. If immediate fix needed:
kubectl rollout undo deployment/payment-service -n production
```

---

### Q90: ArgoCD shows an application as "OutOfSync" even after syncing. What do you investigate?
```bash
# 1. Check what ArgoCD thinks is different
argocd app diff my-app

# 2. Common causes:
#    a. Mutating webhook modifying resources after apply
#       (e.g., Istio injecting sidecar, Kyverno adding labels)
#       Fix: Add ignoreDifferences to ArgoCD Application spec

#    b. Server-side apply changing field managers
#       Fix: Use serverSideApply: true in syncOptions

#    c. Default values added by Kubernetes API
#       Fix: ignoreDifferences for specific fields

# Example fix for Istio sidecar injection differences:
spec:
  ignoreDifferences:
    - group: apps
      kind: Deployment
      jsonPointers:
        - /spec/template/metadata/annotations/kubectl.kubernetes.io~1last-applied-configuration

# 3. Force sync with replace if truly stuck:
argocd app sync my-app --force --replace
```

---

## Project-Based Discussion

### Q91: In Aegis Stack, how did you reduce environment setup from "days to under 20 minutes"?

**Answer:**
> Before automation, setting up an AWS EKS environment required: manually creating a VPC in the console, configuring subnets across 3 AZs, setting up IAM roles with correct trust policies, creating the EKS cluster, configuring worker node groups, setting up kubeconfig, and installing cluster addons — each step error-prone and undocumented.
>
> With Terraform, the entire stack is declarative. `terraform apply` provisions all of this in sequence with correct dependencies. EKS itself takes ~12 minutes to provision. The total end-to-end, including VPC, IAM, EKS, and initial addon installation, completes in under 20 minutes. The config is version-controlled, reproducible, and can create identical dev/staging/prod environments.

---

### Q92: How did you achieve "inter-service latency reduction of 35%" in PodPlate?

**Answer:**
> The baseline issue was that services were making synchronous HTTP calls to each other with no retry logic, timeouts, or circuit breaking. When one service was slow, callers would wait indefinitely, holding connections and cascading the slowdown.
>
> With Istio:
> 1. **Timeouts** — Set 500ms timeouts on all inter-service calls at the mesh level (no code changes needed).
> 2. **Circuit breakers** — Configured `outlierDetection` in DestinationRules: after 5 consecutive 5xx errors from a host, eject it from the load balancing pool for 30 seconds. This prevents the caller from repeatedly hitting a failed instance.
> 3. **Retries** — Configured automatic retries for idempotent GET requests with exponential backoff.
> 4. **Connection pooling** — Limited `maxConnections` and `http1MaxPendingRequests` to prevent overloading slow services.
>
> The 35% latency reduction was measured via Prometheus `histogram_quantile(0.99, ...)` comparing p99 latency before and after.

---

### Q93: What was the hardest technical challenge in your projects?

**Answer (genuine):**
> The hardest part was making OPA Gatekeeper and Kyverno work together without conflicts. Both are admission controllers, and they both evaluate the same webhook calls. I had to carefully design which policies lived in which tool — Kyverno handled mutation (adding required labels, resource limits) and validation (image signing checks), while Gatekeeper handled organization-wide compliance rules written in Rego. Getting the webhook failure modes right was also tricky: if both webhooks are set to `Fail` and either is unavailable, all pod creation blocks. I had to configure appropriate timeouts and understand when to use `Fail` vs `Ignore` mode for each policy.

---

## Coding & Scripting Challenges

### Q94: Write a Bash script to automate a Kubernetes rolling restart of all Deployments in a namespace.
```bash
#!/bin/bash
set -euo pipefail

NAMESPACE=${1:-default}

echo "Rolling restart of all Deployments in namespace: $NAMESPACE"

deployments=$(kubectl get deployments -n "$NAMESPACE" -o jsonpath='{.items[*].metadata.name}')

for deploy in $deployments; do
    echo "Restarting deployment: $deploy"
    kubectl rollout restart deployment/"$deploy" -n "$NAMESPACE"
    kubectl rollout status deployment/"$deploy" -n "$NAMESPACE" --timeout=120s
    echo "✅ $deploy restarted successfully"
done

echo "All deployments restarted in $NAMESPACE"
```

---

### Q95: Write a Python function to parse Terraform state and list all EC2 instances.
```python
import json

def list_ec2_from_tfstate(state_file_path: str) -> list[dict]:
    with open(state_file_path) as f:
        state = json.load(f)
    
    instances = []
    for resource in state.get("resources", []):
        if resource["type"] == "aws_instance":
            for instance in resource["instances"]:
                attrs = instance["attributes"]
                instances.append({
                    "id":         attrs.get("id"),
                    "name":       next(
                        (v for k, v in (attrs.get("tags") or {}).items() if k == "Name"),
                        "Unnamed"
                    ),
                    "type":       attrs.get("instance_type"),
                    "private_ip": attrs.get("private_ip"),
                    "public_ip":  attrs.get("public_ip"),
                    "state":      attrs.get("instance_state"),
                })
    return instances

# Usage
instances = list_ec2_from_tfstate("terraform.tfstate")
for i in instances:
    print(f"{i['name']} ({i['id']}): {i['type']} | {i['private_ip']}")
```

---

### Q96: Given a Kubernetes YAML, write a script to extract all image names.
```bash
#!/bin/bash
# Usage: ./get-images.sh deployment.yaml
# Or: kubectl get pods -o yaml | ./get-images.sh

yq eval '.. | select(has("image")) | .image' "$1" 2>/dev/null | sort -u

# Python alternative (no yq needed):
python3 - "$1" << 'EOF'
import yaml, sys

with open(sys.argv[1]) as f:
    docs = list(yaml.safe_load_all(f))

images = set()
def find_images(obj):
    if isinstance(obj, dict):
        if "image" in obj:
            images.add(obj["image"])
        for v in obj.values():
            find_images(v)
    elif isinstance(obj, list):
        for item in obj:
            find_images(item)

for doc in docs:
    find_images(doc)

for img in sorted(images):
    print(img)
EOF
```

---

## Real-World Production Issues

### Q97: How do you perform a zero-downtime Kubernetes deployment?
**Answer — Checklist:**

```yaml
# 1. Set correct update strategy
spec:
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxSurge: 1         # Allow 1 extra pod during update
      maxUnavailable: 0   # Never remove a pod until replacement is ready

# 2. Set readiness probe (traffic only goes to ready pods)
readinessProbe:
  httpGet:
    path: /health
    port: 8080
  initialDelaySeconds: 10
  periodSeconds: 5
  successThreshold: 1
  failureThreshold: 3

# 3. Set terminationGracePeriodSeconds for graceful shutdown
spec:
  terminationGracePeriodSeconds: 60

# 4. Handle SIGTERM in application code
# app should: stop accepting new requests, finish in-flight requests, then exit

# 5. Configure PodDisruptionBudget
# minAvailable: 2  → at least 2 pods always running

# 6. Set resource requests and limits
# (Scheduler needs accurate requests to place pods correctly)
```

---

### Q98: Kubernetes node is NotReady. Walk me through your diagnosis.
```bash
# 1. Get node status
kubectl get nodes
# → node-3: NotReady

# 2. Describe node
kubectl describe node node-3
# Check: Conditions (MemoryPressure, DiskPressure, PIDPressure, NetworkUnavailable)
# Check: Events at the bottom

# 3. SSH to the node (if accessible)
ssh ec2-user@<node-ip>

# 4. Check kubelet status
systemctl status kubelet
journalctl -u kubelet -n 50 --no-pager

# 5. Check disk
df -h
# If /var/lib/docker or /var/lib/containerd is full → images are causing issues
# Fix: docker system prune / crictl rmi --prune

# 6. Check memory
free -h
# OOM? Check dmesg for OOM killer events
dmesg | grep -i "killed process"

# 7. Check network
ping <api-server-ip>
curl -k https://<api-server>:6443/healthz

# 8. AWS-specific: Check if instance is healthy in Auto Scaling Group
aws autoscaling describe-auto-scaling-instances | grep <instance-id>

# 9. Cordon node to prevent scheduling while investigating
kubectl cordon node-3

# 10. Drain node for maintenance/replacement
kubectl drain node-3 --ignore-daemonsets --delete-emptydir-data
```

---

## STAR Method Answers

### Q99: Tell me about a time you improved system reliability. (STAR)
> **Situation:** In PodPlate, services were experiencing cascading failures — when the inventory service slowed down under load, order service requests piled up waiting for responses, eventually causing order service to also become unresponsive.
>
> **Task:** Implement a resilience pattern to prevent cascade failures without changing application code.
>
> **Action:** I implemented Istio circuit breakers via DestinationRules — configuring `outlierDetection` to detect 5 consecutive 5xx errors from a backend and eject it for 30 seconds. I also set HTTP request timeouts of 500ms on all service-to-service calls and configured HPA to scale the inventory service from 2 to 8 replicas when CPU exceeded 70%.
>
> **Result:** Inter-service latency at p99 dropped by 35%. Cascade failures became isolated circuit-break events rather than full outages. The system maintained service availability for end users even when a single backend service was degraded.

---

### Q100: Tell me about a time you automated a tedious manual process. (STAR)
> **Situation:** In RoutineOps, every new environment (dev, staging, prod) required a developer to manually click through the AWS console for 3–4 hours, creating VPC, subnets, security groups, EKS cluster, and node groups — with no documentation and frequent mistakes.
>
> **Task:** Replace the manual process with reproducible, automated infrastructure provisioning.
>
> **Action:** I wrote modular Terraform configurations covering the full stack: VPC module (subnets, route tables, NAT gateway, internet gateway), IAM module (least-privilege roles for EKS, IRSA), and EKS module (cluster, managed node groups, addons). I used `for_each` for multi-AZ resources and S3+DynamoDB for remote state with locking. I parameterized all environment-specific values via `terraform.tfvars` files.
>
> **Result:** A complete environment now provisions in under 20 minutes with `terraform apply`. Configuration drift across dev/staging/prod is eliminated — all environments are identical by definition. The time saved per environment: 3.5+ hours.

---

## ⚡ Quick Reference — Common Commands

### Kubernetes
```bash
kubectl get all -n <ns>
kubectl describe pod <pod> -n <ns>
kubectl logs <pod> -n <ns> --previous
kubectl exec -it <pod> -n <ns> -- /bin/sh
kubectl rollout restart deployment/<name> -n <ns>
kubectl rollout undo deployment/<name> -n <ns>
kubectl top pods -n <ns>
kubectl top nodes
kubectl get events -n <ns> --sort-by='.lastTimestamp'
kubectl auth can-i create pods --as=system:serviceaccount:ns:sa-name
```

### Terraform
```bash
terraform init
terraform plan -out=tfplan
terraform apply tfplan
terraform destroy -target=aws_instance.web
terraform state list
terraform state show aws_eks_cluster.main
terraform output
```

### Docker
```bash
docker build -t myapp:latest .
docker run -d -p 8080:8080 --name myapp myapp:latest
docker exec -it myapp /bin/sh
docker stats
docker system prune -f
docker inspect <container>
```

### AWS CLI
```bash
aws eks update-kubeconfig --name <cluster> --region ap-south-1
aws ec2 describe-instances --filters Name=instance-state-name,Values=running
aws s3 ls s3://my-bucket/
aws logs tail /aws/eks/my-cluster/cluster --follow
aws sts get-caller-identity
```

---

## 📚 Last-Minute Revision Checklist

- [ ] Self-introduction rehearsed (30s / 2min / 4min versions)
- [ ] Can explain all 3 projects end-to-end without looking at notes
- [ ] Know the metrics from your resume cold: 35% latency, 50% incident resolution, 99.9% uptime, 8 min deploy, 30s MTTA
- [ ] CKA exam topics: RBAC, Network Policies, PV/PVC, etcd backup, kubeadm
- [ ] AWS SAA topics: VPC design, IAM policies, S3, EC2, EKS, RDS, CloudFront
- [ ] Can whiteboard a CI/CD pipeline from scratch
- [ ] Can whiteboard a VPC architecture
- [ ] STAR stories ready for: challenge, failure, conflict, achievement, automation

---

> **Prepared for:** Abhijit Ray | B.Tech CSE, Sister Nivedita University, Kolkata  
> **Contact:** roy055659@gmail.com | +91 78108 56895  
> **GitHub:** github.com/abhijitray7810 | **LinkedIn:** linkedin.com/in/abhijitray7810
