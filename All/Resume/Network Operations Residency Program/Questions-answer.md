# Google NORP — Direct Interview Q&A (Topic-Wise)
**Candidate:** Abhijit Ray | **Role:** Network Operations Residency Program (NORP), Bengaluru

Format: **Q → A**, grouped by topic. Answers are written so you can speak them directly in an interview.

---

## Topic 1: TCP/IP & Core Networking

**Q1. What happens when you type a URL in a browser and press enter?**
A. The browser checks DNS cache, then queries a DNS resolver to get the server's IP. It opens a TCP connection via a 3-way handshake (SYN, SYN-ACK, ACK), performs a TLS handshake if HTTPS, sends an HTTP request, and the server responds with the page data over the established connection.

**Q2. Explain the TCP three-way handshake.**
A. Client sends SYN (synchronize) with an initial sequence number. Server replies with SYN-ACK, acknowledging the client's sequence and sending its own. Client replies with ACK, confirming. After this, both sides can reliably exchange data with sequence tracking.

**Q3. TCP vs UDP — when do you use each?**
A. TCP is connection-oriented, reliable, and ordered — used for HTTP, file transfer, SSH. UDP is connectionless and faster but unreliable — used for DNS queries, video streaming, VoIP, where speed matters more than guaranteed delivery.

**Q4. What is ARP and why is it needed?**
A. ARP (Address Resolution Protocol) maps an IP address to a MAC address within a local network so devices can communicate at Layer 2. A device broadcasts "who has this IP?" and the owner replies with its MAC address, which gets cached in the ARP table.

**Q5. What is NAT and why is it used?**
A. NAT (Network Address Translation) translates private IP addresses to a public IP (and back) so multiple devices on a private network can share one public IP to reach the internet. I configured NAT gateways in AWS VPCs in my RoutineOps and Aegis Stack projects so private-subnet EC2 instances could reach the internet without exposing them publicly.

**Q6. Difference between a router and a switch?**
A. A switch operates at Layer 2, forwarding traffic based on MAC addresses within a single network/broadcast domain. A router operates at Layer 3, forwarding traffic between different networks based on IP addresses and routing tables.

**Q7. Explain subnetting. How do you split a /24 into 4 subnets?**
A. A /24 has 256 addresses (0–255). To split into 4 subnets, you borrow 2 bits, making it /26 — giving 4 subnets of 64 addresses each: 0–63, 64–127, 128–191, 192–255. Each subnet has a network address, broadcast address, and 62 usable hosts.

**Q8. What is DNS and how does resolution work?**
A. DNS translates domain names to IP addresses. A query goes from the client to a recursive resolver, which queries a root server, then a TLD server (e.g., .com), then the authoritative nameserver for the domain, which returns the IP. Results are cached based on TTL.

**Q9. What are IP Tables used for?**
A. IP Tables is a Linux firewall utility that filters packets using chains (INPUT, OUTPUT, FORWARD) and tables (filter, nat, mangle). I used IP Table-style rules and AWS security groups/NACLs to control traffic flow and apply NAT rules in my Terraform-provisioned VPCs.

**Q10. What is the OSI model, and how do troubleshooting tools map to it?**
A. The OSI model has 7 layers: Physical, Data Link, Network, Transport, Session, Presentation, Application. `ping`/`traceroute` work mainly at Network layer (L3), `netstat` shows Transport layer (L4) connections, and `nslookup`/`dig` work at the Application layer (L7) for DNS.

---

## Topic 2: Troubleshooting & Operations

**Q11. A user says "the website is down." How do you troubleshoot?**
A. I'd start broad and narrow down: ping the host to check basic reachability, run `traceroute` to find where the path breaks, use `dig`/`nslookup` to confirm DNS resolves correctly, check `netstat` for open ports/connections on the server, then check firewall rules and security groups, and finally check application-level logs.

**Q12. How would you detect packet loss between two data centers?**
A. I'd run `traceroute` or `mtr` to identify which hop introduces loss, check interface error counters and bandwidth utilization on routers along the path, and correlate with monitoring dashboards (e.g., Prometheus/Grafana) for timing patterns before escalating to the network team responsible for that segment.

**Q13. How do you coordinate planned maintenance with zero downtime?**
A. Plan and document the change, test it in a staging environment first, schedule it during low-traffic windows, communicate to stakeholders in advance, apply the change incrementally with monitoring active, and have a rollback plan ready. In RoutineOps, I applied VPC route table changes this way using Terraform, validating in staging before production.

**Q14. Critical link between two data centers goes down at 2 AM. What are your first steps?**
A. First, confirm the alert is real (check monitoring dashboards, not just one alert). Then check if it's a single link or wider outage via traceroute/route tables. Notify the on-call team/stakeholders with current status. Check for automatic failover/rerouting. Begin isolating the cause (hardware, config change, carrier issue) while keeping communication updates flowing every 15-30 minutes until resolved.

---

## Topic 3: Linux / Unix Administration

**Q15. What's your experience with Linux system administration?**
A. I've worked extensively with Bash scripting, process monitoring, log analysis, and automation. For example, in RoutineOps I wrote Python health-monitoring scripts and Bash automation integrated into CI/CD pipelines (GitHub Actions + Jenkins), and used standard Unix networking tools (`dig`, `traceroute`, `netstat`, `nslookup`) for diagnostics.

