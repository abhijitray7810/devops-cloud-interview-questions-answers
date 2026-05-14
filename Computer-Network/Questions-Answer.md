# 🌐 Top 70+ Computer Networking Interview Questions & Answers
### For DevOps | AWS Cloud | SRE | Platform Engineer Roles

> **Why This Guide?**
> Networking is the backbone of every cloud, DevOps, and SRE role. Whether you're debugging a failing pod, designing a VPC, or troubleshooting why a website is down — every action involves networking. This guide goes beyond definitions: each answer explains **why it matters**, **what problem it solves**, and uses **real scenario-based responses** the way interviewers expect.

---

## 📋 Table of Contents

1. [OSI & TCP/IP Model](#-1-osi--tcpip-model)
2. [TCP vs UDP](#-2-tcp-vs-udp)
3. [IP Addressing & Subnetting](#-3-ip-addressing--subnetting)
4. [DNS — Domain Name System](#-4-dns--domain-name-system)
5. [HTTP / HTTPS & TLS](#-5-http--https--tls)
6. [Routing & Switching](#-6-routing--switching)
7. [Ports & Protocols](#-7-ports--protocols)
8. [Load Balancer & Reverse Proxy](#-8-load-balancer--reverse-proxy)
9. [AWS Networking](#-9-aws-networking)
10. [Kubernetes Networking](#-10-kubernetes-networking)
11. [Linux Networking Commands](#-11-linux-networking-commands)
12. [SSH Networking](#-12-ssh-networking)
13. [Firewalls & Security](#-13-firewalls--security)
14. [CDN & Caching](#-14-cdn--caching)
15. [Troubleshooting Scenarios](#-15-real-world-troubleshooting-scenarios)
16. [The Ultimate URL Question](#-16-the-ultimate-question-what-happens-when-you-type-a-url)

---

## 🔷 1. OSI & TCP/IP Model

> **Why Interviewers Ask This:** Every network tool, protocol, and problem maps to a specific layer. Knowing layers helps you instantly reason about *where* a problem exists — is it DNS (Layer 7)? IP routing (Layer 3)? A physical cable (Layer 1)?

---

### Q1. What are the 7 layers of the OSI model and what does each do?

**Answer:**

| Layer | Name | Function | Real Example |
|-------|------|----------|-------------|
| 7 | Application | User-facing protocols, data format | HTTP, HTTPS, FTP, DNS, SMTP |
| 6 | Presentation | Encryption, encoding, compression | TLS/SSL, JPEG, ASCII |
| 5 | Session | Manages sessions/connections | NetBIOS, RPC |
| 4 | Transport | End-to-end delivery, ports | TCP, UDP |
| 3 | Network | Logical addressing, routing | IP, ICMP, routers |
| 2 | Data Link | MAC addressing, frames, switches | Ethernet, ARP, switches |
| 1 | Physical | Bits over cable/wireless | Cables, NICs, hubs |

**Scenario:** "My application can't connect to the database."
- Layer 7 → Is the DB protocol correct?
- Layer 4 → Is port 5432 open/listening?
- Layer 3 → Can the host route to the DB IP?
- Layer 1/2 → Is the physical/virtual NIC up?

---

### Q2. What is the difference between the OSI model and the TCP/IP model?

**Answer:**

| Feature | OSI Model | TCP/IP Model |
|---------|-----------|--------------|
| Layers | 7 | 4 |
| Purpose | Conceptual/theoretical | Practical implementation |
| Developed by | ISO | DARPA |
| Used for | Troubleshooting framework | Actual internet communication |

TCP/IP layers: Application → Transport → Internet → Network Access

**Why it matters:** OSI is what you use to *diagnose* problems. TCP/IP is what actually *runs* the internet.

---

### Q3. At which layer does a router work? What about a switch?

**Answer:**
- **Router → Layer 3 (Network):** Makes decisions based on IP addresses, routes packets between different networks.
- **Switch → Layer 2 (Data Link):** Makes decisions based on MAC addresses, connects devices within the same network.
- **Layer 7 Load Balancer:** Works at the Application layer — can inspect HTTP headers, URLs, cookies.

**Scenario:** "Traffic isn't reaching my EC2." First check: is the route table (Layer 3) correct? Then check security groups (Layer 4). Then check application health (Layer 7).

---

### Q4. Where does HTTPS operate in the OSI model?

**Answer:**
HTTPS spans multiple layers:
- **Layer 7 (Application):** HTTP protocol itself — requests, responses, headers
- **Layer 6 (Presentation):** TLS encryption/decryption
- **Layer 4 (Transport):** TCP connection on port 443

This is why a TLS handshake failure shows up differently from an HTTP 404 — they're happening at different layers.

---

## 🔷 2. TCP vs UDP

> **Why Interviewers Ask This:** Choosing the wrong transport protocol causes either slow performance (TCP overhead for streaming) or data loss (UDP for critical transactions). Every cloud engineer must know when to use which.

---

### Q5. What is the difference between TCP and UDP?

**Answer:**

| Feature | TCP | UDP |
|---------|-----|-----|
| Connection | Connection-oriented (3-way handshake) | Connectionless |
| Reliability | Guaranteed delivery, ordering, retransmission | Best-effort, no guarantees |
| Speed | Slower (overhead) | Faster |
| Error checking | Yes, with recovery | Yes, but no recovery |
| Use cases | SSH, HTTP, HTTPS, databases | DNS, video streaming, gaming, VoIP |

**Scenario:** "We're building a live video streaming service. Which protocol?"
→ UDP. A few dropped frames are acceptable, but latency must be minimal. TCP's retransmission would cause buffering.

---

### Q6. Why does DNS use UDP instead of TCP?

**Answer:**
DNS queries are small (under 512 bytes typically) and need to be fast. Using TCP would add the overhead of a 3-way handshake for every single DNS lookup — that would make browsing noticeably slow.

**Exception:** DNS uses TCP when:
- The response exceeds 512 bytes (e.g., DNSSEC, large zone transfers)
- Zone transfers between DNS servers (AXFR) always use TCP

**Scenario:** "DNS resolution is slow." Check: Is something forcing DNS to use TCP unnecessarily? Is the DNS server being overwhelmed with UDP floods?

---

### Q7. Why does SSH use TCP?

**Answer:**
SSH is used for secure, interactive terminal sessions — every keystroke must arrive in order and without loss. If a character is dropped, the session becomes corrupted. TCP's guaranteed, ordered delivery is essential here. Speed is secondary to correctness.

---

### Q8. Explain the TCP 3-way handshake.

**Answer:**
```
Client          Server
  |--SYN-------->|   "I want to connect" (seq=x)
  |<--SYN-ACK----|   "OK, I acknowledge" (seq=y, ack=x+1)
  |--ACK-------->|   "Got it, let's talk" (ack=y+1)
  [Connection Established]
```

**Why it matters in DevOps:**
- `SYN_SENT` state → client trying to connect, server unreachable
- `SYN_RECV` overflow → SYN flood attack (DDoS)
- `TIME_WAIT` buildup → too many short-lived connections (connection pool needed)

**Scenario:** "Seeing many `SYN_RECV` entries in `ss -s` output." → Possible SYN flood or firewall dropping ACK packets.

---

## 🔷 3. IP Addressing & Subnetting

> **Why Interviewers Ask This:** In cloud environments, you design VPCs, allocate subnets, configure NAT, and troubleshoot routing — all of which require solid IP and CIDR knowledge. A wrong subnet mask means hosts can't communicate.

---

### Q9. What is CIDR notation and why is it used?

**Answer:**
CIDR (Classless Inter-Domain Routing) notation expresses an IP address and its subnet mask together. Example: `192.168.1.0/24`

- `/24` means the first 24 bits are the network portion → 8 bits for hosts → 2⁸ - 2 = **254 usable hosts**
- `/16` → 16 bits for hosts → 2¹⁶ - 2 = **65,534 usable hosts**
- `/32` → single host (used for Elastic IPs, specific routes)

**Why -2?** One address is the network address (192.168.1.0) and one is the broadcast address (192.168.1.255).

**Scenario:** "Design a VPC for 500 EC2 instances." → Use at least `/23` (510 hosts). A `/24` only gives 254 hosts — not enough.

---

### Q10. How many usable hosts are in a /24, /16, and /28 subnet?

**Answer:**

| CIDR | Total IPs | Usable Hosts | Common Use |
|------|-----------|--------------|------------|
| /24 | 256 | 254 | Small subnets |
| /16 | 65,536 | 65,534 | VPC CIDR blocks |
| /28 | 16 | 14 | Small AWS subnets |
| /32 | 1 | 1 (the host itself) | Specific host routes, EIPs |

**Formula:** Usable hosts = 2^(32 - prefix) - 2

---

### Q11. What is the difference between a public and private IP address?

**Answer:**

**Private IP ranges** (RFC 1918 — not routable on the internet):
- `10.0.0.0/8` — large corporate networks, VPCs
- `172.16.0.0/12` — Docker default bridge network
- `192.168.0.0/16` — home/office networks

**Public IPs** are assigned by ISPs and are routable globally.

**Problem it solves:** IPv4 only has ~4.3 billion addresses. Private IPs + NAT allow millions of devices to share a single public IP.

**Scenario:** "My EC2 in a private subnet can't access the internet." → It has a private IP. It needs a NAT Gateway to translate outbound traffic to a public IP.

---

### Q12. What is NAT and how does it work?

**Answer:**
NAT (Network Address Translation) rewrites the source IP of packets leaving a private network to a public IP, and keeps a translation table to route return traffic back.

```
EC2 (10.0.1.5) → NAT Gateway (52.x.x.x) → Internet
Internet → NAT Gateway → translates back → EC2 (10.0.1.5)
```

**Types:**
- **SNAT (Source NAT):** Private → Public (outbound internet access)
- **DNAT (Destination NAT):** Public → Private (port forwarding, load balancers)
- **PAT (Port Address Translation):** Multiple private IPs share one public IP using different ports

---

## 🔷 4. DNS — Domain Name System

> **Why Interviewers Ask This:** DNS is the phone book of the internet. Almost every outage or connectivity issue touches DNS. "It's always DNS" is a running joke in ops — because it's often true.

---

### Q13. How does DNS resolution work? Walk me through the full flow.

**Answer:**
```
Browser checks local cache
  ↓ (not found)
OS checks /etc/hosts file
  ↓ (not found)
Query sent to Recursive Resolver (e.g., 8.8.8.8)
  ↓
Recursive Resolver checks its cache
  ↓ (not found)
Queries Root Name Server → returns TLD server address
  ↓
Queries TLD Server (.com, .org) → returns Authoritative NS
  ↓
Queries Authoritative Name Server → returns IP address
  ↓
Recursive Resolver caches result, returns IP to browser
  ↓
Browser connects to IP
```

**Why it matters:** If DNS fails, nothing works even if your servers are perfectly healthy. Always check DNS first in any "website down" scenario.

---

### Q14. What is the difference between an A record and a CNAME record?

**Answer:**

| Record | Points To | Example |
|--------|-----------|---------|
| **A** | IPv4 address directly | `api.example.com → 54.23.1.5` |
| **AAAA** | IPv6 address | `api.example.com → 2001:db8::1` |
| **CNAME** | Another domain name (alias) | `www.example.com → example.com` |
| **MX** | Mail server | `example.com → mail.example.com` |
| **TXT** | Text data | SPF, DKIM, domain verification |
| **NS** | Authoritative name servers | `example.com → ns1.awsdns.com` |

**Important rule:** A CNAME **cannot** be used at the apex/root domain (e.g., `example.com`). This is why AWS Route 53 has ALIAS records — they behave like CNAMEs but work at the root.

**Scenario:** "I changed our domain to point to a new load balancer IP. Users still see the old site." → Check TTL. If TTL was 86400 (24 hours), old DNS cache is still being served. Lower TTL before migrations!

---

### Q15. What is TTL in DNS and why does it matter?

**Answer:**
TTL (Time To Live) is the number of seconds a DNS record is cached by resolvers and clients.

- **High TTL (86400 = 24h):** Fewer DNS queries, faster resolution, but slow propagation during changes
- **Low TTL (60 = 1 min):** Fast propagation during changes, but more DNS load and slightly higher latency

**Best practice for migrations:**
1. Lower TTL to 60 seconds 24–48 hours before the change
2. Make the DNS change
3. Wait for old TTL to expire
4. Raise TTL back after confirming

---

### Q16. What is the difference between a recursive resolver and an authoritative name server?

**Answer:**
- **Recursive Resolver** (e.g., 8.8.8.8, 1.1.1.1): Does the "hunting" work — asks multiple servers on your behalf, caches results. This is what your OS sends queries to.
- **Authoritative Name Server:** Holds the actual DNS records for a domain. It gives the final, definitive answer. (e.g., Route 53 for AWS-hosted domains)

**Analogy:** Recursive resolver = librarian who goes and finds the book. Authoritative server = the book itself.

---

## 🔷 5. HTTP / HTTPS & TLS

> **Why Interviewers Ask This:** Every web request, API call, and microservice communication uses HTTP/HTTPS. Understanding status codes, TLS, and proxies is essential for building and debugging production systems.

---

### Q17. What is the difference between HTTP and HTTPS?

**Answer:**

| Feature | HTTP | HTTPS |
|---------|------|-------|
| Port | 80 | 443 |
| Encryption | None — plaintext | TLS/SSL encrypted |
| Authentication | None | Server verified via certificate |
| SEO | Penalized by Google | Preferred |
| Use case | Internal/legacy | All production web traffic |

**Problem HTTPS solves:** Without it, anyone on the network (ISP, café WiFi) can read usernames, passwords, and credit card numbers in transit — a man-in-the-middle attack.

---

### Q18. Walk me through the TLS handshake.

**Answer:**
```
Client                          Server
  |--ClientHello---------------->|  (TLS version, cipher suites, random)
  |<--ServerHello----------------|  (chosen cipher, server random)
  |<--Certificate----------------|  (server's SSL cert with public key)
  |<--ServerHelloDone------------|
  |--ClientKeyExchange----------->|  (pre-master secret, encrypted with server's public key)
  Both sides derive the same session key (symmetric)
  |--ChangeCipherSpec------------>|
  |--Finished (encrypted)-------->|
  |<--ChangeCipherSpec------------|
  |<--Finished (encrypted)--------|
  [Secure channel established — HTTP traffic flows encrypted]
```

**Key concept:** Asymmetric encryption (slow) is only used to establish the session. After that, fast symmetric encryption handles actual data.

**Scenario:** "SSL certificate error in production." Steps:
1. Check cert expiry: `echo | openssl s_client -connect host:443 2>/dev/null | openssl x509 -noout -dates`
2. Check if cert matches domain (wildcard or SAN)
3. Check cert chain is complete (intermediate CA included)

---

### Q19. What are the most important HTTP status codes?

**Answer:**

| Code | Meaning | DevOps Scenario |
|------|---------|-----------------|
| 200 | OK | Request succeeded |
| 201 | Created | Resource created (POST) |
| 301 | Moved Permanently | Old URL redirects to new — update links |
| 302 | Found (Temporary Redirect) | A/B testing, maintenance page |
| 400 | Bad Request | Client sent malformed data |
| 401 | Unauthorized | Missing/invalid auth token |
| 403 | Forbidden | Valid auth but insufficient permissions |
| 404 | Not Found | Wrong URL, check your DNS/routing |
| 429 | Too Many Requests | Rate limiting triggered |
| 500 | Internal Server Error | App crashed — check logs |
| 502 | Bad Gateway | Load balancer can't reach upstream server |
| 503 | Service Unavailable | Service down or overloaded |
| 504 | Gateway Timeout | Upstream server too slow |

**Scenario:** "Users getting 502 errors." → Your load balancer is up but backend instances are down. Check: health checks, target group registration, application logs.

---

### Q20. What is a reverse proxy and how is it different from a load balancer?

**Answer:**

**Reverse Proxy:** Sits in front of servers, forwards client requests to backend servers. Hides backend topology. Examples: NGINX, Caddy.

**Load Balancer:** Distributes traffic across multiple backend servers for availability and scale. Examples: AWS ALB, HAProxy.

**Key insight:** A load balancer IS a type of reverse proxy, but a reverse proxy doesn't have to be a load balancer.

```
Client → [Reverse Proxy / Load Balancer] → [Backend Server 1]
                                          → [Backend Server 2]
                                          → [Backend Server 3]
```

**What reverse proxy adds beyond load balancing:**
- SSL termination
- Caching
- Compression
- Path-based routing (`/api` → service A, `/web` → service B)
- Rate limiting
- Authentication

---

### Q21. What HTTP methods do you know and when are they used?

**Answer:**

| Method | Purpose | Safe? | Idempotent? |
|--------|---------|-------|-------------|
| GET | Retrieve data | Yes | Yes |
| POST | Create new resource | No | No |
| PUT | Replace entire resource | No | Yes |
| PATCH | Update part of resource | No | No |
| DELETE | Remove resource | No | Yes |
| HEAD | Like GET but no body | Yes | Yes |
| OPTIONS | Check allowed methods (CORS) | Yes | Yes |

---

## 🔷 6. Routing & Switching

> **Why Interviewers Ask This:** When EC2 instances can't talk to each other, or a subnet can't reach the internet, it's almost always a routing problem. Understanding ARP, route tables, and VLANs is fundamental.

---

### Q22. What is the difference between a router and a switch?

**Answer:**

| | Router | Switch |
|-|--------|--------|
| Layer | Layer 3 (Network) | Layer 2 (Data Link) |
| Uses | IP addresses | MAC addresses |
| Connects | Different networks | Devices in same network |
| Example | Home router, AWS Route Table | Office Ethernet switch |

**Scenario:** "Two EC2 instances in the same VPC subnet can't ping each other." → This is a Layer 2/switch issue. Check security group rules (AWS's virtual switch has SGs). This is NOT a routing problem since they're on the same subnet.

---

### Q23. How does ARP work?

**Answer:**
ARP (Address Resolution Protocol) maps IP addresses to MAC addresses within a local network.

**Flow:**
```
Host A wants to send to 192.168.1.10 (Host B)
Host A checks its ARP cache — not found
Host A broadcasts: "Who has 192.168.1.10? Tell 192.168.1.5"
Host B responds: "192.168.1.10 is at MAC aa:bb:cc:dd:ee:ff"
Host A caches this and sends the frame directly to Host B's MAC
```

**Why it matters:** ARP spoofing/poisoning is a common attack where a malicious host responds to ARP requests with its own MAC → man-in-the-middle attack.

**Linux commands:**
```bash
arp -a           # view ARP table
ip neigh show    # modern equivalent
```

---

### Q24. What is a VLAN and why is it used?

**Answer:**
VLAN (Virtual LAN) logically segments a physical network into separate broadcast domains without needing separate physical switches.

**Problems it solves:**
- **Security:** HR traffic isolated from Engineering traffic on the same physical switch
- **Performance:** Reduces broadcast traffic — broadcasts don't cross VLAN boundaries
- **Organization:** Logical grouping of devices regardless of physical location

**AWS equivalent:** Subnets within a VPC serve a similar purpose — they segment network traffic.

---

## 🔷 7. Ports & Protocols

> **Why Interviewers Ask This:** "netstat shows port 3306 not listening" — you need to instantly know that means MySQL isn't running. Port knowledge speeds up troubleshooting dramatically.

---

### Q25. List the most important ports you must know.

**Answer:**

| Port | Protocol | Service |
|------|---------|---------|
| 20/21 | TCP | FTP (data/control) |
| 22 | TCP | SSH, SCP, SFTP |
| 23 | TCP | Telnet (insecure, avoid) |
| 25 | TCP | SMTP (email sending) |
| 53 | TCP/UDP | DNS |
| 67/68 | UDP | DHCP |
| 80 | TCP | HTTP |
| 110 | TCP | POP3 (email) |
| 143 | TCP | IMAP (email) |
| 443 | TCP | HTTPS |
| 465/587 | TCP | SMTP over SSL/TLS |
| 3306 | TCP | MySQL |
| 5432 | TCP | PostgreSQL |
| 6379 | TCP | Redis |
| 27017 | TCP | MongoDB |
| 2181 | TCP | ZooKeeper |
| 9092 | TCP | Kafka |
| 8080 | TCP | HTTP alternate / app servers |
| 6443 | TCP | Kubernetes API Server |
| 2379/2380 | TCP | etcd |
| 10250 | TCP | Kubelet API |

---

### Q26. How do you check which ports are open/listening on a Linux server?

**Answer:**
```bash
# Modern approach (preferred)
ss -tlnp           # TCP listening ports with process names
ss -ulnp           # UDP listening ports

# Classic approach
netstat -tlnp      # same as ss but older tool

# Check specific port
ss -tlnp | grep :443
lsof -i :443       # shows process using the port

# Check if remote port is open
nc -zv 10.0.1.5 5432          # Test PostgreSQL connectivity
telnet 10.0.1.5 22            # Test SSH port
curl -v telnet://10.0.1.5:22  # Test with curl
```

**Scenario:** "Application can't connect to Redis on another server."
```bash
# On the Redis server
ss -tlnp | grep 6379    # Is Redis listening?

# From application server
nc -zv redis-host 6379  # Can we reach the port?
```

---

## 🔷 8. Load Balancer & Reverse Proxy

> **Why Interviewers Ask This:** High availability and horizontal scaling are core DevOps principles. Load balancers are how you achieve both. Every production system uses them.

---

### Q27. What is the difference between an L4 and L7 load balancer?

**Answer:**

| | L4 Load Balancer | L7 Load Balancer |
|-|-----------------|-----------------|
| OSI Layer | Transport (Layer 4) | Application (Layer 7) |
| Sees | IP + Port | Full HTTP request (URL, headers, cookies) |
| Routing basis | IP/port rules | URL path, hostname, HTTP headers |
| Speed | Faster (less processing) | Slower (more processing) |
| SSL termination | No (pass-through) | Yes |
| AWS equivalent | NLB (Network Load Balancer) | ALB (Application Load Balancer) |
| Use case | TCP/UDP apps, games, IoT | Web apps, microservices, APIs |

**Scenario:** "Route `/api` traffic to API servers and `/static` to CDN origin."
→ Must use **L7 (ALB)** — only it can inspect the URL path.

---

### Q28. What is a sticky session and when would you use it?

**Answer:**
Sticky sessions (session affinity) ensure a user's requests always go to the same backend server.

**How it works:** The load balancer sets a cookie. On subsequent requests, it routes based on that cookie.

**When to use:**
- Legacy stateful apps that store session data in-memory (not in a shared DB/Redis)
- WebSocket connections that must stay on the same server

**Problems with sticky sessions:**
- Uneven load distribution (one server gets a power user)
- If the server goes down, all its sessions are lost anyway

**Best practice:** Design stateless apps (store sessions in Redis/DynamoDB) so sticky sessions aren't needed.

---

### Q29. What is the difference between active-active and active-passive load balancing?

**Answer:**

**Active-Active:** All servers handle traffic simultaneously. Load is distributed across all.
- Higher throughput, better resource utilization
- More complex failover logic

**Active-Passive:** One server handles traffic; the other is on standby waiting to take over.
- Simpler failover
- Standby resources are "wasted" during normal operation
- Used for databases (primary + replica failover)

---

## 🔷 9. AWS Networking

> **Why Interviewers Ask This:** AWS cloud networking is the bread and butter of every cloud DevOps role. VPC design, security groups, and NAT are tested in almost every interview.

---

### Q30. What is a VPC and why do you need it?

**Answer:**
VPC (Virtual Private Cloud) is your own isolated, private network within AWS. It's logically separated from other AWS customers' networks.

**What you control:**
- IP address range (CIDR block, e.g., `10.0.0.0/16`)
- Subnets (public and private)
- Route tables
- Internet gateways
- Security rules (Security Groups + NACLs)

**Problem it solves:** Without VPC, all your AWS resources would be on a flat shared network — no isolation, no security. VPC gives you the equivalent of your own data center network inside AWS.

---

### Q31. What is the difference between a public subnet and a private subnet in AWS?

**Answer:**

| | Public Subnet | Private Subnet |
|-|--------------|----------------|
| Internet access | Direct via Internet Gateway | Via NAT Gateway only |
| Resources | Load balancers, bastion hosts | EC2 app servers, RDS databases |
| Route table has | Route to Internet Gateway (0.0.0.0/0 → IGW) | Route to NAT Gateway |
| Public IP | Resources can have public IPs | Resources have only private IPs |

**Typical 3-tier architecture:**
```
Internet → [Public Subnet: ALB] → [Private Subnet: App Servers] → [Private Subnet: RDS]
```

---

### Q32. What is the difference between a Security Group and a NACL?

**Answer:**

| Feature | Security Group | NACL (Network ACL) |
|---------|---------------|-------------------|
| Level | Instance level (ENI) | Subnet level |
| State | **Stateful** — return traffic auto-allowed | **Stateless** — must explicitly allow both directions |
| Rules | Allow only | Allow AND Deny |
| Evaluation | All rules evaluated | Rules evaluated in order (numbered) |
| Default | Deny all inbound, allow all outbound | Allow all traffic |

**Scenario:** "EC2 can't receive HTTPS traffic."
1. Check Security Group: Is port 443 open for inbound?
2. Check NACL: Is port 443 allowed inbound AND outbound (for response traffic)?

**Common mistake:** Opening inbound port 443 in NACL but forgetting to allow outbound ephemeral ports (1024-65535) for return traffic.

---

### Q33. How does a private subnet access the internet in AWS?

**Answer:**
Through a **NAT Gateway** placed in a **public subnet**.

```
EC2 (private subnet, 10.0.2.5)
  → Route table: 0.0.0.0/0 → NAT Gateway (in public subnet)
  → NAT Gateway translates source IP to its Elastic IP
  → Internet Gateway routes traffic to internet
  → Return traffic follows reverse path
```

**Key points:**
- NAT Gateway must be in a public subnet
- NAT Gateway has an Elastic IP (public IP)
- EC2 can initiate outbound connections; internet cannot initiate inbound connections to EC2
- NAT Gateway is managed by AWS — no patching needed

---

### Q34. What is VPC Peering and when would you use it?

**Answer:**
VPC Peering connects two VPCs privately using AWS's backbone network — no internet, no VPN, no encryption overhead.

**Use cases:**
- Connect production VPC to shared-services VPC (logging, monitoring)
- Connect different account VPCs within an organization
- Connect VPCs across regions

**Important limitations:**
- **No transitive routing:** If A↔B and B↔C, A cannot reach C through B
- CIDR ranges must not overlap
- For hub-and-spoke topology with many VPCs, use **AWS Transit Gateway** instead

---

### Q35. What is the difference between Internet Gateway and NAT Gateway?

**Answer:**

| | Internet Gateway (IGW) | NAT Gateway |
|-|------------------------|-------------|
| Traffic direction | Both inbound and outbound | Outbound only |
| Who uses it | Public subnets | Private subnets |
| Attached to | VPC | Subnet (must be public subnet) |
| Public IP required? | EC2 must have public IP | NAT GW has Elastic IP |
| Cost | Free | Charged per hour + data |

---

## 🔷 10. Kubernetes Networking

> **Why Interviewers Ask This:** Kubernetes networking is famously complex. Pod networking, Services, and Ingress are the most common areas where things break in production.

---

### Q36. How do pods communicate with each other in Kubernetes?

**Answer:**
Every pod gets its own unique IP address. Kubernetes guarantees:
- Any pod can communicate with any other pod **without NAT**
- Node-to-pod and pod-to-pod communication uses flat networking

This is implemented by **CNI (Container Network Interface)** plugins like:
- **Flannel:** Simple overlay network
- **Calico:** Policy-aware, supports NetworkPolicies
- **Cilium:** eBPF-based, high performance
- **WeaveNet:** Encrypted mesh

**Same node:** Pods communicate via virtual Ethernet bridge
**Different nodes:** Traffic goes through the CNI overlay network

---

### Q37. What is the difference between ClusterIP, NodePort, and LoadBalancer services?

**Answer:**

| Service Type | Accessible From | Use Case |
|-------------|----------------|----------|
| **ClusterIP** | Inside cluster only | Internal microservice communication |
| **NodePort** | Outside cluster via NodeIP:Port (30000-32767) | Dev/testing, direct node access |
| **LoadBalancer** | Outside cluster via cloud LB | Production external access |
| **ExternalName** | DNS alias only | Point to external service by DNS |

**Scenario:** "Frontend pods can't reach backend pods."
```bash
# Check if service exists and has endpoints
kubectl get svc backend-service
kubectl get endpoints backend-service   # must show pod IPs
kubectl describe svc backend-service    # check selector matches pod labels
```

---

### Q38. What is an Ingress in Kubernetes?

**Answer:**
Ingress is an API object that manages external HTTP/HTTPS traffic routing into the cluster. It acts as a Layer 7 load balancer and reverse proxy.

**What Ingress provides that Services don't:**
- Host-based routing: `api.example.com` → API service, `www.example.com` → frontend service
- Path-based routing: `/api` → backend, `/` → frontend
- SSL/TLS termination
- Single external IP for multiple services

**Requires an Ingress Controller** (e.g., NGINX Ingress Controller, AWS ALB Ingress Controller) to be installed.

```yaml
# Example Ingress
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: my-ingress
spec:
  rules:
  - host: api.example.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: api-service
            port:
              number: 80
```

---

### Q39. What is kube-proxy and what does it do?

**Answer:**
kube-proxy runs on every node and implements Kubernetes Service networking. It maintains network rules (iptables or IPVS rules) that allow traffic destined for a Service ClusterIP to be redirected to the actual pod IPs.

**How it works:**
```
Request to ClusterIP (10.96.0.1:80)
  → kube-proxy's iptables rules intercept
  → DNAT to a random pod IP (10.244.1.5:8080)
  → Pod receives traffic
```

---

### Q40. What is a NetworkPolicy in Kubernetes?

**Answer:**
NetworkPolicy is a Kubernetes resource that controls which pods can communicate with each other (pod-to-pod firewall rules). By default, all pods can talk to all pods.

**Scenario:** "Our payment service should ONLY accept traffic from the checkout service, nothing else."
```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: payment-isolation
spec:
  podSelector:
    matchLabels:
      app: payment
  ingress:
  - from:
    - podSelector:
        matchLabels:
          app: checkout
```

**Note:** NetworkPolicies require a CNI that supports them (Calico, Cilium) — Flannel alone does NOT enforce them.

---

## 🔷 11. Linux Networking Commands

> **Why Interviewers Ask This:** DevOps is hands-on. You'll be asked "How would you debug this?" — the answer is always a sequence of commands. Know these cold.

---

### Q41. How do you debug network connectivity issues on Linux?

**Answer — The systematic approach:**

```bash
# Step 1: Check if the interface is up
ip addr show            # list all interfaces and IPs
ip link show            # check interface state (UP/DOWN)

# Step 2: Check routing
ip route show           # what's the default gateway?
ip route get 8.8.8.8   # which route would traffic to 8.8.8.8 take?

# Step 3: Test local network (Layer 3)
ping -c 4 192.168.1.1   # can I reach the gateway?
ping -c 4 8.8.8.8       # can I reach the internet by IP?

# Step 4: Test DNS (Layer 7)
dig google.com           # full DNS resolution detail
nslookup google.com      # simpler DNS test
cat /etc/resolv.conf     # what DNS servers am I using?

# Step 5: Test specific port connectivity
nc -zv target-host 443  # is port 443 reachable?
curl -v https://example.com  # full HTTP test

# Step 6: Trace the path
traceroute google.com    # where does traffic stop?
tracepath google.com     # similar, no root required

# Step 7: Deep packet inspection
tcpdump -i eth0 port 80  # capture HTTP traffic
tcpdump -i eth0 host 10.0.1.5  # capture traffic to/from specific host
```

---

### Q42. How do you test DNS resolution from Linux?

**Answer:**
```bash
# Most detailed — shows full resolution path
dig google.com
dig @8.8.8.8 google.com    # query specific DNS server
dig google.com +trace       # trace full resolution chain
dig -x 8.8.8.8              # reverse DNS lookup

# Simpler tool
nslookup google.com
nslookup google.com 8.8.8.8  # query specific server

# Check what DNS server the system is using
cat /etc/resolv.conf
systemd-resolve --status | grep DNS

# Flush DNS cache
systemd-resolve --flush-caches
```

**Scenario:** "App resolves wrong IP for internal service."
```bash
dig service-name.namespace.svc.cluster.local  # in K8s
# Check if /etc/resolv.conf points to correct DNS
```

---

### Q43. How do you use tcpdump to capture and analyze traffic?

**Answer:**
```bash
# Capture all traffic on eth0
tcpdump -i eth0

# Capture HTTP traffic
tcpdump -i eth0 port 80

# Capture traffic to/from specific host
tcpdump -i eth0 host 10.0.1.5

# Save to file for Wireshark analysis
tcpdump -i eth0 -w capture.pcap

# Read saved capture
tcpdump -r capture.pcap

# Capture DNS queries
tcpdump -i eth0 port 53

# Verbose with no DNS resolution (faster output)
tcpdump -i eth0 -nn port 443
```

**Scenario:** "We suspect someone is making unexpected outbound connections." 
```bash
tcpdump -i eth0 -nn 'not port 22 and not port 443 and not port 80' > suspicious.txt
```

---

### Q44. Explain the difference between `netstat` and `ss`.

**Answer:**
Both show socket/connection information, but `ss` is the modern replacement:
- `ss` is faster (reads from kernel directly)
- `netstat` reads from `/proc/net` (slower on systems with many connections)
- Same flags work for both

```bash
ss -tlnp    # TCP, listening, numeric, show process
ss -s       # summary statistics
ss -o state ESTABLISHED  # only established connections
ss -tnp dst :443         # connections to port 443
```

---

## 🔷 12. SSH Networking

> **Why Interviewers Ask This:** SSH is the primary way DevOps engineers access servers. Understanding key auth, tunneling, and the handshake shows operational maturity.

---

### Q45. How does SSH key-based authentication work?

**Answer:**
```
Key generation: ssh-keygen → creates id_rsa (private) + id_rsa.pub (public)

Setup: Public key added to server's ~/.ssh/authorized_keys

Authentication flow:
1. Client connects and says "I want to auth with this public key"
2. Server checks authorized_keys — finds matching key
3. Server sends a challenge (random data encrypted with client's public key)
4. Client decrypts with private key, sends back proof
5. Server verifies — auth succeeds without ever transmitting the password
```

**Why it's better than passwords:**
- Private key never leaves your machine
- Not vulnerable to brute force (no password to guess)
- Can be automated without passwords in scripts

---

### Q46. What is SSH tunneling and when would you use it?

**Answer:**
SSH tunneling forwards TCP traffic through an encrypted SSH connection.

**Local forwarding (access remote service locally):**
```bash
ssh -L 5432:db-host:5432 bastion-host
# Now connect to localhost:5432 to reach db-host:5432 via bastion
```

**Remote forwarding (expose local port to remote host):**
```bash
ssh -R 8080:localhost:3000 remote-server
# Remote server can reach your local:3000 via remote:8080
```

**Dynamic forwarding (SOCKS proxy):**
```bash
ssh -D 1080 bastion-host
# Configure browser to use SOCKS5 proxy localhost:1080
# All browser traffic tunnels through bastion
```

**Use cases:**
- Access private RDS/Redis from local machine via bastion
- Bypass firewall restrictions
- Secure access to internal dashboards

---

### Q47. What happens during an SSH connection? Walk me through the handshake.

**Answer:**
1. **TCP connection** established to port 22
2. **Version exchange**: Both sides announce SSH protocol version
3. **Algorithm negotiation**: Agree on key exchange, encryption, MAC algorithms
4. **Key exchange** (Diffie-Hellman): Establish shared session key without transmitting it
5. **Server authentication**: Server proves identity with its host key (prevents MITM)
6. **User authentication**: Password, public key, or other method
7. **Session opened**: Encrypted channel ready for commands/data

---

## 🔷 13. Firewalls & Security

> **Why Interviewers Ask This:** Misconfigurations in firewalls are the #1 cause of "can't connect" issues. Understanding stateful vs stateless, iptables, and AWS security is critical.

---

### Q48. What is the difference between a stateful and stateless firewall?

**Answer:**

**Stateless Firewall:** Evaluates each packet independently against rules. Has no memory of previous packets.
- Must explicitly allow both inbound AND outbound for a conversation
- AWS NACLs are stateless

**Stateful Firewall:** Tracks connection state. If you allow a connection initiation, return traffic is automatically allowed.
- Only need to allow inbound — response is automatically permitted
- AWS Security Groups are stateful
- Most modern firewalls and iptables (with connection tracking) are stateful

**Practical example:**
- **NACL:** Allow inbound port 443 → ALSO must allow outbound ephemeral ports (1024-65535) for responses
- **Security Group:** Allow inbound port 443 → responses automatically allowed

---

### Q49. How does iptables work?

**Answer:**
iptables processes packets through **chains** in **tables**:

**Tables:** filter (main), nat (address translation), mangle (modify packets)

**Chains in filter table:**
- `INPUT`: Packets destined for this host
- `OUTPUT`: Packets originating from this host
- `FORWARD`: Packets routed through this host

```bash
# View current rules
iptables -L -n -v

# Allow inbound SSH
iptables -A INPUT -p tcp --dport 22 -j ACCEPT

# Block outbound to specific IP
iptables -A OUTPUT -d 1.2.3.4 -j DROP

# Allow established connections (stateful)
iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT

# Save rules (persist across reboot)
iptables-save > /etc/iptables/rules.v4
```

---

### Q50. How is `ufw` different from `iptables`?

**Answer:**
`ufw` (Uncomplicated Firewall) is a simplified frontend for iptables — it writes iptables rules under the hood but with a much friendlier syntax.

```bash
ufw enable
ufw allow 22/tcp
ufw allow 443/tcp
ufw deny 23/tcp
ufw status verbose
```

**When to use what:**
- `ufw`: Quick setup for simple server firewall rules
- `iptables`: Complex rules, NAT, Docker/Kubernetes networking (Docker modifies iptables directly)
- `nftables`: Modern replacement for iptables (newer distros)

---

## 🔷 14. CDN & Caching

> **Why Interviewers Ask This:** CDNs reduce latency by serving content from edge locations near users. Platform and SRE engineers must understand cache invalidation and edge network behavior.

---

### Q51. What is a CDN and how does it work?

**Answer:**
CDN (Content Delivery Network) is a globally distributed network of servers (edge locations/PoPs) that cache and serve content close to end users.

**Request flow without CDN:**
```
User in Mumbai → Server in Virginia → 200ms latency
```

**Request flow with CDN:**
```
User in Mumbai → CDN edge in Mumbai → 5ms latency (if cached)
               → If not cached: edge fetches from origin → caches → serves
```

**What CDNs cache:** Static assets (JS, CSS, images), API responses (if headers allow), HTML pages

**Major CDNs:** Cloudflare, AWS CloudFront, Akamai, Fastly

---

### Q52. What are cache-control headers and how do they affect CDN behavior?

**Answer:**

| Header | Effect |
|--------|--------|
| `Cache-Control: public, max-age=86400` | CDN + browser cache for 24 hours |
| `Cache-Control: private` | Only browser caches (not CDN) |
| `Cache-Control: no-cache` | Must revalidate with origin before serving |
| `Cache-Control: no-store` | Never cache (sensitive data) |
| `ETag` | Fingerprint for conditional requests |
| `Vary: Accept-Encoding` | Cache different versions for gzip/non-gzip |

**Scenario:** "After deploying new JavaScript, users still see the old version."
→ CDN has stale cache. Solutions:
1. Invalidate/purge cache in CDN dashboard
2. Use cache busting: rename files with hash (`app.abc123.js`)
3. Set appropriate `max-age` for different content types

---

## 🔷 15. Real-World Troubleshooting Scenarios

> **Why Interviewers Ask This:** Real DevOps work IS troubleshooting. They want to see your systematic thinking, not just memorized answers.

---

### Q53. A website is down. Walk me through your investigation.

**Answer — Full systematic approach:**

```bash
# 1. Check DNS resolution
dig example.com +short          # Does it resolve?
nslookup example.com            # Alternative check

# 2. Check network connectivity
ping -c 4 <resolved-ip>         # Is the host reachable?
traceroute example.com          # Where does traffic stop?

# 3. Check if the port is open
nc -zv example.com 443          # Is HTTPS port reachable?
curl -v https://example.com     # Full HTTP test

# 4. Check SSL certificate
echo | openssl s_client -connect example.com:443 2>/dev/null | openssl x509 -noout -dates

# 5. Check load balancer health (if AWS)
# → AWS Console → EC2 → Load Balancers → Target Groups → Health checks

# 6. Check application logs
# → CloudWatch Logs / ELK / application log files

# 7. Check resource utilization
top / htop                      # CPU/memory
df -h                           # Disk space (full disk = crash)
free -h                         # Memory
```

**Answer format in interview:**
"I'd start from the outside in — first verify DNS, then network connectivity, then SSL, then HTTP response, then application health, then infrastructure. This eliminates entire layers of the stack quickly."

---

### Q54. DNS is not resolving for your application pods in Kubernetes.

**Answer:**

```bash
# 1. Check CoreDNS is running
kubectl get pods -n kube-system | grep coredns
kubectl logs -n kube-system coredns-<id>

# 2. Test DNS from within a pod
kubectl exec -it my-pod -- nslookup kubernetes.default
kubectl exec -it my-pod -- cat /etc/resolv.conf

# 3. Check CoreDNS ConfigMap
kubectl get configmap coredns -n kube-system -o yaml

# 4. Check if the Service exists
kubectl get svc my-service
kubectl get endpoints my-service    # Must have pod IPs listed

# 5. Test full DNS resolution
kubectl exec -it my-pod -- dig my-service.my-namespace.svc.cluster.local
```

**Common causes:**
- CoreDNS pods crashed/OOMKilled → increase memory limits
- Service selector doesn't match pod labels → no endpoints
- Wrong DNS name format (must use FQDN for cross-namespace)
- NetworkPolicy blocking DNS (port 53) traffic

---

### Q55. An EC2 instance in a private subnet can't access the internet.

**Answer:**

```bash
# Step 1: From EC2, test connectivity
ping 8.8.8.8        # Can reach internet IP?
curl ifconfig.me    # What's my public IP? (via NAT GW)

# Step 2: Check route table (AWS Console / CLI)
aws ec2 describe-route-tables --filters "Name=association.subnet-id,Values=subnet-xxxx"
# Should have: 0.0.0.0/0 → nat-xxxxxxxx

# Step 3: Verify NAT Gateway
aws ec2 describe-nat-gateways
# State must be "available"
# NAT GW must be in a PUBLIC subnet

# Step 4: Check NAT Gateway's subnet route table
# Its subnet must have: 0.0.0.0/0 → igw-xxxxxxxx

# Step 5: Check Security Group
# Outbound rules: Allow 0.0.0.0/0 or at least HTTPS/HTTP

# Step 6: Check NACL
# Allow outbound AND inbound ephemeral ports (1024-65535)
```

**Root causes in order of frequency:**
1. Route table missing NAT Gateway route
2. NAT Gateway in wrong (private) subnet
3. No Internet Gateway attached to VPC
4. Security Group blocking outbound
5. NACL blocking ephemeral return ports

---

### Q56. High latency on your web application. How do you investigate?

**Answer:**

```bash
# 1. Network latency check
ping -c 100 server-ip | tail -5   # Average/min/max latency
mtr server-ip                      # Continuous traceroute + stats

# 2. Application layer check
curl -w "@curl-format.txt" -o /dev/null -s https://example.com
# Measure: dns_resolution, tcp_connect, ssl_handshake, time_to_first_byte, total_time

# 3. On the server — check load
top                    # CPU-bound?
iostat -x 1            # I/O-bound?
ss -s                  # Connection count

# 4. Check for packet loss
ping -c 1000 server-ip | grep "packet loss"

# 5. Application profiling
# → Check slow queries in DB (EXPLAIN ANALYZE)
# → Check APM (Datadog, New Relic) for slow traces
# → Check if it's CDN latency vs origin latency
```

**Common causes:**
- DB slow queries (most common)
- Network congestion / packet loss
- Server CPU/memory saturation
- Missing CDN for static assets
- Unoptimized DNS (high TTL lookups)

---

### Q57. A pod can't connect to a database. How do you debug?

**Answer:**

```bash
# 1. Is the DB service resolvable?
kubectl exec -it app-pod -- nslookup db-service
kubectl exec -it app-pod -- nslookup db-service.db-namespace.svc.cluster.local

# 2. Can we reach the DB port?
kubectl exec -it app-pod -- nc -zv db-service 5432
kubectl exec -it app-pod -- telnet db-service 5432

# 3. Check DB service and endpoints
kubectl get svc db-service -n db-namespace
kubectl get endpoints db-service -n db-namespace   # Must have IPs

# 4. Check NetworkPolicy
kubectl get networkpolicies -n db-namespace
# Is there a policy blocking our app pods?

# 5. Check DB pod logs
kubectl logs db-pod -n db-namespace | grep ERROR

# 6. Check credentials/secrets
kubectl get secret db-credentials -o yaml
# Are they mounted correctly in the app pod?
```

---

## 🔷 16. The Ultimate Question: What Happens When You Type a URL?

> **Why Interviewers Ask This:** This single question tests your knowledge of DNS, TCP, TLS, HTTP, routing, load balancing, and CDN — the entire networking stack in one scenario.

---

### Q58. What happens when you type `https://www.google.com` and press Enter?

**Answer — The complete flow:**

#### Phase 1: DNS Resolution
```
1. Browser checks its DNS cache (from previous visits)
2. OS checks /etc/hosts file
3. OS queries DNS resolver (configured in /etc/resolv.conf, e.g., 8.8.8.8)
4. Recursive resolver checks its cache
5. If miss: queries root nameserver → .com TLD server → google.com authoritative NS
6. Returns IP: 142.250.180.46 (example)
7. Result cached with TTL
```

#### Phase 2: TCP Connection
```
8. Browser initiates TCP connection to 142.250.180.46:443
9. 3-way handshake: SYN → SYN-ACK → ACK
10. TCP connection established
```

#### Phase 3: TLS Handshake
```
11. Client Hello: sends supported TLS versions, cipher suites
12. Server Hello: picks TLS 1.3, sends certificate
13. Certificate verification: browser checks CA signature, expiry, domain match
14. Key exchange (Diffie-Hellman): establish session keys
15. Both sides confirm: encrypted channel established
```

#### Phase 4: HTTP Request & Routing
```
16. Browser sends: GET / HTTP/2 with headers (Host: www.google.com, cookies, etc.)
17. Request hits Google's CDN/edge node closest to you
18. If CDN has cached response → served immediately
19. If not → forwarded to Google's load balancer
20. Load balancer selects a backend server (health-checked, least-connections)
21. Backend processes request
```

#### Phase 5: Response
```
22. Server generates HTML response
23. Response sent back through LB → CDN → TLS channel → TCP → to browser
24. Browser parses HTML, discovers CSS/JS/image resources
25. Browser makes additional requests (often parallel, HTTP/2 multiplexing)
26. CDN serves static assets from edge locations
27. Page renders
```

**Why this question matters:** In an actual incident — "users can't access our site" — you walk through exactly this flow to find where it breaks.

---

## 🔷 Additional Must-Know Questions

### Q59. What is BGP and why does it matter in cloud networking?

**Answer:**
BGP (Border Gateway Protocol) is the routing protocol of the internet — it determines how traffic is routed between autonomous systems (large networks, ISPs, cloud providers).

**In cloud/DevOps context:**
- AWS uses BGP for Direct Connect (private line to AWS)
- BGP route leaks can cause massive internet outages (e.g., Facebook 2021 outage)
- Route 53 / Anycast uses BGP to route users to nearest edge location

---

### Q60. What is Anycast and how does Cloudflare/Route 53 use it?

**Answer:**
Anycast assigns the same IP address to multiple servers globally. Routing automatically directs traffic to the nearest node.

**How:** Multiple servers advertise the same IP via BGP. BGP naturally routes to the closest AS.

**Result:** `1.1.1.1` (Cloudflare DNS) is actually hosted in 200+ locations. Your DNS query automatically goes to the nearest one — sub-millisecond response.

---

### Q61. What is MTU and what is an MTU mismatch?

**Answer:**
MTU (Maximum Transmission Unit) is the largest packet size that can be transmitted on a network segment. Standard Ethernet MTU = **1500 bytes**.

**MTU mismatch symptom:** Large packets work but large file transfers, HTTPS, or VPN connections randomly fail or are very slow.

**Diagnosis:**
```bash
ping -M do -s 1472 target-host   # Test 1500-byte packets (1472 + 28 bytes headers)
ip link show eth0                  # Check interface MTU
```

**Common in:** VPNs, VXLANs, Kubernetes overlays (which add headers, requiring lower MTU on pods)

---

### Q62. What is the difference between bandwidth and latency?

**Answer:**
- **Bandwidth:** How much data can flow per second (pipe width) — measured in Mbps/Gbps
- **Latency:** How long it takes for one packet to travel (pipe length) — measured in milliseconds

**Why both matter:**
- High bandwidth + high latency → Good for bulk transfers (backups), bad for interactive apps
- Low bandwidth + low latency → Bad for large downloads, fine for gaming/VoIP
- High bandwidth + low latency → Best of both (ideal)

**Rule of thumb:** Latency kills interactivity. Bandwidth limits throughput.

---

### Q63. What is DHCP and how does it work?

**Answer:**
DHCP (Dynamic Host Configuration Protocol) automatically assigns IP addresses and network config to devices.

**DORA process:**
```
Client → Broadcast: DHCP Discover   ("Anyone there? I need an IP")
Server → Unicast:   DHCP Offer      ("Here, take 192.168.1.100")
Client → Broadcast: DHCP Request    ("I'll take 192.168.1.100")
Server → Unicast:   DHCP Acknowledge("It's yours for 24 hours")
```

**In cloud:** AWS VPC uses DHCP Options Sets — this is how EC2 instances automatically get their private IPs, DNS servers, and domain name.

---

### Q64. What is the difference between TCP Keep-Alive and HTTP Keep-Alive?

**Answer:**

**TCP Keep-Alive:** OS-level probes sent to verify the connection is still alive after idle period. Prevents firewalls from dropping idle connections.

**HTTP Keep-Alive (Connection: keep-alive):** Reuses the same TCP connection for multiple HTTP requests, avoiding the overhead of a new TCP + TLS handshake per request.

**Why it matters:** HTTP/1.1 uses keep-alive by default. Without it, every request would need a new TCP handshake + TLS handshake → huge latency increase for pages with many resources.

---

### Q65. What is a socket and what is a socket exhaustion problem?

**Answer:**
A socket is a combination of IP + Port that identifies one end of a network connection. Each TCP connection uses one socket on the client (ephemeral port, 1024-65535) and one on the server (well-known port).

**Socket exhaustion:** If a service opens too many connections (or doesn't close them), it runs out of available ports.

```bash
# Check current socket count
ss -s
cat /proc/sys/net/ipv4/ip_local_port_range  # Available ephemeral ports (default: 32768-60999)

# Fix: Expand port range
sysctl -w net.ipv4.ip_local_port_range="1024 65535"

# Or: Enable socket reuse
sysctl -w net.ipv4.tcp_tw_reuse=1
```

---

### Q66. What is ICMP and why is `ping` sometimes blocked?

**Answer:**
ICMP (Internet Control Message Protocol) is used for diagnostic messages — ping (echo request/reply), traceroute, "destination unreachable", "TTL exceeded".

**Why ping is blocked:**
- Security policy: Some admins block ICMP to prevent network reconnaissance
- AWS Security Groups: Don't allow ICMP by default
- Firewalls: Block ICMP flood attacks

**Important:** If `ping` fails, it doesn't necessarily mean the host is down — it might just have ICMP blocked. Test with:
```bash
nc -zv host 443     # Test if TCP port is open instead
curl -v https://host
```

---

### Q67. What is the difference between `curl` and `wget`?

**Answer:**

| Feature | curl | wget |
|---------|------|------|
| Protocols | HTTP, HTTPS, FTP, SCP, SFTP, SMTP, and more | HTTP, HTTPS, FTP |
| Output | stdout by default | File by default |
| Recursive download | No | Yes |
| Response headers | `curl -I` or `-v` | `wget --server-response` |
| API testing | Excellent (full header control) | Limited |
| Resume download | Limited | Yes (`-c`) |

**In DevOps:** `curl` is preferred for API testing and health checks. `wget` for simple file downloads.

---

### Q68. How does a container get its network interface (Docker/Kubernetes)?

**Answer:**

**Docker:**
1. Docker daemon creates a virtual Ethernet pair (veth)
2. One end placed in container namespace (appears as `eth0` inside container)
3. Other end attached to Docker bridge network (`docker0`)
4. iptables rules handle NAT for outbound traffic

**Kubernetes (via CNI):**
1. Kubelet calls CNI plugin when pod is created
2. CNI creates veth pair, assigns IP from pod CIDR
3. Sets up routes on the node for pod-to-pod communication
4. CNI handles cross-node routing (varies by plugin)

---

### Q69. What is service mesh (Istio/Linkerd) and why is it needed?

**Answer:**
A service mesh adds a sidecar proxy (usually Envoy) to each pod that intercepts all network traffic. This provides:

- **Mutual TLS (mTLS):** Automatic encryption between all services
- **Traffic management:** Canary deployments, circuit breaking, retries
- **Observability:** Distributed tracing, metrics, service topology
- **Policy enforcement:** Rate limiting, access control

**Problem it solves:** In a microservices architecture with 50+ services, implementing retry logic, TLS, and observability in every service's code is impractical. The mesh handles it transparently at the network level.

---

### Q70. What is Egress traffic control and why does it matter in security?

**Answer:**
Egress is traffic leaving your network/cluster. Most security focuses on ingress (inbound), but egress control is equally important.

**Why control egress:**
- Prevent data exfiltration (attacker can't send your data out)
- Prevent compromised pods from reaching C2 (command & control) servers
- Compliance requirements (data sovereignty)

**How to implement:**
- **Kubernetes:** NetworkPolicy with `egress` rules, or service mesh policies
- **AWS:** Security Group outbound rules, VPC Endpoint (keep traffic off internet), AWS Network Firewall
- **Linux:** iptables OUTPUT chain rules

---

## 📌 Quick Reference Cheat Sheet

### Critical Commands for Interviews

```bash
# Network interfaces
ip addr show                    # All interfaces + IPs
ip link show                    # Interface state
ifconfig -a                     # Older alternative

# Routing
ip route show                   # Routing table
ip route get 8.8.8.8            # Route for specific destination
netstat -r                      # Older routing table view

# DNS
dig domain.com                  # Full DNS query
dig domain.com +short           # IP only
dig @8.8.8.8 domain.com         # Query specific server
nslookup domain.com             # Simple DNS test

# Ports & Connections
ss -tlnp                        # TCP listening ports
ss -s                           # Socket statistics
netstat -tlnp                   # Alternative to ss

# Connectivity tests
ping -c 4 host                  # ICMP test
nc -zv host port                # TCP port test
curl -v https://host            # Full HTTP test
traceroute host                 # Path trace
mtr host                        # Continuous path trace

# Packet capture
tcpdump -i eth0 port 80         # Capture HTTP
tcpdump -i eth0 -w file.pcap    # Save to file

# SSL
openssl s_client -connect host:443    # Test SSL
```

---

## 🎯 Interview Strategy

### How to Answer Scenario Questions

Use the **STAR + Layer** approach:
1. **State** which layer you're starting at (always start broad)
2. **Tools** you'd use at each layer
3. **Actions** you'd take based on findings
4. **Resolution** and how you'd prevent recurrence

### Priority Order for Study

If time is short, master in this order:
1. ✅ DNS (it's always DNS)
2. ✅ TCP 3-way handshake + TLS
3. ✅ CIDR & Subnetting
4. ✅ HTTP status codes + methods
5. ✅ AWS VPC, Subnets, Security Groups vs NACLs
6. ✅ "What happens when you type a URL"
7. ✅ Linux troubleshooting commands (`ss`, `dig`, `curl`, `tcpdump`)
8. ✅ Kubernetes Services (ClusterIP/NodePort/LB) + Ingress
9. ✅ Load Balancer L4 vs L7
10. ✅ NAT Gateway vs Internet Gateway

---

*Built for DevOps | SRE | Cloud | Platform Engineering interview preparation*
*Covers: OSI, TCP/IP, DNS, HTTP/HTTPS, TLS, Routing, AWS VPC, Kubernetes Networking, Linux Commands, Firewalls, CDN, Troubleshooting*
