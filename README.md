<div align="center">

# 🕵️ SHADOWPIPE

### *Operation Shadow Pipeline — An AI-Powered Red Team vs. Blue Team Cybersecurity Simulation*

<br/>

[![Status](https://img.shields.io/badge/status-lab%20complete-brightgreen?style=for-the-badge)](#)
[![Environment](https://img.shields.io/badge/environment-isolated%20VM%20lab-blue?style=for-the-badge&logo=vmware&logoColor=white)](#)
[![AI SOC](https://img.shields.io/badge/AI%20SOC-LLaMA%203.3%2070B-6f42c1?style=for-the-badge&logo=meta&logoColor=white)](#)
[![SIEM](https://img.shields.io/badge/SIEM-Wazuh-1A9CD8?style=for-the-badge)](#)
[![IDS](https://img.shields.io/badge/IDS-Suricata-EF3B2D?style=for-the-badge)](#)
[![License](https://img.shields.io/badge/license-Educational%20Use%20Only-yellow?style=for-the-badge)](#-license--ethical-use)

[![Last Commit](https://img.shields.io/github/last-commit/yourusername/shadowpipe?style=for-the-badge)](#)
[![Stars](https://img.shields.io/github/stars/yourusername/shadowpipe?style=for-the-badge&color=gold)](#)

</div>

---

> ⚠️ **This project runs entirely inside an isolated, host-only virtual lab** (VMware `VMnet2`, `192.168.20.0/24`, no route to the internet or host network). It exists to **teach and demonstrate** supply-chain attack detection and AI-driven autonomous defense. It is **not** a tool for attacking systems you do not own or have explicit authorization to test. See [License & Ethical Use](#-19-license--ethical-use).

---

## 📚 Table of Contents

1. [Executive Summary](#-1-executive-summary)
2. [Why This Project Exists](#-2-why-this-project-exists)
3. [Lab Architecture](#-3-lab-architecture)
4. [Network Topology](#-4-network-topology)
5. [The Target: FakeCorp](#-5-the-target-fakecorp)
6. [Red Team: Attack Simulation](#-6-red-team-attack-simulation)
7. [Blue Team: Detection & Autonomous Response](#-7-blue-team-detection--autonomous-response)
8. [Canary Deception Defense](#-8-canary-deception-defense)
9. [Real-Time Dashboard](#-9-real-time-dashboard)
10. [Framework Mappings](#-10-framework-mappings)
11. [Tools & Technologies](#-11-tools--technologies)
12. [Key Metrics](#-12-key-metrics)
13. [Repository Structure](#-13-repository-structure)
14. [Running the Demo](#-14-running-the-demo)
15. [Edge Cases & Known Limitations](#-15-edge-cases--known-limitations)
16. [What I'd Improve for Production](#-16-what-id-improve-for-production)
17. [Learning Outcomes](#-17-learning-outcomes)
18. [Author](#-18-author)
19. [License & Ethical Use](#-19-license--ethical-use)

---

## 🧭 1. Executive Summary

**SHADOWPIPE (Operation Shadow Pipeline)** is a capstone cybersecurity simulation built during the **C-DAC PGCP-ITISS** (Post Graduate Certificate Programme in IT Infrastructure, Systems and Security) programme. It demonstrates, end-to-end, how **a single leaked credential** can cascade into a full **software supply-chain compromise** — and how an **AI-powered autonomous defense system** can detect, contain, and remediate that compromise in roughly a minute, instead of the industry-average **197 days** it takes organizations to detect a breach (IBM/Ponemon Cost of a Data Breach Report).

The simulation is fully self-contained across **5 virtual machines** on an isolated host-only network, pitting a scripted **Red Team attack chain** against a **Blue Team stack** of network IDS, SIEM correlation, and an LLM-driven autonomous SOC agent.

---

## 🎯 2. Why This Project Exists

### Problem Statement
Most students learn security concepts in isolation — SQL injection in one module, SIEM in another, incident response in a third — without ever seeing how they connect into a real attack **narrative**. Organizations, meanwhile, routinely take months to detect breaches that begin with something as simple as a credential accidentally committed to source control.

### Objectives
- Build a **realistic, contained attack chain** from initial reconnaissance to full supply-chain compromise
- Build a **corresponding defense stack** that detects, correlates, and autonomously responds to that exact chain
- Demonstrate **AI/LLM-driven decision-making** in a live security operations context
- Map every action to **industry frameworks** (MITRE ATT&CK, NIST CSF, OWASP Top 10) for credibility and structure
- Do all of this **safely**, inside a network with no route to the internet or host machine

### Learning Goals
- Offensive technique chaining (recon → credential theft → persistence → exfiltration → lateral compromise)
- Defensive tooling integration (Suricata → Wazuh → automated Active Response)
- Practical LLM integration into a security workflow (triage, decision-making, payload/report generation)
- Deception-based defense (canary/honeytoken design)
- Communicating security work through frameworks recruiters and interviewers recognize (MITRE, NIST, OWASP)

### Audience
Security students, blue team/SOC analysts, red teamers, DevSecOps engineers, and anyone reviewing this as a **portfolio/interview artifact** — this project is designed to be explained clearly, stage by stage, in a technical interview.

---

## 🏗️ 3. Lab Architecture

The entire environment runs on **VMware Workstation Pro 17**, using **five virtual machines** connected via a **host-only network adapter (VMnet2)** on the `192.168.20.0/24` subnet.

| VM | IP Address | OS | Role |
|---|---|---|---|
| **WebServer** | `192.168.20.10` | Ubuntu Server 24.04 LTS | Hosts the fictional company's site + billing application |
| **GitLab** | `192.168.20.20` | Ubuntu Server 24.04 LTS | Self-hosted GitLab CE — source control + CI/CD |
| **Wazuh** | `192.168.20.30` | Ubuntu Server 24.04 LTS | Blue team stack: Suricata IDS, Wazuh SIEM, AI agent, dashboard |
| **Client** | `192.168.20.40` | Ubuntu Desktop | Simulated victim workstation |
| **Kali** | `192.168.20.50` | Kali Linux 2024 | Red team attacker machine |

> 💡 **Why host-only networking?** A host-only virtual switch has **no route to the physical network or the internet**. This guarantees that reverse shells, DNS exfiltration, and credential-theft traffic used in the simulation stay completely contained within the lab — a non-negotiable safety requirement for this kind of exercise.

---

## 🌐 4. Network Topology

```
  [KALI]          192.168.20.50     Attacker Machine (Red Team)
     │
  [WEBSERVER]     192.168.20.10     Company Website + Billing App
     │
  [GITLAB]        192.168.20.20     Source Control + CI/CD
     │
  [WAZUH]         192.168.20.30     IDS + SIEM + AI Defense Agent
     │
  [CLIENT]        192.168.20.40     Victim Workstation

  ══════════════════════════════════════════════════
   VMware VMnet2  |  Host-Only  |  192.168.20.0/24
  ══════════════════════════════════════════════════
```

| Port | Protocol | Service | Machine |
|---|---|---|---|
| 80 | HTTP | Company site + billing API | WebServer |
| 80 | HTTP | GitLab CE web + API | GitLab |
| 55000 | HTTPS | Wazuh REST API | Wazuh |
| 9999 | HTTP | Dashboard status listener | Wazuh |
| 4444 | TCP | Reverse shell listener (lab-internal only) | Kali |
| 5555 | HTTP | Exfiltration listener (lab-internal only) | Kali |

---

## 🏢 5. The Target: FakeCorp

**FakeCorp** is a fictional financial-software company simulating a realistic small-to-medium business: a public website, an internal billing application ("BillingPro Enterprise"), and a self-hosted GitLab instance for code and CI/CD.

The application intentionally contains a set of **realistic, common vulnerability classes** so the attack chain reflects real-world incident patterns rather than contrived exploits:

| Vulnerability Class | Where It Appears | OWASP Category |
|---|---|---|
| Security misconfiguration (`.git` exposed on the live web root) | Web server static file config | A05: Security Misconfiguration |
| SQL injection | Login & customer-search endpoints | A03: Injection |
| Command injection | Diagnostics & report-generation endpoints | A03: Injection |
| Hardcoded/committed credentials | Git commit history | A07: Identification & Authentication Failures |
| Unsigned software distribution | Client download channel | A08: Software & Data Integrity Failures |
| No logging/monitoring on the vulnerable app | Application layer | A09: Logging & Monitoring Failures |

> 📝 The **root cause** of the entire attack chain is a single, very common developer mistake: a `.git` directory left reachable from a public web server, with a credential sitting in its commit history.

---

## 🔴 6. Red Team: Attack Simulation

An automated Python script drives a **10-stage attack chain**, each stage building on the credentials or access gained in the previous one.

| # | Stage | MITRE ATT&CK Technique | What Happens |
|---|---|---|---|
| 1 | `.git` Discovery | T1592.002 — Gather Victim Host Info | Probes the web server for an exposed `.git` directory |
| 2 | Repository Reconstruction | T1083 — File & Directory Discovery | Uses `git-dumper` to rebuild the full repo from exposed git objects |
| 3 | Credential Extraction | T1552.001 — Credentials in Files | Scans full commit history for credential-shaped strings |
| 4 | GitLab Authentication | T1078 — Valid Accounts | Uses the extracted token to authenticate to the GitLab API |
| 5 | CI/CD Pipeline Poisoning | T1195.002 — Supply Chain Compromise | Modifies the pipeline config via the GitLab API to add a persistence beacon |
| 6 | DNS Data Exfiltration | T1071.004 — App Layer Protocol: DNS | Demonstrates DNS-tunneling-based exfiltration of stolen data |
| 7 | Deception Probe | — | Checks for obvious honeypot/decoy files before proceeding |
| 8 | Web Server Compromise | T1059.004 — Unix Shell Execution | Exploits a command-injection endpoint to obtain shell access |
| 9 | Supply Chain Backdoor | T1195.002 — Supply Chain Compromise | Distributes a backdoored client application to end users |
| 10 | Data Exfiltration | T1041 / T1020 — Exfiltration Over C2 / Automated Exfiltration | Extracts application data via the discovered injection point |

<details>
<summary><strong>💡 Notable engineering detail: AI-morphed persistence beacon</strong></summary>

The pipeline-poisoning stage uses an LLM (LLaMA 3.3 70B via Groq) to rewrite its own persistence beacon into a syntactically different but functionally identical form on each run — demonstrating why **signature-based detection alone** struggles against AI-assisted, polymorphic payloads, and why **behavioral correlation** (used on the Blue Team side) matters more than pattern matching.
</details>

---

## 🔵 7. Blue Team: Detection & Autonomous Response

### Suricata IDS
Monitors the lab network in real time via **4 custom detection rules**, watching for `.git` directory scanning, DNS-tunneling patterns, and the use of both real and canary GitLab tokens in HTTP traffic. Alerts are written to structured JSON (`eve.json`) for downstream correlation.

### Wazuh SIEM
Ingests Suricata's alert stream and applies **custom correlation rules** that escalate isolated network events into a scored incident. A maximum-severity match (e.g., confirmed DNS exfiltration) triggers **Active Response**, which automatically launches the AI defense agent.

### AI Agent — Autonomous 17-Stage Response
A Python agent, powered by **LLaMA 3.3 70B via the Groq API**, executes a structured incident-response playbook:

| Phase | Stages | What It Does |
|---|---|---|
| **Triage** | 1–3 | Pulls recent SIEM alerts, sends them to the LLM for classification (attack type, confidence, attacker IP), cross-checks IP reputation |
| **Containment** | 4–7 | Blocks the attacker's IP at the firewall, revokes the compromised credential, neutralizes the poisoned pipeline, removes the exposed `.git` directory |
| **Preservation** | 8–10 | Captures forensic evidence and verifies remediation took effect |
| **Remediation** | 11–17 | Patches the vulnerable application, releases a fixed version, deploys a **canary deception token**, and generates an encrypted incident report |

> 🧠 **Fail-safe design:** if the LLM call fails or returns unparseable output, the agent **defaults to treating the situation as a confirmed attack** and proceeds with containment — a deliberate "fail-closed" bias, since an unnecessary containment action is far cheaper than a missed breach.

---

## 🕳️ 8. Canary Deception Defense

After remediating the initial compromise, the AI agent plants a **canary token** — a fake credential structurally identical to the original leak, seeded back into the same exposed location.

- If the attacker returns and reuses the same technique, they retrieve the **canary** instead of a real credential.
- The moment that canary is presented to the GitLab API, the **deception trap fires**: the system confirms the intrusion attempt, logs the attacker's identity and timeline, and confirms the incident response chain (firewall block, SIEM alert, forensic capture) was already in place *before* the attacker realized anything was wrong.

This demonstrates a core blue-team principle: **the original vulnerability can be turned into an attribution and early-warning mechanism**, rather than simply being closed.

---

## 📊 9. Real-Time Dashboard

A terminal-based live dashboard (Python **Rich** library) displays both sides of the simulation simultaneously:
- **Red Team progress** — current attack stage and status
- **Blue Team progress** — current defense stage and status
- A live **estimated breach-cost counter**, derived from industry breach-cost-per-second figures, that freezes once the incident is resolved — visually reinforcing the value of fast automated response.

---

## 🗺️ 10. Framework Mappings

<details>
<summary><strong>MITRE ATT&CK Coverage</strong></summary>

Spans Reconnaissance, Discovery, Credential Access, Initial Access, Execution, Persistence, Defense Evasion, Command & Control, and Exfiltration tactics — with each attack stage mapped to a specific technique ID (see Red Team section above).
</details>

<details>
<summary><strong>NIST Cybersecurity Framework Coverage</strong></summary>

Maps outcomes across all five NIST CSF functions — **Identify, Protect, Detect, Respond, Recover** — showing which controls were violated by the initial vulnerability and which were subsequently enforced by the Blue Team response.
</details>

<details>
<summary><strong>OWASP Top 10 (2021) Coverage</strong></summary>

Touches **A03 (Injection), A05 (Security Misconfiguration), A07 (Identification & Auth Failures), A08 (Software & Data Integrity Failures), A09 (Logging & Monitoring Failures)**, and **A10 (SSRF)** — see Section 5 above.
</details>

---

## 🧰 11. Tools & Technologies

| Category | Tool | Purpose |
|---|---|---|
| Virtualization | VMware Workstation Pro 17 | Hosts the 5-VM isolated lab |
| Networking | VMnet2 (Host-Only) | Private, internet-isolated subnet |
| Web Application | Node.js 20 + Express 4 + SQLite3 | Target billing application |
| Source Control / CI/CD | GitLab CE 17 + GitLab Runner | Self-hosted repo + pipeline |
| Network IDS | Suricata 7 | Custom rule-based network detection |
| SIEM / XDR | Wazuh 4.9 | Event correlation + automated active response |
| AI / LLM | Groq API — LLaMA 3.3 70B | SOC triage, decision-making, report generation |
| Attack Tooling | git-dumper | Git repository reconstruction from exposed objects |
| Encryption | Python `cryptography` (Fernet) | Authenticated encryption for incident reports |
| Threat Intel | AbuseIPDB API, VirusTotal API | IP reputation lookups |
| Dashboard | Python `rich` (Live) | Real-time terminal visualization |
| Core Scripting | Python, Bash | Automation across all 5 machines |

---

## 📈 12. Key Metrics

| Metric | Value |
|---|---|
| Attack stages | 10 |
| Defense stages | 17 |
| Virtual machines | 5 |
| Custom Suricata rules | 4 |
| Full defense response time | ~50–55 seconds |
| Industry average breach detection time | 197 days *(IBM/Ponemon)* |
| AI-driven speed improvement | ~99.9% faster than manual response |

---

## 📁 13. Repository Structure

```
.
├── attacker/            # Red team automation (Kali VM)
│   └── attack.py
├── webserver/           # FakeCorp site + BillingPro app (WebServer VM)
│   ├── public/
│   └── server.js
├── blue-team/           # Wazuh VM assets
│   ├── ai_agent.py
│   ├── dashboard.py
│   ├── reset_all.sh
│   ├── suricata-rules/
│   └── wazuh-rules/
├── client/              # Victim workstation payload/demo assets
├── docs/                # Architecture notes, diagrams, documentation
└── README.md
```

---

## ▶️ 14. Running the Demo

> ⚠️ This lab is intended to run **only inside the isolated VMware VMnet2 environment** described above. Do not adapt or repoint any script at infrastructure outside this lab.

1. **Wazuh VM** — run the environment reset script to restore the lab to a clean starting state and launch the live dashboard
2. **Kali VM** — run the automated attack script to begin the 10-stage red team sequence
3. **Client VM** — follow the in-lab prompt to simulate the compromised software download
4. Watch the **dashboard** as the attack completes and the AI agent automatically triggers, working through its 17-stage response in real time
5. Re-run the attack script to observe either **vulnerability-patched failure** or the **canary deception trap** firing, depending on which remediation path was taken

---

## 🧯 15. Edge Cases & Known Limitations

- The "patched" client application still contains a secondary, lower-severity deserialization issue (`pickle.load()`), intentionally left in place to demonstrate that **security remediation is iterative** — not every issue is caught in a single response cycle.
- Attacker attribution in the lab is IP-based; a real-world VPN/proxy would require additional correlation signals (payload patterns, timing analysis) beyond what this lab implements.
- If the SIEM or GitLab service is unreachable, the AI agent degrades gracefully (fallback decisions, logged failures) rather than halting entirely.
- The environment **must be reset between demo runs** — firewall rules and rotated credentials persist otherwise.

---

## 🔮 16. What I'd Improve for Production

| Improvement | Why |
|---|---|
| Human-in-the-loop approval before automated containment | Avoid fully autonomous actions on production infrastructure |
| Software Bill of Materials (SBOM) + code signing | Detect tampered software before distribution, not after |
| Secrets manager (e.g., HashiCorp Vault) | Remove any credential storage from git entirely |
| Endpoint Detection & Response (EDR) | Catch anomalous process spawning (e.g., unexpected shell from an app process) |
| Secondary/manual analyst review queue | Backstop for AI misclassification |
| Proper ticketing integration (Jira/ServiceNow) | Replace ad hoc report generation with real incident workflows |

---

## 🎓 17. Learning Outcomes

- End-to-end attack-chain construction and reasoning (recon → access → persistence → exfiltration)
- SIEM rule authoring and multi-source alert correlation
- Practical integration of an LLM into a live security response pipeline
- Deception-based defense design (canary/honeytoken systems)
- Mapping real technical work to **MITRE ATT&CK**, **NIST CSF**, and **OWASP Top 10** — the frameworks used in real SOC and compliance reporting
- Safe, isolated lab design for offensive security demonstrations

---

## 👤 18. Author


Kshitij Shrivastava — C-DAC PGCP-ITISS

- 🔗 GitHub: (https://github.com/kshitijshri99)
- 💼 LinkedIn: (https://www.linkedin.com/in/kshitij-shrivastava-17551b172)

---

## 📜 19. License & Ethical Use

This project is shared for **educational and portfolio purposes only**. It targets a **fictional company** inside a **fully isolated, host-only virtual network with no route to the internet or any external system**.

**Do not** repurpose any script, technique, or configuration in this repository against systems you do not own or do not have explicit written authorization to test. Unauthorized access to computer systems is illegal in most jurisdictions. The author assumes no liability for misuse of the concepts demonstrated here.
