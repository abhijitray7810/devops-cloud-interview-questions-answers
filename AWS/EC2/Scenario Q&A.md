# 🚀 AWS EC2 Interview Questions — FAANG/MAANG Level
> **50+ Real-World Scenario-Based Questions with STAR Method Answers**

![AWS](https://img.shields.io/badge/AWS-EC2-orange?style=for-the-badge&logo=amazonaws)
![Level](https://img.shields.io/badge/Level-FAANG%2FMAANG-red?style=for-the-badge)
![Questions](https://img.shields.io/badge/Questions-50%2B-blue?style=for-the-badge)
![Method](https://img.shields.io/badge/Answer%20Format-STAR-green?style=for-the-badge)

---

## 📖 Table of Contents

- [What is the STAR Method?](#-what-is-the-star-method)
- [Section 1 — EC2 Fundamentals](#-section-1--ec2-fundamentals)
- [Section 2 — Instance Types & Sizing](#-section-2--instance-types--sizing)
- [Section 3 — Auto Scaling & Load Balancing](#-section-3--auto-scaling--load-balancing)
- [Section 4 — Storage (EBS, EFS, Instance Store)](#-section-4--storage-ebs-efs-instance-store)
- [Section 5 — Networking & Security](#-section-5--networking--security)
- [Section 6 — High Availability & Disaster Recovery](#-section-6--high-availability--disaster-recovery)
- [Section 7 — Cost Optimization](#-section-7--cost-optimization)
- [Section 8 — Performance Tuning](#-section-8--performance-tuning)
- [Section 9 — Monitoring & Troubleshooting](#-section-9--monitoring--troubleshooting)
- [Section 10 — Advanced & System Design](#-section-10--advanced--system-design)
- [Quick Cheat Sheet](#-quick-cheat-sheet)

---

## 🌟 What is the STAR Method?

The **STAR method** is the gold standard for answering behavioral and scenario-based interview questions at top tech companies.

| Letter | Stands For | Meaning |
|--------|-----------|---------|
| **S** | **Situation** | Set the context — what was happening? |
| **T** | **Task** | What was your responsibility or challenge? |
| **A** | **Action** | What specific steps did YOU take? |
| **R** | **Result** | What was the measurable outcome? |

> 💡 **Pro Tip:** Always quantify your results. Numbers impress interviewers. "Reduced latency by 40%" beats "improved performance."

---

## 📘 Section 1 — EC2 Fundamentals

---

### ❓ Q1. What is Amazon EC2 and how does it work? Explain with a real-world use case.

**🎯 Difficulty:** Medium | **🏢 Asked at:** Amazon, Google, Meta

**STAR Answer:**

- **Situation:** During my onboarding at a mid-sized e-commerce company, I needed to explain EC2 architecture to a new team of developers who had only worked with on-premise servers.
- **Task:** Create a working analogy and deploy a real demo to help them understand EC2's core concepts — virtualization, instance lifecycle, and billing.
- **Action:** I explained EC2 as "renting a virtual computer in AWS's data center." I spun up a `t3.micro` instance, walked through AMI selection, key pair creation, security group configuration, and connected via SSH. I demonstrated the start/stop/terminate lifecycle and showed how billing stops when an instance is stopped (but EBS continues billing).
- **Result:** The team understood EC2 fundamentals within a day. We deployed our first staging environment in under 2 hours, reducing our on-premise dependency by 60% within the quarter.

> **Key Concepts to Know:**
> - EC2 = Elastic Compute Cloud (IaaS)
> - Based on Xen/Nitro hypervisors
> - AMI (Amazon Machine Image) = blueprint for your instance
> - Instance lifecycle: `pending → running → stopping → stopped → terminated`

---

### ❓ Q2. Explain the difference between stopping and terminating an EC2 instance.

**🎯 Difficulty:** Easy | **🏢 Asked at:** Amazon, Walmart Labs, Flipkart

**STAR Answer:**

- **Situation:** A junior developer on our team accidentally terminated a production EC2 instance thinking "terminate" meant the same as "stop." This caused 2 hours of downtime.
- **Task:** I was tasked with recovering the instance and implementing guardrails to prevent this from happening again.
- **Action:** I used the last available AMI snapshot to restore the instance. I then enabled **EC2 Termination Protection** on all production instances using `aws ec2 modify-instance-attribute --disable-api-termination`. I also created an IAM policy that denied `ec2:TerminateInstances` for all non-senior engineers and set up an AWS Config rule to alert on any termination attempt.
- **Result:** Zero accidental terminations in the following 18 months. Recovery time dropped from 2 hours to under 15 minutes using pre-baked AMIs.

> **Key Difference Table:**
>
> | Action | EBS Root Volume | Public IP | Billing | Can Restart? |
> |--------|----------------|-----------|---------|-------------|
> | **Stop** | Preserved | Released | EBS only | ✅ Yes |
> | **Terminate** | Deleted (default) | Released | Stops | ❌ No |

---

### ❓ Q3. What are AMIs and how do you create a custom AMI for a golden image strategy?

**🎯 Difficulty:** Medium | **🏢 Asked at:** Netflix, Uber, Amazon

**STAR Answer:**

- **Situation:** Our startup was manually configuring every new EC2 instance — installing nginx, Node.js, monitoring agents, and applying security hardening. Each setup took 45+ minutes.
- **Task:** Build a "golden AMI" pipeline so any new instance would be production-ready in under 5 minutes with zero manual configuration.
- **Action:** I used **HashiCorp Packer** to automate AMI creation. The pipeline: (1) launched a base Amazon Linux 2 instance, (2) ran Ansible playbooks to install software and apply CIS hardening benchmarks, (3) created a private AMI, (4) tagged it with version and build date, (5) shared it across all AWS accounts via AWS Organizations. I integrated this into our CI/CD pipeline so a new golden AMI was built weekly.
- **Result:** New instance boot-to-ready time dropped from 45 minutes to under 4 minutes. Security audit pass rate improved from 72% to 98%. AMI creation was fully automated with zero human intervention.

---

### ❓ Q4. How does EC2 user data work, and when would you use it vs. a configuration management tool?

**🎯 Difficulty:** Medium | **🏢 Asked at:** Microsoft, Atlassian, Adobe

**STAR Answer:**

- **Situation:** Our auto-scaling group was launching instances that needed environment-specific configuration (dev vs. prod database endpoints). We couldn't bake these into AMIs since they changed per environment.
- **Task:** Design a solution to inject runtime configuration into EC2 instances without hardcoding secrets or building separate AMIs per environment.
- **Action:** I used **EC2 User Data** as a bootstrap script that ran on first launch. The script: (1) fetched environment parameters from **AWS Systems Manager Parameter Store** using IAM role credentials, (2) wrote configuration files, (3) started the application service. For ongoing configuration drift management, I layered **AWS Systems Manager State Manager** to enforce desired state continuously. User Data handled bootstrap; SSM handled ongoing compliance.
- **Result:** We reduced our AMI count from 12 (one per environment + version) to 3 base AMIs. Configuration changes that previously required re-baking AMIs now took under 2 minutes via Parameter Store updates.

---

### ❓ Q5. Explain EC2 placement groups — when would you use Cluster vs. Spread vs. Partition?

**🎯 Difficulty:** Hard | **🏢 Asked at:** Amazon, Goldman Sachs, Twitch

**STAR Answer:**

- **Situation:** Our HPC (High Performance Computing) team was running molecular simulation workloads on EC2 that required extremely low-latency inter-node communication (< 1ms). Standard placement was giving us inconsistent 5–15ms latency.
- **Task:** Reduce inter-node latency to sub-millisecond levels while ensuring the solution scaled to 64 nodes without compromising fault tolerance for our batch processing cluster.
- **Action:** I deployed a **Cluster Placement Group** for the simulation nodes using `c5n.18xlarge` instances with Elastic Fabric Adapter (EFA) networking. This co-located instances on the same physical hardware rack, providing the highest possible network throughput. Separately, for our Kafka brokers (which needed fault isolation), I used a **Partition Placement Group** across 3 AZs so each partition's failure wouldn't affect others. For our critical single-node services, I used **Spread Placement Groups** to ensure each ran on distinct underlying hardware.
- **Result:** HPC workload latency dropped from 10ms average to 0.4ms — a 96% reduction. Simulation jobs completed 3x faster. Kafka cluster survived an AZ failure with zero data loss due to partition isolation.

> **When to Use What:**
> - **Cluster PG** → Low latency, high throughput (HPC, ML training, Hadoop)
> - **Spread PG** → Max fault isolation, critical individual instances (max 7 per AZ)
> - **Partition PG** → Distributed systems with rack-level failure isolation (Kafka, Cassandra, HDFS)

---

## 💻 Section 2 — Instance Types & Sizing

---

### ❓ Q6. How do you choose the right EC2 instance type for a workload?

**🎯 Difficulty:** Medium | **🏢 Asked at:** Amazon, Meta, Stripe

**STAR Answer:**

- **Situation:** A new ML inference service needed to be deployed on EC2. The team had provisioned `m5.4xlarge` instances, but the service was CPU-bound and frequently throttled, causing 95th-percentile latency to spike to 8 seconds.
- **Task:** Analyze the workload and recommend the optimal instance type to bring p95 latency under 500ms within budget.
- **Action:** I profiled the application using **CloudWatch detailed metrics** and **AWS Compute Optimizer**. The analysis showed: high CPU utilization (consistently > 85%), low memory usage (< 20%), GPU ops not being utilized. I recommended migrating to `c6i.4xlarge` (Compute Optimized) which had 15% better price-to-vCPU ratio and Intel Ice Lake processors with AVX-512 instructions that the ML framework could exploit. I ran A/B load tests comparing both instance families.
- **Result:** p95 latency dropped from 8 seconds to 320ms — 96% improvement. Monthly EC2 cost decreased by 23% despite running identical workloads. AWS Compute Optimizer's recommendation aligned with our analysis.

> **Instance Family Quick Reference:**
>
> | Family | Type | Best For | Example |
> |--------|------|----------|---------|
> | `m` | General Purpose | Balanced workloads | m6i, m7g |
> | `c` | Compute Optimized | CPU-heavy, HPC | c6i, c7g |
> | `r` | Memory Optimized | In-memory DB, caches | r6i, r7g |
> | `i` | Storage Optimized | High I/O, databases | i4i, i3en |
> | `p/g` | GPU | ML training/inference | p4d, g5 |
> | `t` | Burstable | Dev/test, low traffic | t3, t4g |

---

### ❓ Q7. What are T-series burstable instances and when should you NOT use them in production?

**🎯 Difficulty:** Medium | **🏢 Asked at:** Lyft, Shopify, Twilio

**STAR Answer:**

- **Situation:** Our production API server running on `t3.medium` instances started experiencing random 30-second latency spikes affecting 5% of all requests during peak hours. Users were reporting timeouts.
- **Task:** Root-cause the latency spikes and implement a permanent fix without over-provisioning.
- **Action:** I checked **CloudWatch EC2 metrics** and found `CPUCreditBalance` dropping to zero during peak hours. When `t3` instances exhaust CPU credits, they throttle to the baseline CPU performance (20% for t3.medium). I had two options: (1) enable unlimited mode (incurs extra charges when credits are exhausted), or (2) migrate to a fixed-performance instance. I analyzed 30 days of CPU patterns — the workload needed sustained CPU above baseline. I migrated to `c6i.large` (fixed performance, similar cost for our sustained usage pattern). I also configured **CloudWatch Alarms** on `CPUCreditBalance` for all remaining T-series instances to alert before they hit zero.
- **Result:** Latency spikes eliminated entirely. Alert system caught 3 other T-series instances approaching credit exhaustion in the following month, preventing similar incidents.

---

### ❓ Q8. Explain Graviton instances. What are the trade-offs when migrating from x86 to ARM?

**🎯 Difficulty:** Hard | **🏢 Asked at:** Amazon, Netflix, Airbnb

**STAR Answer:**

- **Situation:** AWS announced Graviton3 instances with up to 40% better price-performance. Leadership asked us to evaluate migrating our 500-node microservices fleet from `m5` to `m7g` (Graviton3) to reduce costs.
- **Task:** Lead the migration evaluation: identify incompatible workloads, benchmark performance, and build a phased migration plan.
- **Action:** I categorized our 47 microservices: (1) Pure Python/Java/Go services — immediately compatible; (2) Services with native C extensions — required recompilation; (3) Services using x86-specific Docker images — needed ARM64 variants. I set up a parallel test environment with `m7g` instances, rebuilt Docker images for ARM64 using **AWS CodeBuild's multi-arch builds**, and ran 2-week shadow traffic tests. I found one service using an x86-only closed-source binary that couldn't migrate. For the rest, I benchmarked: Graviton3 was 18–35% faster on our workloads.
- **Result:** Migrated 44 of 47 services to Graviton3 over 8 weeks. Annual EC2 cost reduction: $1.2M (31%). The 3 incompatible services were flagged for vendor engagement. Zero production incidents during migration.

---

### ❓ Q9. How do you right-size EC2 instances, and what tools do you use?

**🎯 Difficulty:** Medium | **🏢 Asked at:** Capital One, Intuit, Twitter

**STAR Answer:**

- **Situation:** A cloud cost audit revealed we were spending $800K/month on EC2, but utilization was averaging only 15% CPU and 25% memory across the fleet. Classic over-provisioning from a "lift-and-shift" migration 18 months earlier.
- **Task:** Lead a right-sizing initiative to reduce EC2 costs by at least 35% without affecting SLAs.
- **Action:** I used **AWS Compute Optimizer** and **AWS Cost Explorer's right-sizing recommendations**. My process: (1) Exported Compute Optimizer recommendations for all instances; (2) Filtered by "Over-provisioned" with high confidence; (3) Grouped by application team for coordinated migration; (4) For stateful services, right-sized during maintenance windows using instance type modification (stop → change type → start); (5) For stateless services in ASGs, updated launch templates and did rolling replacements; (6) Tracked actual vs. projected savings in a dashboard.
- **Result:** Reduced EC2 spend from $800K to $490K/month — a 39% reduction ($3.7M/year). Average CPU utilization improved from 15% to 48%. No SLA violations during the 12-week program.

---

## ⚖️ Section 3 — Auto Scaling & Load Balancing

---

### ❓ Q10. Design an Auto Scaling Group for a flash sale event expecting 50x normal traffic.

**🎯 Difficulty:** Hard | **🏢 Asked at:** Amazon, Flipkart, eBay

**STAR Answer:**

- **Situation:** Our e-commerce platform's Black Friday sale was 6 weeks away. The previous year, the site had crashed within 8 minutes of sale start due to a 40x traffic surge that Auto Scaling couldn't handle fast enough (EC2 instance launch takes 2–4 minutes).
- **Task:** Design an ASG strategy that could handle 50x traffic spikes with zero warm-up lag.
- **Action:** I implemented a multi-layer strategy: (1) **Predictive Scaling** — enabled CloudWatch predictive scaling with 2-week training data to pre-launch instances 30 minutes before expected traffic; (2) **Scheduled Scaling** — added a scheduled action to pre-scale to 3x baseline capacity 1 hour before sale start; (3) **Warm Pool** — configured an ASG Warm Pool of pre-initialized, pre-warmed instances in `stopped` state (EBS-only cost, ready in <60 seconds vs. 4 minutes); (4) **Lifecycle Hooks** — used launch lifecycle hooks to fully initialize app before marking instances InService; (5) **Target Tracking** — set CPU target at 50% to give headroom for bursts.
- **Result:** Flash sale handled 52x normal traffic with zero downtime. Time-to-scale from warm pool was 45 seconds vs. 4 minutes. Revenue on sale day: $47M (vs. $0 in the crash year). Infrastructure cost during peak: $12K/hour (acceptable).

---

### ❓ Q11. What is the difference between ALB, NLB, and CLB? When do you use each?

**🎯 Difficulty:** Medium | **🏢 Asked at:** Meta, LinkedIn, DoorDash

**STAR Answer:**

- **Situation:** Our company had a monolithic CLB (Classic Load Balancer) serving all traffic. We were adding microservices and a WebSocket-based real-time notification service, and CLB couldn't route based on URL paths or support WebSockets properly.
- **Task:** Migrate from CLB to the right modern load balancer type for each use case without service disruption.
- **Action:** I audited traffic patterns and service requirements: (1) REST API microservices → **ALB** (Application Load Balancer) with path-based routing (`/api/users/*` → Users service, `/api/orders/*` → Orders service) and host-based routing for multi-tenant SaaS; (2) Real-time WebSocket service → **ALB** (supports WebSockets and HTTP/2 natively); (3) High-throughput internal TCP service for our ML model serving (requires ultra-low latency, static IPs for whitelisting) → **NLB** (Network Load Balancer); (4) CLB → deprecated and migrated all traffic off within 30 days.
- **Result:** P99 API latency reduced by 18% due to connection reuse with ALB's HTTP/2. WebSocket support enabled our real-time feature (previously impossible). NLB for ML serving achieved sub-1ms load balancer overhead vs. ~5ms with CLB.

> **Load Balancer Comparison:**
>
> | Feature | CLB (Legacy) | ALB (L7) | NLB (L4) |
> |---------|-------------|---------|---------|
> | Protocol | HTTP/HTTPS/TCP | HTTP/HTTPS/gRPC | TCP/UDP/TLS |
> | Routing | Basic | Path/Host/Header/IP | Port only |
> | WebSockets | ❌ | ✅ | ✅ |
> | Static IP | ❌ | ❌ (use NLB) | ✅ |
> | Latency | High | Medium | Ultra-low |
> | Use Case | Legacy | Web apps/APIs | Ultra-low latency/Gaming |

---

### ❓ Q12. How do you implement blue/green deployment using EC2 and ALB?

**🎯 Difficulty:** Hard | **🏢 Asked at:** Netflix, Spotify, Atlassian

**STAR Answer:**

- **Situation:** Our team was deploying new application versions in-place (rolling updates), which caused ~2% of requests to fail during deployments due to version mismatches between old and new instances handling the same user session.
- **Task:** Implement zero-downtime blue/green deployments that allowed instant rollback within 30 seconds if a deployment went wrong.
- **Action:** I designed a blue/green system using: (1) Two ASGs — "Blue" (current production) and "Green" (new version); (2) ALB with **weighted target group routing** — normally 100% to Blue; (3) Deployment process: launch Green ASG with new AMI → run automated smoke tests → shift 10% traffic to Green via ALB weighted routing → monitor error rates/latency for 10 minutes → shift 100% to Green if healthy → keep Blue for 30 minutes as rollback option → terminate Blue. I scripted the entire flow in a Jenkins pipeline using AWS CLI `modify-listener` commands to adjust target group weights.
- **Result:** Zero deployment-related user errors for 14 consecutive months. Mean rollback time: 8 seconds (vs. 25 minutes with in-place deployments). Deployment confidence increased — teams deployed 4x more frequently.

---

### ❓ Q13. Explain target tracking vs. step scaling vs. simple scaling policies. Which would you choose for a payment processing system?

**🎯 Difficulty:** Hard | **🏢 Asked at:** Stripe, PayPal, Square

**STAR Answer:**

- **Situation:** Our payment processing service used simple scaling (add 2 instances when CPU > 70%, remove 2 when CPU < 40%), which caused thrashing — instances constantly being added/removed during variable load, and scale-out was too slow during sudden payment volume spikes.
- **Task:** Redesign the scaling policy to handle payment volume spikes (which can be 10x normal in < 60 seconds during major sales events) while preventing over-scaling during gradual load changes.
- **Action:** I implemented a **combined scaling approach**: (1) **Target Tracking** for normal operations — target 60% CPU utilization (AWS auto-calculates scaling steps, has built-in cooldown management, handles scale-in conservatively to avoid premature termination); (2) **Step Scaling** as an overlay for extreme spikes — if CPU hits 85%, immediately add 5 instances; if CPU hits 95%, add 10 instances (bypasses target tracking's gradual approach); (3) **Scheduled Scaling** for known peak windows (payday, Black Friday); (4) Set scale-in cooldown to 10 minutes for payment services to prevent premature termination of instances processing long-running transactions.
- **Result:** Payment processing scale-out response time improved from 4 minutes to 45 seconds. Zero transaction timeouts due to under-provisioning. Scale-in thrashing eliminated — instances now stayed active for minimum 10-minute windows.

---

### ❓ Q14. How do you handle session stickiness (sticky sessions) at scale, and why is it an anti-pattern?

**🎯 Difficulty:** Medium | **🏢 Asked at:** Amazon, Uber, Grab

**STAR Answer:**

- **Situation:** Our legacy web application used server-side sessions stored in-memory. ALB sticky sessions ensured each user always hit the same backend instance. When an instance failed, all users on that instance were logged out — 15% of concurrent users at any given time. Auto Scaling was also broken: scale-in couldn't remove any instance without disrupting active sessions.
- **Task:** Eliminate the dependency on sticky sessions while maintaining user session continuity and enabling proper auto-scaling.
- **Action:** I migrated sessions to **Amazon ElastiCache (Redis)**: (1) Deployed Redis cluster in a private subnet; (2) Modified the application to read/write sessions from Redis (instead of in-memory); (3) Disabled ALB sticky sessions; (4) Tested with chaos engineering — killed random EC2 instances and confirmed users were NOT logged out; (5) Enabled proper ASG scale-in termination policies (terminate oldest instance first). The migration took 2 sprints with zero user-visible disruption (sessions migrated transparently).
- **Result:** Instance failure no longer caused logouts — user impact dropped from 15% to 0% on instance failures. Auto Scaling could now properly scale in, reducing off-peak costs by 40%. Application became truly stateless and horizontally scalable.

---

## 💾 Section 4 — Storage (EBS, EFS, Instance Store)

---

### ❓ Q15. Explain the difference between EBS, EFS, and Instance Store. Give a use case for each.

**🎯 Difficulty:** Medium | **🏢 Asked at:** Amazon, Databricks, Palantir

**STAR Answer:**

- **Situation:** Onboarding a new data engineering team, I needed to select the right storage type for three different workloads: a transactional database, shared ML training data accessed by 50 nodes concurrently, and a temporary high-speed sorting operation for data processing jobs.
- **Task:** Evaluate and recommend the right EC2 storage type for each workload with justification.
- **Action:**
  - **Database (PostgreSQL)** → **EBS gp3**: Block storage, persists independently of instance lifecycle, supports Multi-Attach for limited HA scenarios, provisioned IOPS available (`io2`) for high-throughput needs. Configured with 3000 IOPS baseline and 125 MiB/s throughput.
  - **Shared ML training data** → **EFS (Elastic File System)**: NFS-based, truly shared across all 50 training nodes simultaneously, scales storage automatically, no pre-provisioning. Used EFS Intelligent Tiering to save costs on infrequently accessed checkpoints.
  - **Temporary sort/shuffle operations** → **Instance Store (NVMe SSD)**: Physically attached NVMe SSDs on `i3en.24xlarge` instances, 3.5x higher IOPS than EBS, zero network latency. Data is ephemeral (lost on stop/terminate) — fine for temporary intermediate data.
- **Result:** ML training jobs ran 2.1x faster using EFS vs. copying data to each instance. Database IOPS costs reduced 18% by tuning gp3 parameters. Data processing sort operations completed 4x faster using instance store vs. EBS.

---

### ❓ Q16. A database on EBS is hitting I/O performance limits. How do you diagnose and fix it?

**🎯 Difficulty:** Hard | **🏢 Asked at:** Oracle, MongoDB, Amazon

**STAR Answer:**

- **Situation:** Our production MySQL database on a `db.r5.2xlarge` EC2 instance (gp2 EBS volume, 500 GB) started showing query latency spikes during business hours. P99 latency jumped from 50ms to 4 seconds during peak load.
- **Task:** Diagnose the I/O bottleneck and resolve it with minimal downtime (SLA: < 15 minutes maintenance window).
- **Action:** Diagnosis: CloudWatch showed `VolumeQueueLength > 1` (I/O requests queuing), `BurstBalance` at 0% (gp2 had exhausted burst credits on a 500GB volume — max sustained IOPS = 1500). Root cause: workload exceeded sustained IOPS. Fix: (1) **Live volume modification** (no downtime needed with gp3) — `aws ec2 modify-volume` to migrate from `gp2` to `gp3`, increasing baseline IOPS from 1500 to 6000 at no extra cost; (2) Provisioned 125 MiB/s throughput; (3) For future-proofing, set up CloudWatch alarm on `BurstBalance < 20%` to alert before exhaustion. I also right-sized the volume to 1TB to increase the gp2 IOPS ceiling as a safety net.
- **Result:** Query latency returned to < 60ms within 10 minutes of volume modification (live, zero downtime). `BurstBalance` concept eliminated (gp3 has no burst model — always consistent IOPS). Similar incident has not recurred in 14 months.

---

### ❓ Q17. How do you take consistent EBS snapshots for a running database without downtime?

**🎯 Difficulty:** Hard | **🏢 Asked at:** Notion, Figma, Snowflake

**STAR Answer:**

- **Situation:** Our compliance team required daily database backups with a 4-hour RPO. We were taking EBS snapshots of our running MySQL instance without freezing I/O, which created "crash-consistent" but not "application-consistent" snapshots. A restore test revealed the restored database needed 45 minutes of crash recovery, sometimes resulting in data inconsistency.
- **Task:** Implement application-consistent EBS snapshots that could be restored cleanly within the 4-hour RPO requirement.
- **Action:** I implemented a pre/post snapshot script using **AWS Systems Manager Run Command**: (1) Pre-snapshot: run `FLUSH TABLES WITH READ LOCK` on MySQL + sync filesystem to disk (`sync; echo 3 > /proc/sys/vm/drop_caches`); (2) Trigger EBS snapshot via `aws ec2 create-snapshot`; (3) Post-snapshot: release MySQL lock (`UNLOCK TABLES`). Entire lock window: < 8 seconds. I automated this with **AWS Data Lifecycle Manager (DLM)** custom pre/post scripts for daily snapshots + 30-day retention. Also tested restores monthly by spinning up a new instance from snapshot.
- **Result:** Snapshot lock window: 6–8 seconds (negligible). Restored databases required zero crash recovery — clean start in < 2 minutes. Monthly restore tests consistently validated recovery within 22 minutes (well within 4-hour RPO). Compliance audit passed.

---

## 🔐 Section 5 — Networking & Security

---

### ❓ Q18. Explain Security Groups vs. NACLs. How do you design a defense-in-depth network security model?

**🎯 Difficulty:** Hard | **🏢 Asked at:** Amazon, CrowdStrike, Cloudflare

**STAR Answer:**

- **Situation:** Our startup had passed a security audit with a major enterprise customer as a prerequisite. The auditor found that all EC2 instances shared a single Security Group and were in public subnets with direct internet access — a significant security risk.
- **Task:** Redesign the network security architecture to implement defense-in-depth within 4 weeks while maintaining application availability.
- **Action:** I designed a layered security model: (1) **VPC design**: Public subnet (only ALB/NAT Gateway), Private subnet (application EC2), Isolated subnet (databases — no internet route); (2) **NACLs** (stateless, subnet-level): Block known malicious IP ranges, deny all inbound on management ports (22, 3389) at subnet level; (3) **Security Groups** (stateful, instance-level): Web tier SG allows only ports 80/443 from ALB SG; App tier SG allows only traffic from Web tier SG; DB SG allows only port 3306 from App tier SG; Bastion SG allows port 22 from corporate IP range only; (4) Implemented **AWS Systems Manager Session Manager** to eliminate SSH entirely — zero open port 22.
- **Result:** Attack surface reduced by 94% (zero public-facing instances with direct SSH). Enterprise customer passed us through security review. A penetration test revealed zero critical findings (vs. 7 in the previous audit). No public IP addresses on application or database instances.

---

### ❓ Q19. How do you securely connect to EC2 instances without exposing port 22 to the internet?

**🎯 Difficulty:** Medium | **🏢 Asked at:** Dropbox, Box, Okta

**STAR Answer:**

- **Situation:** Security team mandated eliminating all public-internet-accessible SSH (port 22) following an industry-wide wave of SSH brute-force attacks. We had 200+ EC2 instances, all with security groups allowing `0.0.0.0/0:22`.
- **Task:** Implement secure, audited EC2 access without port 22 exposed to the internet, while maintaining developer productivity.
- **Action:** Implemented **AWS Systems Manager Session Manager**: (1) Attached `AmazonSSMManagedInstanceCore` IAM role to all EC2 instances; (2) Installed SSM Agent (pre-installed on Amazon Linux 2); (3) Removed port 22 from all Security Groups; (4) Configured **Session Manager preferences** to log all sessions to CloudWatch Logs and S3 for audit; (5) Used IAM policies to control which engineers could start sessions on which instances; (6) For engineers who needed `scp`/`sftp`, configured SSH over SSM tunnel using ProxyCommand in `~/.ssh/config`. Zero open ports required.
- **Result:** Port 22 completely removed from all 200+ security groups. Full session audit trail available in CloudWatch. Developer access actually improved — no more VPN + bastion hop. Last security audit: zero findings on EC2 access controls.

---

### ❓ Q20. Explain IAM roles for EC2. How does the Instance Metadata Service (IMDS) work, and what are IMDSv2 security improvements?

**🎯 Difficulty:** Hard | **🏢 Asked at:** Amazon, Coinbase, Robinhood

**STAR Answer:**

- **Situation:** A security scan found that an attacker who gained SSRF (Server-Side Request Forgery) access to one of our web servers could query `http://169.254.169.254/latest/meta-data/iam/security-credentials/` and steal temporary IAM credentials — a well-known attack vector (used in the Capital One breach).
- **Task:** Mitigate IMDS credential theft risk across our entire EC2 fleet without breaking applications that legitimately use instance metadata.
- **Action:** I enforced **IMDSv2** (which requires a PUT request to obtain a session token before metadata queries — SSRF exploits typically can't follow this two-step process): (1) Used `aws ec2 modify-instance-metadata-options --http-tokens required` on all running instances; (2) Updated launch templates and AMIs to set `HttpTokens: required` by default; (3) Added an **AWS Config rule** (`ec2-imdsv2-check`) to flag any new instance not using IMDSv2; (4) Tested all applications that used metadata — updated AWS SDK versions to support IMDSv2 (SDK auto-handles it); (5) Set `HttpPutResponseHopLimit: 1` to prevent containers from accessing host metadata.
- **Result:** IMDSv2 enforced on 100% of fleet within 3 weeks. SSRF-based credential theft attack vector eliminated. Subsequent penetration test confirmed the IMDS attack vector no longer worked. Compliance team satisfied with the defense.

---

## 🌐 Section 6 — High Availability & Disaster Recovery

---

### ❓ Q21. Design a multi-AZ EC2 architecture with 99.99% availability. Walk through the failure scenarios.

**🎯 Difficulty:** Hard | **🏢 Asked at:** Amazon, Goldman Sachs, JPMorgan

**STAR Answer:**

- **Situation:** Our fintech platform had a 99.9% SLA (8.7 hours downtime/year). A new enterprise contract required 99.99% SLA (52 minutes downtime/year) — 10x improvement.
- **Task:** Redesign the EC2 architecture to achieve 99.99% availability with documented failure mode handling for each component.
- **Action:** Multi-AZ design across 3 AZs: (1) **ALB** across all 3 AZs (ALB is multi-AZ by default — AZ failure handled transparently); (2) **ASG** with `MinCapacity=6`, `DesiredCapacity=9` (3 per AZ) — if one AZ fails, ASG auto-launches replacement instances in remaining AZs; (3) **EBS Multi-Attach** with `io2` for shared storage; (4) **Amazon Aurora Multi-AZ** for database (failover in < 30 seconds); (5) **ElastiCache Redis Multi-AZ** with auto-failover; (6) Implemented **health checks** at all layers: ALB target group health checks (2 consecutive failures = remove from rotation), ASG EC2 health checks (replace unhealthy instances), Route 53 health checks for multi-region DR. Tested via **AWS Fault Injection Simulator** (FIS): simulated AZ failure, instance termination, CPU stress.
- **Result:** Architecture achieved 99.997% actual availability over 12 months (17 minutes unplanned downtime). Enterprise contract signed. FIS chaos tests revealed and fixed 3 previously unknown failure modes before they hit production.

---

### ❓ Q22. How do you implement cross-region disaster recovery for EC2 workloads?

**🎯 Difficulty:** Hard | **🏢 Asked at:** Netflix, Expedia, Booking.com

**STAR Answer:**

- **Situation:** A regulatory requirement mandated that our trading platform maintain a Recovery Time Objective (RTO) of 30 minutes and Recovery Point Objective (RPO) of 5 minutes for a complete region failure scenario.
- **Task:** Design and implement a cross-region DR solution that met RTO 30min / RPO 5min without running duplicate full-capacity infrastructure.
- **Action:** I designed a **Warm Standby** DR architecture: (1) **AMI replication**: AWS EventBridge triggered cross-region AMI copy to `us-west-2` for every new golden AMI build; (2) **EBS snapshot replication**: DLM policy copied hourly snapshots to DR region (< 5 min RPO for most data); (3) **Aurora Global Database**: sub-second replication lag, < 1-minute RTO for DB failover; (4) **Warm Standby EC2**: DR region ran 25% capacity ASG (enough to handle degraded service during failover ramp-up); (5) **Route 53 health checks** + DNS failover: automatic traffic cut-over to DR region if primary fails health checks; (6) Conducted quarterly DR drills with full region failover simulation.
- **Result:** DR drill results: RTO achieved in 18 minutes (vs. 30-minute requirement), RPO = 4 minutes. DR region cost: 15% of primary region cost (warm standby is much cheaper than active-active). Passed regulatory audit with zero findings.

---

## 💰 Section 7 — Cost Optimization

---

### ❓ Q23. How do you reduce EC2 costs using Reserved Instances vs. Savings Plans vs. Spot Instances?

**🎯 Difficulty:** Hard | **🏢 Asked at:** Amazon, Airbnb, Pinterest

**STAR Answer:**

- **Situation:** Our startup's monthly AWS bill was $280K, with EC2 accounting for 65% ($182K). All instances were On-Demand. Our CFO mandated a 40% EC2 cost reduction.
- **Task:** Analyze usage patterns and design a purchasing strategy mixing Reserved Instances, Savings Plans, and Spot Instances to achieve the target reduction.
- **Action:** I analyzed 3 months of EC2 usage with Cost Explorer: (1) Identified 40% of compute as **baseline stable** (always-on, predictable instance types) → purchased **1-year Compute Savings Plans** (up to 66% savings, flexible across instance types/regions); (2) 35% of compute was **batch processing** (ML training, ETL jobs, nightly analytics) → migrated to **Spot Instances** with Spot Interruption Handlers and checkpointing (up to 90% savings); (3) 15% was **variable but predictable** web tier → **Target Tracking ASG with Spot mixed instances policy** (70% Spot + 30% On-Demand for stability); (4) Remaining 10% was truly unpredictable/critical → kept On-Demand.
- **Result:** Monthly EC2 cost reduced from $182K to $94K — a 48% reduction ($1.06M annual savings). Zero Spot interruption-related outages (proper fault tolerance implementation). CFO signed off on a 3-year Savings Plan for additional 12% saving.

---

### ❓ Q24. Walk me through handling Spot Instance interruptions for a critical batch job.

**🎯 Difficulty:** Hard | **🏢 Asked at:** Amazon, Lyft, Databricks

**STAR Answer:**

- **Situation:** Our ML training pipeline ran on Spot Instances to save costs but was regularly interrupted mid-training — losing 4–8 hours of GPU compute and costing more in retries than we'd save on Spot pricing.
- **Task:** Redesign the ML training pipeline to be interrupt-tolerant with no more than 2-minute work loss per interruption.
- **Action:** I implemented: (1) **Instance Metadata polling**: Background thread checked `http://169.254.169.254/latest/meta-data/spot/instance-action` every 5 seconds for 2-minute interruption notice; (2) **Checkpoint-on-interrupt**: Upon receiving notice, training loop saved model checkpoint to S3 immediately; (3) **SQS-based job queue**: Each training job was defined as an SQS message. On interruption, the message became visible again for another instance to pick up; (4) **Spot Fleet with diversified instance types**: Requested 6 different GPU instance types across 3 AZs — reduced interruption probability from 15% to < 3%; (5) **Resume from checkpoint**: Training script detected existing S3 checkpoint on startup and resumed from last epoch.
- **Result:** Interruption-related work loss capped at < 2 minutes (one checkpoint interval). Effective Spot savings: 72% vs On-Demand. ML training costs reduced from $45K/month to $12.6K/month. Interruption frequency dropped from 15% to 2.8% due to instance diversification.

---

## 📊 Section 8 — Performance Tuning

---

### ❓ Q25. How do you optimize EC2 network performance for a high-throughput application?

**🎯 Difficulty:** Hard | **🏢 Asked at:** Amazon, Twitch, Cloudflare

**STAR Answer:**

- **Situation:** Our video transcoding service was processing 4K video uploads and hitting network throughput limits — uploads were completing at 800 Mbps when we needed > 20 Gbps for our SLA.
- **Task:** Increase network throughput by 25x without changing the application code.
- **Action:** I identified and addressed network bottlenecks: (1) Upgraded from `m5.4xlarge` (Up to 10 Gbps) to `m5n.8xlarge` with **Enhanced Networking** (up to 25 Gbps); (2) Enabled **Elastic Network Adapter (ENA)** driver (pre-installed on Amazon Linux 2, confirmed version 2.2.10+); (3) For our HPC tier, used `c5n.18xlarge` with **Elastic Fabric Adapter (EFA)** — provides OS-bypass networking, critical for MPI-based workloads; (4) Placed instances in a **Cluster Placement Group** to maximize intra-cluster bandwidth; (5) Tuned OS network parameters: `net.core.rmem_max`, `net.ipv4.tcp_rmem` for large socket buffers.
- **Result:** Network throughput increased from 800 Mbps to 23 Gbps — a 28x improvement. Transcoding pipeline throughput increased 18x. SLA met. Instance count reduced from 30 to 4 (cost savings of 70%).

---

### ❓ Q26. How do you benchmark and optimize EBS I/O performance?

**🎯 Difficulty:** Hard | **🏢 Asked at:** Oracle, MongoDB, Elastic

**STAR Answer:**

- **Situation:** A Cassandra cluster's write latency was inconsistent — usually 2ms but spiking to 200ms for 5% of writes. The spikes were causing cascading timeouts and client retries, amplifying the load.
- **Task:** Identify the root cause of I/O latency spikes and reduce p99 write latency to under 10ms.
- **Action:** I ran EBS benchmarking using `fio` to characterize actual I/O patterns: `fio --rw=randwrite --ioengine=libaio --iodepth=32 --bs=4k`. Found: sustained 6000 IOPS but spikes to `VolumeQueueLength=48` during compaction events. Root causes: (1) gp2 volumes bursting exhausted; (2) Compaction and write I/O competing for IOPS. Fixes: (1) Migrated to `io2 Block Express` with 16,000 provisioned IOPS + 1000 MiB/s throughput; (2) Used Cassandra's `compaction_throughput_mb_per_sec` setting to throttle compaction I/O; (3) Separated commitlog to a separate EBS volume (IO isolation); (4) Set EBS `Multi-Attach` on `io2` for shared access.
- **Result:** P99 write latency: 2ms (no more spikes). `VolumeQueueLength` never exceeded 2. Cassandra availability improved from 99.8% to 99.98%. Client timeout rate dropped from 0.5% to 0.001%.

---

## 🔍 Section 9 — Monitoring & Troubleshooting

---

### ❓ Q27. An EC2 instance becomes unreachable. Walk me through your troubleshooting process.

**🎯 Difficulty:** Hard | **🏢 Asked at:** Amazon, Pagerduty, Datadog

**STAR Answer:**

- **Situation:** At 2 AM, our on-call alert fired: a production EC2 instance hosting a critical payment API was unreachable. Automated health checks failed. Users were getting 503 errors.
- **Task:** Diagnose and resolve the issue within our 15-minute RTO SLA.
- **Action:** Systematic troubleshooting: (1) **EC2 System Status Checks**: AWS console showed "System Status Check Failed" — hardware issue on AWS side. Checked "Instance Status Check": passed. This told me the underlying hardware had failed; (2) **Action**: Stopped and started the instance (NOT reboot — stop/start moves to new hardware); (3) Instance came back up in 3 minutes but app didn't start; (4) **Console output**: `aws ec2 get-console-output` showed file system errors — dirty shutdown had corrupted the journal; (5) **Recovery**: Used EC2 serial console to run `fsck` on unmounted partition; (6) Instance recovered fully in 11 minutes total. Post-incident: added CloudWatch alarm on `StatusCheckFailed_System` to auto-recover future hardware failures via EC2 Auto Recovery.
- **Result:** Total downtime: 11 minutes (within 15-minute RTO). EC2 Auto Recovery configured for all production instances — subsequent hardware failures (3 in next 6 months) auto-recovered in under 5 minutes with zero on-call involvement.

---

### ❓ Q28. How do you use CloudWatch to build a comprehensive EC2 monitoring strategy?

**🎯 Difficulty:** Medium | **🏢 Asked at:** Amazon, New Relic, Splunk

**STAR Answer:**

- **Situation:** Our team was reactive to EC2 incidents — we'd find out about problems from customer support tickets rather than proactive monitoring. MTTR (Mean Time to Resolve) was 45 minutes.
- **Task:** Build a proactive monitoring system that detected issues within 2 minutes and reduced MTTR to under 10 minutes.
- **Action:** I built a comprehensive monitoring strategy: (1) **Default CloudWatch Metrics** (5-min): CPU, NetworkIn/Out, DiskReadOps/WriteOps; (2) **Detailed Monitoring** (1-min) enabled for production instances; (3) **CloudWatch Agent** for custom metrics: memory utilization (not available by default), disk usage, process counts; (4) **CloudWatch Alarms**: CPU > 85% for 3 minutes → SNS → PagerDuty; StatusCheckFailed → EC2 Auto Recovery; Memory > 90% → alert; (5) **CloudWatch Dashboards**: per-service real-time view; (6) **CloudWatch Container Insights** for containerized workloads; (7) **CloudWatch Logs Insights** for log analysis; (8) Integrated with **AWS X-Ray** for distributed tracing.
- **Result:** Mean Time to Detect (MTTD) improved from "customer reports" to 2 minutes. MTTR reduced from 45 to 9 minutes. Proactive alerts caught 23 incidents in the first 3 months before users were impacted.

---

### ❓ Q29. How do you diagnose high CPU on an EC2 instance without installing additional tools?

**🎯 Difficulty:** Medium | **🏢 Asked at:** Google, Amazon, Meta

**STAR Answer:**

- **Situation:** A production instance's CPU was at 99%, causing API timeouts, but the instance was in an isolated environment with strict security policies (no third-party tools allowed, no internet access to download profilers).
- **Task:** Identify the CPU-consuming process and root cause using only built-in Linux and AWS tools.
- **Action:** Using only built-in tools: (1) `top -b -n 1 | head -30` — identified PID 14823 consuming 94% CPU; (2) `ps aux | grep 14823` — identified it as a specific Java process; (3) `jstack 14823` (Java thread dump) — found a thread stuck in an infinite loop in a specific class/method; (4) `lsof -p 14823` — checked for file descriptor leaks or stuck I/O; (5) CloudWatch Logs — correlated the CPU spike start time with a recent deployment: commit `f3a9b2c` had introduced the bug; (6) Immediate mitigation: `kill -9 14823` to kill the runaway process (service auto-restarted via systemd). Root cause: a missing null check caused an infinite loop on empty input.
- **Result:** Issue diagnosed in 8 minutes using zero third-party tools. Bug fixed and redeployed in 22 minutes. Added a unit test for null input to prevent regression.

---

## 🏗️ Section 10 — Advanced & System Design

---

### ❓ Q30. Design a highly available web application on EC2 serving 10 million users/day.

**🎯 Difficulty:** Hard | **🏢 Asked at:** Amazon, Meta, Google

**STAR Answer:**

- **Situation:** In a system design interview at a FAANG company, I was asked to design the EC2 infrastructure for a social media platform expected to serve 10 million daily active users with 99.99% uptime.
- **Task:** Design a scalable, highly available, cost-optimized EC2 architecture from scratch.
- **Action:** My design:
  - **Traffic estimation**: 10M DAU → ~115 req/sec average, ~500 req/sec peak (5x)
  - **Multi-AZ, multi-region**: 3 AZs in `us-east-1` primary, `us-west-2` warm standby
  - **Edge**: CloudFront CDN in front of ALB (cache static assets, reduce origin load by 80%)
  - **Web/API tier**: ALB + ASG of `c6g.4xlarge` (Graviton3, cost-optimized), min 9 instances (3/AZ), target 50% CPU
  - **Caching**: ElastiCache Redis Multi-AZ for sessions + hot data (90% cache hit rate)
  - **Database**: Aurora PostgreSQL Multi-AZ + Aurora Replicas for read scaling
  - **Media storage**: S3 (EC2 not used for static assets)
  - **Background jobs**: Spot Instance ASG with SQS-backed job queue
  - **Security**: Private subnets for app/DB, public for ALB only. IMDSv2 enforced. SSM Session Manager for access.
  - **Observability**: CloudWatch + X-Ray + structured logs to CloudWatch Logs
- **Result:** Architecture handles 500+ req/sec peak with sub-100ms p99 latency. Estimated cost: $45K/month (70% reduction vs naive design with over-provisioning). 99.997% historical availability in similar implementations.

---

### ❓ Q31. How would you migrate a large on-premises application to EC2 with zero downtime?

**🎯 Difficulty:** Hard | **🏢 Asked at:** Amazon, Accenture, Deloitte

**STAR Answer:**

- **Situation:** A 10-year-old on-premises Java application (24/7 availability requirement, 50,000 concurrent users) needed to migrate to AWS. Previous attempts by another team had resulted in 6-hour downtime and ultimately rolled back.
- **Task:** Lead a zero-downtime migration to EC2 with a clear rollback plan at every stage.
- **Action:** Phased migration: (1) **Discovery**: Used **AWS Application Discovery Service** to map all dependencies, ports, processes; (2) **Replication**: Used **AWS MGN (Application Migration Service)** to continuously replicate on-prem servers to EC2 (block-level replication, sub-second lag after initial sync); (3) **Testing**: Launched test instances from replicated data, ran full regression suite; (4) **Cutover planning**: Identified minimum downtime window = database sync time; (5) **Day of cutover**: Put application in maintenance mode → final database sync (< 2 min) → update DNS to point to EC2 ALB (TTL pre-set to 60s) → validate → remove maintenance mode. Total downtime: 3 minutes; (6) Keep on-prem running for 2 weeks as rollback option.
- **Result:** Migration completed with 3 minutes of downtime (maintenance window announced). All 50,000 concurrent users transitioned seamlessly. Post-migration performance: 35% better response times (newer hardware, EBS gp3 vs spinning disk). On-prem decommissioned 30 days later saving $180K/year.

---

### ❓ Q32. How do you implement infrastructure as code for EC2 using Terraform or CloudFormation?

**🎯 Difficulty:** Medium | **🏢 Asked at:** HashiCorp, Amazon, Salesforce

**STAR Answer:**

- **Situation:** Our team was provisioning EC2 instances manually through the AWS console. Configuration drift between environments was causing "works in staging, fails in production" bugs. We had 12 engineers making ad-hoc console changes with no audit trail.
- **Task:** Implement Infrastructure as Code (IaC) for all EC2 resources with drift detection and GitOps-based change management.
- **Action:** Chose **Terraform** over CloudFormation for its multi-cloud portability and HCL readability: (1) Wrote Terraform modules for: `ec2_instance`, `auto_scaling_group`, `security_group`, `launch_template`; (2) Used **Terraform workspaces** for environment isolation (dev/staging/prod) with variable overrides; (3) Implemented **Terraform Cloud** for state management + policy as code (Sentinel policies to enforce tagging, instance type allowlists, encryption requirements); (4) Set up **CI/CD pipeline**: PR → `terraform plan` output as PR comment → review → merge → `terraform apply`; (5) Enabled **AWS Config** to detect and alert on out-of-band console changes (drift detection); (6) Documented all modules in a Confluence wiki with usage examples.
- **Result:** Configuration drift eliminated — 100% of EC2 resources managed via IaC within 8 weeks. "Works in staging, fails in prod" incidents dropped from 3/month to 0. New environment provisioning time: 8 minutes (vs. 2+ days manually). Full audit trail of all infrastructure changes via Git history.

---

### ❓ Q33. Explain the EC2 Nitro system. Why does it matter for security and performance?

**🎯 Difficulty:** Hard | **🏢 Asked at:** Amazon, AMD, Intel

**STAR Answer:**

- **Situation:** During a security architecture review, I was asked to explain why newer EC2 instance types (C5, M5, R5+) had different security properties than older Xen-based instances and whether we should prioritize migrating to Nitro-based instances.
- **Task:** Evaluate the security and performance benefits of Nitro instances and build a migration business case.
- **Action:** I researched Nitro architecture: The Nitro System offloads hypervisor functions to dedicated hardware ASICs (Nitro Card) and software (Nitro Hypervisor). Benefits: (1) **Security**: Host hardware and hypervisor are isolated from EC2 instance — even AWS operators cannot access guest memory or storage; (2) **Performance**: Near bare-metal performance — dedicated hardware handles I/O, giving > 95% of hardware resources to customer workloads vs ~80% on Xen; (3) **Features**: Nitro-only features include NVMe EBS access, EFA, 100 Gbps networking, bare metal instances, Graviton processors, and hardware-level memory encryption; (4) **Compliance**: Nitro architecture satisfies strict regulatory requirements for data isolation. I built a migration plan prioritizing Nitro for all new deployments and scheduled migration of Xen instances during their next maintenance window.
- **Result:** 100% Nitro fleet achieved in 4 months. Average performance improvement: 22% (CPU-bound tasks). Compliance team validated Nitro's hardware isolation for our PCI-DSS workloads. Network throughput ceiling increased from 10 Gbps to 100 Gbps for our highest-throughput services.

---

## 🧩 Rapid-Fire Questions (Q34–Q55)

> These are shorter questions often asked as follow-ups or in rapid-fire rounds. Answers are concise but complete.

---

### ❓ Q34. What is an Elastic IP, and when should you avoid using it?

**STAR Answer:**
- **S:** Needed a stable public IP for a third-party API whitelist that required our server's IP.
- **T:** Assign a static public IP to our EC2 instance that persists across stop/start cycles.
- **A:** Allocated an Elastic IP and associated it with the instance. However, I documented the anti-patterns: EIPs are per-region limited (default 5), they incur charges when NOT associated, and over-reliance on EIPs prevents you from embracing ephemeral, auto-scaling architectures.
- **R:** Used EIP only for the specific whitelist requirement. All other public access went through ALB with DNS — no EIPs needed.

---

### ❓ Q35. How does EC2 hibernation work? What are its limitations?

**STAR Answer:**
- **S:** Our data science team ran long ML preprocessing jobs on large EC2 instances. If a job ran overnight and needed to be paused (off-hours cost saving), they'd lose all in-memory state.
- **T:** Enable a way to pause work mid-computation and resume the next morning without losing state.
- **A:** Configured EC2 hibernation on supported instance types (requires EBS root volume encrypted, instance RAM < 150 GB). Hibernation saves RAM contents to EBS root volume, then stops the instance. On start, RAM is restored — processes resume exactly where they stopped.
- **R:** Data science team saved 60% on overnight compute costs. Hibernation limitations documented: not supported on bare metal, requires encrypted root EBS, max 60-day hibernation period.

---

### ❓ Q36. What is EC2 Dedicated Hosts vs. Dedicated Instances? When is each required?

**STAR Answer:**
- **S:** A pharmaceutical customer required HIPAA-compliant EC2 isolation with software license compliance (Windows Server per-socket/per-core licensing).
- **T:** Achieve physical server isolation for compliance and optimize Microsoft Windows licensing costs.
- **A:** Used **Dedicated Hosts** (you control the physical server, see socket/core counts for BYOL licensing) for Windows-licensed workloads. Used **Dedicated Instances** (physical isolation but AWS controls hardware placement) for HIPAA compliance without licensing requirements.
- **R:** Saved $180K/year on Windows licensing using BYOL on Dedicated Hosts. HIPAA audit passed with Dedicated Instance attestation.

---

### ❓ Q37. Explain the difference between vertical and horizontal scaling on EC2.

**STAR Answer:**
- **S:** Our monolithic app couldn't be horizontally scaled (it had local state). We needed to scale for a product launch.
- **T:** Scale the application to handle 5x traffic while planning for future horizontal scalability.
- **A:** Short-term: **vertical scaling** — stopped the instance, changed from `m5.xlarge` to `m5.4xlarge` (4x vCPU, 4x RAM). This took 2 minutes. Long-term: refactored the app to externalize state to ElastiCache/RDS, enabling **horizontal scaling** — adding more instances instead of bigger instances. Horizontal scaling is preferred for AWS: it's more fault-tolerant, more cost-effective, and scales indefinitely.
- **R:** Vertical scaling handled the immediate launch. Horizontal scaling (post-refactor) allowed us to run 12 smaller instances for the same cost with 4x better fault tolerance.

---

### ❓ Q38. How do you share EC2 AMIs across multiple AWS accounts in an organization?

**STAR Answer:**
- **S:** Our platform team managed golden AMIs but individual product teams (with separate AWS accounts) needed to launch instances from them.
- **T:** Share AMIs across 20 AWS accounts securely without making them public.
- **A:** Used `aws ec2 modify-image-attribute --image-id ami-xxx --launch-permission --add '[{"UserId":"123456789"}]'` to share with specific accounts. For org-wide sharing, used AWS Resource Access Manager (RAM) to share AMIs across the entire organization with one command. Also ensured EBS snapshot permissions were granted (AMI sharing without snapshot sharing fails).
- **R:** All 20 accounts could launch from golden AMIs. AMI update propagation time: < 5 minutes. No account needed to maintain their own base AMIs.

---

### ❓ Q39. What is EC2 Instance Connect, and how is it different from Session Manager?

**STAR Answer:**
- **S:** Needed to provide temporary SSH access to an EC2 instance for a contractor without creating a long-lived key pair.
- **T:** Grant time-limited, audited SSH access without managing SSH keys.
- **A:** Used **EC2 Instance Connect** — pushes a temporary one-time SSH public key to instance metadata, valid for 60 seconds. The contractor could SSH in using the AWS console or CLI without any pre-provisioned keys. **Session Manager** (no SSH needed at all, browser-based terminal) was used for our regular team — no open ports required.
- **R:** Contractor access granted and revoked in 10 minutes. Zero permanent keys created. Full CloudTrail audit of all access.

---

### ❓ Q40. How do you handle EC2 key pair management at scale in an enterprise?

**STAR Answer:**
- **S:** 200 developers had their own EC2 key pairs. When developers left the company, their keys weren't reliably revoked — a serious security risk.
- **T:** Centralize and automate EC2 access with zero-trust principles.
- **A:** Migrated all EC2 access to **AWS Systems Manager Session Manager** (zero SSH keys needed). For legacy instances still requiring SSH: implemented **AWS Secrets Manager** to store and rotate SSH keys with automatic rotation. Set up a **Lambda** that revoked old key pairs from all instances on employee offboarding via HR system webhook.
- **R:** Zero orphaned SSH keys after implementation. Access revocation time on offboarding reduced from "sometimes never" to < 5 minutes automated.

---

### ❓ Q41. What are Launch Templates, and why are they preferred over Launch Configurations?

> Launch Templates are the modern replacement for Launch Configurations. Key advantages: versioning (you can have multiple versions), support for newer instance features (Spot mixed instances, T2/T3 Unlimited, Placement Groups, Capacity Reservations), partial specification (inherit from base version), and support for both On-Demand and Spot in a single template. Launch Configurations are immutable — to change anything, you must create a new one. Launch Templates allow in-place version updates and default version pinning.

---

### ❓ Q42. How does EC2 Capacity Reservations work, and when would you use it?

> Capacity Reservations guarantee EC2 capacity in a specific AZ for a specific instance type — you pay for it whether or not you use it (unlike Reserved Instances which are billing discounts, not capacity reservations). Use cases: regulatory compliance requiring guaranteed capacity, DR events, year-end trading systems. Can be combined with Savings Plans for maximum discount on reserved capacity.

---

### ❓ Q43. Explain EC2 Fleet. How is it different from a Spot Fleet?

> **EC2 Fleet** is the newer, more flexible API to launch a mix of On-Demand, Reserved, and Spot Instances in a single request across multiple instance types and AZs. **Spot Fleet** is the older, Spot-specific version. EC2 Fleet supports `instant`, `maintain`, and `request` modes. Use EC2 Fleet for modern workloads requiring multi-instance-type diversity for cost and availability optimization.

---

### ❓ Q44. How do you implement zero-downtime EC2 patching?

> Use **AWS Systems Manager Patch Manager** with a maintenance window: (1) Define patch baseline (approved patches, severity thresholds); (2) Create ASG with multiple instances; (3) Patch one instance at a time using lifecycle hooks — ASG detaches, SSM patches instance, instance rejoins. Or, replace strategy: build new patched AMI → update launch template → trigger rolling instance refresh on ASG with `MinHealthyPercentage: 90%`. Modern approach: treat instances as immutable — never patch in-place, always replace.

---

### ❓ Q45. What is AWS Nitro Enclaves, and what security problem does it solve?

> Nitro Enclaves are isolated compute environments within an EC2 instance with no persistent storage, no network access, and no admin access — not even from the host OS. They solve the problem of processing extremely sensitive data (PII, cryptographic keys, healthcare data) where even the instance operator or AWS itself cannot access the data in plaintext. Use cases: payment card processing, digital rights management, confidential ML inference on sensitive data.

---

### ❓ Q46. Describe EC2 Image Builder and how it integrates with a CI/CD pipeline.

> EC2 Image Builder automates the creation, testing, and distribution of AMIs. Pipeline: define Image Recipe (base AMI + components) → Build Stage (launch instance, apply components) → Test Stage (run test scripts — confirm app works, security benchmarks pass) → Distribution (copy to multiple regions, share to accounts). Integrates with CodePipeline: code change triggers CodePipeline → Image Builder builds new AMI → pipeline deploys to ASG launch template → rolling instance refresh.

---

### ❓ Q47. How do you implement auto-healing for EC2 instances?

> Three layers: (1) **ASG auto-replacement** — ASG replaces instances that fail EC2 or ELB health checks automatically; (2) **EC2 Auto Recovery** — CloudWatch alarm on `StatusCheckFailed_System` triggers `recover` action (moves to new hardware, keeps same IP and EBS); (3) **CloudWatch Agent + custom health checks** — application-level health checks that terminate unhealthy instances (allowing ASG to replace them). Use `aws ec2 recover-instances` action for individual instances outside ASGs.

---

### ❓ Q48. What is the difference between an EC2 Elastic Network Interface (ENI) and an ENA?

> **ENI (Elastic Network Interface)** is a virtual network card you can attach/detach from EC2 instances — useful for management networks, dual-homed instances, or failing over private IPs between instances. **ENA (Elastic Network Adapter)** is a specific high-performance network driver that enables Enhanced Networking — supporting up to 100 Gbps bandwidth. ENA is about throughput performance; ENI is about network topology and IP management.

---

### ❓ Q49. How do tags work on EC2, and why are they critical for large-scale operations?

> EC2 tags (key-value pairs) are used for: (1) **Cost allocation** — Cost Explorer breaks down spending by tag (e.g., `Environment:prod`, `Team:payments`); (2) **Access control** — IAM conditions can restrict instance operations by tag value; (3) **Automation** — SSM Patch Manager, Lambda functions, and Config rules filter resources by tag; (4) **Operations** — filtering in console, CLI. Best practice: enforce mandatory tags via AWS Config rule (`required-tags`) and AWS Organizations SCP to deny resource creation without required tags.

---

### ❓ Q50. What is EC2 Serial Console, and when would you use it?

> EC2 Serial Console provides access to the serial port of an EC2 instance — useful when SSH/SSM are inaccessible (e.g., network misconfiguration, boot failures, kernel panics). You can interact with BIOS, GRUB, and the OS even when the instance can't network. Must be enabled per-instance and per-account. Critical for troubleshooting: if you locked yourself out of SSH by misconfiguring `/etc/ssh/sshd_config`, Serial Console lets you fix it without launching a new instance.

---

### ❓ Q51. How do you encrypt an existing unencrypted EBS volume?

> Unencrypted EBS volumes can't be encrypted in-place. Process: (1) Create snapshot of unencrypted volume; (2) Copy snapshot with encryption enabled (`aws ec2 copy-snapshot --encrypted`); (3) Create new EBS volume from encrypted snapshot; (4) Stop instance → detach old volume → attach new encrypted volume → start instance. To enforce encryption going forward: enable **EBS encryption by default** in the region settings — all new volumes and snapshots will be encrypted automatically.

---

### ❓ Q52. Explain the concept of instance store vs. EBS for root volumes. What are the implications for AMIs?

> Instances with **EBS root volumes** (most modern instances): persist across stop/start, support snapshots/AMIs, support encryption, support modification. **Instance store root volumes** (older instance types like `m1`, `c1`): ephemeral — all data lost on stop or terminate (only survive reboot). For AMIs: EBS-backed AMIs can be stopped and restarted; instance store-backed AMIs must be terminated and relaunched. Modern best practice: always use EBS-backed root volumes.

---

### ❓ Q53. What is Amazon EC2 Auto Scaling predictive scaling, and how does it differ from reactive scaling?

> **Reactive scaling** (Target Tracking, Step Scaling): CloudWatch detects high CPU → triggers scale-out → takes 1–5 minutes. During this lag, existing instances absorb excess load. **Predictive Scaling**: ML analyzes historical CloudWatch metrics (2+ weeks) to forecast future load → automatically schedules scale-out BEFORE load arrives. Perfect for recurring patterns: Monday morning traffic spikes, nightly batch jobs, weekly reporting. Combine both: predictive for known patterns + reactive for unexpected spikes.

---

### ❓ Q54. How do you configure health check grace periods in an ASG, and why is it important?

> The **health check grace period** is the time ASG waits after launching a new instance before starting health checks. If set too short and the application takes 3 minutes to start, the ASG will terminate and replace the instance in a restart loop. If set too long, a genuinely unhealthy instance runs for too long. Best practice: set grace period to application startup time + 20% buffer. Measure actual startup time using UserData execution timing. For applications with variable startup (JVM warm-up), use ELB health checks with appropriate `HealthyThresholdCount` instead of relying solely on EC2 status checks.

---

### ❓ Q55. What is the difference between EC2 On-Demand Capacity Reservations and Zonal Reserved Instances from a capacity standpoint?

> Both provide capacity guarantees in specific AZs. Key differences: (1) **Zonal RIs**: billing discount + capacity reservation in that AZ — but the capacity reservation is tied to the RI term (1 or 3 years); (2) **On-Demand Capacity Reservations (ODCR)**: no billing discount by default, pure capacity reservation, cancel anytime, can be combined with Savings Plans for discounts. ODCRs are better for short-term guarantees; Zonal RIs for long-term predictable workloads. AWS recommends Savings Plans + ODCRs as the modern alternative to Zonal RIs.

---

## 📋 Quick Cheat Sheet

### EC2 Instance Purchasing Options

| Option | Commitment | Savings | Best For |
|--------|-----------|---------|----------|
| On-Demand | None | 0% (baseline) | Unpredictable, short-term |
| Savings Plans | 1 or 3 yr | 30–66% | Steady-state baseline workloads |
| Reserved Instances | 1 or 3 yr | 40–72% | Specific instance type, specific AZ |
| Spot | None | 50–90% | Fault-tolerant, flexible, batch |
| Dedicated Hosts | 1 or 3 yr | Variable | BYOL, regulatory compliance |

### Key EC2 Limits to Know

| Resource | Default Limit |
|----------|-------------|
| vCPUs (On-Demand) | 32–1152 (varies by family) |
| Elastic IPs | 5 per region |
| Security Groups per ENI | 5 |
| Rules per Security Group | 60 inbound + 60 outbound |
| NACL Rules | 20 inbound + 20 outbound |
| Volumes per Instance | 28 EBS + instance stores |

### STAR Method Reminder

```
S → Set the scene (1-2 sentences, specific context)
T → Your specific role/challenge (1 sentence)
A → What YOU did, step by step (3-5 concrete steps)
R → Quantified outcome (numbers, percentages, time saved)
```

> 💡 **Golden Rule**: Every "R" should have a metric. "Improved performance" < "Reduced latency by 47%."

---

## 🔖 Additional Resources

- [AWS EC2 Documentation](https://docs.aws.amazon.com/ec2/)
- [AWS Well-Architected Framework](https://aws.amazon.com/architecture/well-architected/)
- [AWS Compute Optimizer](https://aws.amazon.com/compute-optimizer/)
- [EC2 Instance Types Comparison](https://instances.vantage.sh/)
- [AWS Pricing Calculator](https://calculator.aws/)

---

## 🤝 Contributing

Feel free to open a PR to add more questions, improve STAR answers, or fix any inaccuracies.

---

*Last Updated: April 2026 | AWS EC2 Interview Guide v2.0*

> ⭐ **Star this repo if it helped you land your dream job!**
