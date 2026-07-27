# 🛡️ SHADOWPIPE
### *AI-Powered Supply Chain Attack Simulation & Autonomous Defense*

<p align="center">

![Cybersecurity](https://img.shields.io/badge/Domain-Cybersecurity-red?style=for-the-badge)
![Python](https://img.shields.io/badge/Python-3.x-blue?style=for-the-badge)
![Node.js](https://img.shields.io/badge/Node.js-Express-green?style=for-the-badge)
![Wazuh](https://img.shields.io/badge/Wazuh-SIEM-purple?style=for-the-badge)

</p>

---

## 📖 Overview

**SHADOWPIPE** is an enterprise-scale Red Team vs Blue Team cybersecurity simulation developed as a **CDAC PGCP-ITISS Capstone Project**.

The project demonstrates how **one exposed Git repository** can lead to:

- 🔓 Credential Theft
- ☠️ Supply Chain Compromise
- 💻 Remote Code Execution
- 📦 CI/CD Pipeline Poisoning
- 🗄️ Database Exfiltration

An AI-powered SOC then detects, investigates, contains, patches, and recovers the environment automatically.

---

# 🎯 Key Features

| Red Team | Blue Team |
|----------|-----------|
| Exposed `.git` discovery | Wazuh SIEM |
| Git history secret extraction | Suricata IDS |
| GitLab API abuse | AI Threat Analysis |
| SQL Injection | Firewall Automation |
| Command Injection | Token Revocation |
| DNS Exfiltration | Vulnerability Patching |
| Reverse Shell | Digital Forensics |
| Supply Chain Attack | Canary Deployment |

---

# 🏗️ Architecture

```text
                   VMware Host-Only Network
                     192.168.20.0/24

         Kali Linux (Attacker)
                  │
      ┌───────────┼─────────────┐
      │           │             │
 Web Server    GitLab CE     Client VM
      │
      │
Wazuh + Suricata + AI SOC
```

---

# ⚔️ Attack Lifecycle

```text
.git Discovery
      │
Repository Dump
      │
Credential Extraction
      │
GitLab Authentication
      │
Pipeline Poisoning
      │
DNS Exfiltration
      │
Reverse Shell
      │
Supply Chain Attack
      │
Database Exfiltration
```

---

# 🤖 AI Incident Response

The AI agent automatically performs:

- Collect Wazuh alerts
- Analyze with LLaMA 3.3 70B
- Threat intelligence lookup
- Block attacker IP
- Revoke GitLab tokens
- Restore CI/CD pipeline
- Patch vulnerabilities
- Collect forensic evidence
- Release secure application
- Deploy Git canary tokens
- Generate encrypted incident report

---

# 🛠️ Technology Stack

### Infrastructure
- VMware Workstation
- Ubuntu Server
- Kali Linux

### Backend
- Python
- Node.js
- Express.js
- SQLite

### Security
- Wazuh SIEM
- Suricata IDS
- GitLab CE

### AI
- Groq API
- LLaMA 3.3 70B

---

# 📊 Framework Mapping

✅ MITRE ATT&CK

✅ OWASP Top 10

✅ NIST Cybersecurity Framework

---

# 📂 Suggested Repository Structure

```text
SHADOWPIPE/
│
├── attack.py
├── ai_agent.py
├── dashboard.py
├── listener.py
├── webserver/
├── gitlab/
├── wazuh/
├── docs/
├── screenshots/
├── presentation/
├── README.md
└── LICENSE
```

---

# 🚀 Demo Flow

1. Launch lab environment
2. Execute automated attack
3. Observe compromise
4. Trigger AI SOC
5. Watch autonomous remediation
6. Review forensic evidence
7. Analyze generated incident report

---

# 📚 Learning Outcomes

- Secure SDLC
- Git Security
- Supply Chain Security
- Threat Hunting
- Digital Forensics
- Incident Response
- AI in Cybersecurity
- Red Team & Blue Team Operations

---

# ⚠️ Disclaimer

This project was developed **strictly for educational and research purposes** inside a fully isolated VMware laboratory. All attacks are simulated against fictional infrastructure.

---

# 👨‍💻 Author

**Kshitij Shrivastava**

**CDAC PGCP-ITISS Capstone Project**

---

⭐ *If you found this project interesting, consider giving it a star!*

