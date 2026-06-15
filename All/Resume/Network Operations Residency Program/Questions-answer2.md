# Google NORP Interview Preparation
**Candidate:** Abhijit Ray
**Role:** Network Operations Residency Program (NORP) — University Graduate, 2026 Start, Bengaluru

This guide maps the job description to likely interview topics, lists sample questions per category, and gives STAR-method answer frameworks built from your three projects (Aegis Stack, PodPlate, RoutineOps).

---

## 1. Likely Interview Structure

For a Google residency/new-grad technical role, expect 2–4 rounds:

1. **Recruiter screen** — background, motivation, logistics, basic networking concepts.
2. **Technical phone screen** — networking fundamentals + a coding/scripting exercise (Python or Bash).
3. **Technical/Domain round** — deeper networking troubleshooting, system design lite, hands-on scenarios.
4. **Googleyness & Leadership (G&L) / Behavioral round** — STAR-based, focused on collaboration, ambiguity, ownership, learning agility.

NORP specifically emphasizes: network fundamentals, project management, adaptability to changing priorities, collaboration, and knowledge sharing — so expect a mix of technical depth *and* "how do you work with others" questions.

---

## 2. Networking Fundamentals (Technical Questions)

These test the "minimum qualifications" — TCP/IP, ARP, IP Tables, routing, Unix admin.

| # | Question | What to cover in your answer |
|---|----------|-------------------------------|
| 1 | Walk me through what happens when you type a URL into a browser. | DNS resolution, TCP handshake, TLS, HTTP request/response, routing path. |
| 2 | Explain the TCP three-way handshake and why it's needed. | SYN, SYN-ACK, ACK; reliability, sequence number sync. |
| 3 | What is ARP and why is it needed in a LAN? | Maps IP to MAC; ARP cache, ARP spoofing risk. |
| 4 | Difference between TCP and UDP — when would you use each? | Reliability vs. speed; examples (DNS uses UDP, HTTP uses TCP). |
| 5 | How does NAT work, and why is it used? | Address translation, port mapping, conserving public IPs — tie to your VPC NAT configuration in RoutineOps/Aegis Stack. |
| 6 | What's the difference between a router and a switch? | Layer 3 vs Layer 2, broadcast domains, routing tables. |
| 7 | How would you troubleshoot "the website is down" for a user? | Systematic approach: ping, traceroute, dig/nslookup, check DNS, check firewall/security groups, check app layer. |
| 8 | Explain subnetting — how would you split a /24 into 4 subnets? | CIDR math, subnet mask, usable hosts. |
| 9 | What are IP Tables used for? | Packet filtering, NAT rules, chains (INPUT/OUTPUT/FORWARD) — reference your Terraform-managed security groups/ACLs. |
| 10 | How does DNS resolution work end-to-end? | Recursive resolver → root → TLD → authoritative server; caching/TTL. |
| 11 | What is BGP and why does Google care about it? | High-level: inter-AS routing protocol, path selection — acceptable to give a conceptual answer if not deeply experienced. |
| 12 | How would you detect and resolve packet loss between two data centers? | traceroute/mtr, check interface errors, check for congestion, escalation path. |

**Prep tip:** For each answer, briefly connect it to something concrete you did (e.g., "When I configured NAT for the RoutineOps VPC, I had to map private subnets to a NAT gateway so EC2 instances without public IPs could reach the internet for package installs").

---

## 3. Scripting / Coding Questions

Since the JD mentions Python, Bash, and shell scripting:

- Write a script to parse a log file and count failed connection attempts by IP.
- Given a list of IP addresses, write a function to validate which are valid IPv4 addresses.
- Write a Bash one-liner to find the top 5 IPs hitting a server from an access log.
- How would you automate a repetitive network health-check task? (Tie to your Prometheus/Grafana + Python health-monitoring work in RoutineOps.)

---

## 4. Behavioral / STAR Questions — "Googleyness & Leadership"

For each, use **Situation → Task → Action → Result**. Below are tailored drafts using your real projects. Practice saying these out loud in under 90 seconds each.

### Q1: "Tell me about a time you had to learn a new technology quickly to complete a project."

> **Situation:** While building Aegis Stack, I needed to implement policy-based security controls across a Kubernetes cluster but had limited prior exposure to admission controllers.
> **Task:** I had to evaluate and integrate a policy engine that could enforce network and security rules cluster-wide without breaking existing workloads.
> **Action:** I researched OPA Gatekeeper and Kyverno, tested both in a sandbox VPC, and chose the one that integrated better with our existing Terraform pipeline. I wrote policies incrementally, validating each with Falco runtime monitoring before enforcing in "deny" mode.
> **Result:** I rolled out enforced policies with zero workload disruption, and the environment setup time dropped from days to under 20 minutes due to the automated, repeatable IaC pipeline.

---

### Q2: "Describe a time you diagnosed and resolved a connectivity or performance issue."

> **Situation:** In the PodPlate platform, after introducing Istio service mesh with mTLS, some services intermittently lost connectivity.
> **Task:** I needed to identify the root cause without causing downtime, since other services depended on the mesh.
> **Action:** I used `traceroute`, `netstat`, and `nslookup` to isolate whether the issue was DNS, routing, or mTLS certificate handshake. I cross-checked with Prometheus/Grafana metrics and Loki logs to correlate timing of failures with certificate rotation events.
> **Result:** I identified a misconfigured network policy blocking sidecar-to-sidecar traffic during cert rotation, fixed it, and achieved zero-downtime deployments going forward with a 35% reduction in inter-service latency.