**Q16. How would you find the top IPs hitting a server from an access log?**
A. Using a command like: `awk '{print $1}' access.log | sort | uniq -c | sort -rn | head -5` — this extracts the IP field, counts occurrences, sorts by frequency, and shows the top 5.

---

## Topic 4: Cloud (AWS) & Infrastructure as Code

**Q17. Walk me through how you provisioned a VPC using Terraform.**
A. In Aegis Stack and RoutineOps, I defined subnets (public/private), route tables, internet/NAT gateways, security groups, and network ACLs as Terraform modules. This made environment setup repeatable and reduced setup time from days to under 20 minutes, since the entire network topology was version-controlled and reproducible.

**Q18. Why use Infrastructure as Code instead of manual configuration?**
A. IaC makes infrastructure repeatable, version-controlled, auditable, and reviewable (via pull requests). It reduces human error from manual console changes, enables consistent environments across dev/staging/prod, and makes rollback straightforward by reverting code.

**Q19. How did you monitor network performance in your projects?**
A. I deployed Prometheus for metrics collection, Grafana for dashboards, Loki for log aggregation, and Alertmanager for alerting. In PodPlate, I tracked 15+ custom SLIs, which reduced incident MTTR by 50% through faster, data-driven root cause analysis.

---

## Topic 5: Behavioral / STAR (Googleyness & Leadership)

**Q20. Tell me about a time you learned a new technology quickly.**
A. **Situation:** While building Aegis Stack, I needed a policy engine for Kubernetes security but had limited prior exposure. **Task:** Integrate it without breaking existing workloads. **Action:** I researched OPA Gatekeeper and Kyverno, tested both in a sandbox, and rolled out policies incrementally in "audit" mode before enforcing, validated with Falco. **Result:** Zero workload disruption, and environment setup time dropped from days to under 20 minutes.

**Q21. Tell me about a time you diagnosed a tricky connectivity issue.**
A. **Situation:** After adding Istio mTLS in PodPlate, some services intermittently lost connectivity. **Task:** Find the root cause without downtime. **Action:** I used `traceroute`, `netstat`, and `nslookup`, cross-referenced with Grafana/Loki logs around certificate rotation times. **Result:** Found a network policy blocking sidecar traffic during cert rotation, fixed it, achieved zero-downtime deployments and a 35% latency reduction.

**Q22. Tell me about a time priorities suddenly changed and you adapted.**
A. **Situation:** While building CI/CD automation in RoutineOps, recurring deployment failures started causing unplanned outages. **Task:** Pause planned work to fix the urgent issue. **Action:** I added structured logging, Python health checks, and Prometheus alerts to catch failures earlier. **Result:** Reduced unplanned outages by 60% and improved deployment speed 3x.

**Q23. Tell me about a time you coordinated a change carefully to avoid disruption.**
A. **Situation:** I needed to update VPC route tables and security groups in RoutineOps for a new subnet layout. **Task:** Avoid breaking live connectivity. **Action:** Tested in staging via Terraform first, scheduled the production change for low traffic, monitored Grafana dashboards live during rollout. **Result:** Zero service disruption with full audit traceability.

**Q24. Tell me about a time you documented or shared knowledge for others.**
A. **Situation:** Across all three projects, I treated infrastructure as something others would need to maintain. **Task:** Make systems understandable without me explaining everything verbally. **Action:** I wrote clear Terraform module documentation, structured Grafana dashboards for at-a-glance health checks, and kept consistent naming/patterns across modules. **Result:** Anyone could onboard onto the infrastructure quickly — a habit I want to bring into NORP's rotation handoffs.

**Q25. Why Google, and why NORP specifically?**
A. My coursework and three hands-on projects (Aegis Stack, PodPlate, RoutineOps) all center on provisioning, securing, and operating network infrastructure — exactly what NIE does. NORP's rotational structure lets me go from "building it myself" to "operating Google's network at global scale," while developing project management skills alongside technical depth. I'm excited to apply the same troubleshooting instincts I built with `dig`, `traceroute`, and `netstat` to a much larger, more complex environment, and to learn from experienced engineers across two rotations.

---

## Topic 6: Questions to Ask the Interviewer

**Q26. What should you ask at the end of the interview?**
A. Good options:
- "What does a typical rotation look like — how are residents matched to teams and projects?"
- "What tools or automation does the Global Infrastructure team rely on most day-to-day?"
- "Is there a mentorship structure to help residents transition into a permanent NIE role after the 16 months?"
- "Can you share an example of a recent automation or tooling improvement that made a big operational difference?"

---

## Quick Recap Table

| Topic | # of Q&A | Focus |
|---|---|---|
| TCP/IP & Core Networking | 10 | Fundamentals (JD minimum qualification) |
| Troubleshooting & Operations | 4 | Real-world ops scenarios |
| Linux/Unix | 2 | Scripting & admin |
| AWS & IaC | 3 | Cloud infrastructure |
| Behavioral/STAR | 6 | Googleyness & Leadership |
| Questions to Ask | 1 | Curiosity/engagement |