---

### Q3: "Tell me about a time you had to coordinate planned maintenance or a change with minimal disruption."

> **Situation:** In RoutineOps, I needed to update VPC route tables and security group rules to support a new subnet layout.
> **Task:** This change risked breaking connectivity for live services if sequenced incorrectly.
> **Action:** I planned the change in stages, documented rollback steps, applied changes via Terraform in a staging VPC first, then scheduled the production change during a low-traffic window. I monitored Prometheus/Grafana dashboards in real time during the rollout.
> **Result:** The maintenance was completed with zero service disruption and full audit traceability — directly mirroring the "coordinating planned maintenance" responsibility in this role.

---

### Q4: "Tell me about a time priorities changed suddenly and you had to adapt."

> **Situation:** While working on CI/CD automation for RoutineOps, a recurring deployment failure started causing unplanned outages, which became more urgent than the feature I was building.
> **Task:** I had to pause planned work and prioritize root-causing the outages.
> **Action:** I shifted focus to add structured logging and Python-based health checks into the pipeline, used Prometheus alerts to catch failures earlier, and worked through the backlog of incidents methodically.
> **Result:** This reduced unplanned outages by 60% and improved deployment speed by 3x — and taught me to re-prioritize quickly when operational stability is at risk, which I understand is core to network operations.

---

### Q5: "Tell me about a time you worked with others / shared knowledge with a team."

> **Situation:** Across my three projects, I documented infrastructure decisions and runbooks (e.g., Terraform module READMEs, troubleshooting guides for `dig`/`traceroute`/`netstat` workflows).
> **Task:** Even working largely independently, I treated each project as if a team would need to onboard onto it.
> **Action:** I wrote clear documentation, used consistent IaC patterns (Terraform modules), and structured dashboards (Grafana) so anyone could understand system health at a glance.
> **Result:** This habit of "build for the next person" is something I want to bring into NORP's rotational structure, where knowledge transfer between rotations is critical.
>
> *(If you have any group project, internship, or hackathon collaboration experience, swap this in — Google strongly prefers a real team-based story here.)*

---

### Q6: "Why Google, and why NORP specifically?"

Talking points to weave together (not strict STAR, but be specific and personal):
- Your coursework + hands-on AWS/Terraform/Kubernetes projects show direct alignment with NIE (Network Implementation Engineering).
- NORP's rotational structure matches your interest in growing from "build it" (your solo projects) to "operate it at scale" (Google's network).
- Mention scale: you've operated at the scale of a personal VPC/cluster; you're excited to apply the same troubleshooting instincts (`dig`, `traceroute`, `netstat`, `nslookup`) at global scale.
- Avoid generic "Google is a great company" — anchor it to NIE/network operations specifically.

---

## 5. Situational / Scenario Questions

These test judgment under operational conditions (common for ops/NIE roles):

- "A critical link between two data centers just went down at 2 AM. Walk me through your first 15 minutes."
- "You notice a configuration change is about to be pushed that could cause an outage, but the engineer pushing it is confident it's fine. What do you do?"
- "You're given a vague task with incomplete information and a tight deadline — how do you proceed?"
- "Two team members disagree on the root cause of a network issue — how do you help resolve it?"

**Framework for these:** Acknowledge urgency → gather data before acting → communicate status to stakeholders → fix/mitigate → document/postmortem. Reference your monitoring stack (Prometheus/Grafana/Loki/Alertmanager) as how you'd gather data quickly.

---

## 6. Questions You Could Ask the Interviewer

Shows curiosity and "Googleyness":

- "What does a typical rotation look like in NORP — how are rotation teams and projects assigned?"
- "What tools/automation does the Global Infrastructure org rely on most for day-to-day operations?"
- "How does NORP support residents transitioning into a permanent NIE role — is there a mentorship structure?"
- "What's a recent project where 'better tooling or automation' (as mentioned in the JD) made a measurable difference?"

---

## 7. Quick Prep Checklist

- [ ] Practice explaining each project (Aegis Stack, PodPlate, RoutineOps) in under 2 minutes, focused on networking aspects.
- [ ] Review subnetting/CIDR math — be ready to do it on a whiteboard/shared doc.
- [ ] Refresh OSI model layers and map each tool (`dig`, `traceroute`, `netstat`, `nslookup`) to the layer it diagnoses.
- [ ] Prepare 4–5 STAR stories covering: ownership, ambiguity, conflict/collaboration, learning agility, failure & recovery.
- [ ] Have 2–3 thoughtful questions ready for each interviewer.
- [ ] Be ready to discuss trade-offs (e.g., why Terraform over manual config, why Istio mTLS, why Gatekeeper vs Kyverno).

---

*Good luck with your interview — your project portfolio (Aegis Stack, PodPlate, RoutineOps) maps unusually well onto NIE responsibilities (provisioning, monitoring, troubleshooting, maintenance coordination). Lean into that alignment in every answer.*
