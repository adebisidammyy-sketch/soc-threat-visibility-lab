# 🔐 SOC Threat Visibility Lab
### Wazuh + Elastic Stack | Docker | AWS CloudTrail | Suricata IDS

![Wazuh](https://img.shields.io/badge/Wazuh-4.14.5-blue)
![Docker](https://img.shields.io/badge/Docker-Containerized-blue)
![AWS](https://img.shields.io/badge/AWS-CloudTrail-orange)
![Status](https://img.shields.io/badge/Status-Completed-green)

---

## 📌 Project Overview

A fully operational Security Operations Center (SOC) home lab built from scratch, simulating real-world threat detection, cloud monitoring, and incident response workflows.

Built as part of **Cybersecurity Project 1 — 2026** for **FireOps Systems**, a fictional fintech startup operating across multiple African countries with on-premise and cloud infrastructure.

---

## 🏗️ Architecture

![Architecture Diagram](Soc%20threat-visibility%20pictures/architecture.png)

**Components:**
- **Kali Linux VM** — Wazuh Manager + Dashboard running on Docker
- **Ubuntu Linux VM** — Wazuh Agent + Suricata IDS
- **AWS Cloud** — CloudTrail logging → S3 bucket → Wazuh
- **Attacker Machine** — Kali Linux (simulated attacks)

---

## 🛠️ Tools & Technologies

| Tool | Purpose |
|------|---------|
| Wazuh 4.14.5 | SIEM & threat detection |
| Elastic Stack | Log indexing & visualization |
| Docker | Container deployment |
| Suricata IDS | Network intrusion detection |
| AWS CloudTrail | Cloud API logging |
| Hydra | Brute force simulation |
| Nmap | Network port scanning |
| Kali Linux | Attacker machine |
| Ubuntu 22.04 | Target/agent machine |

---

## ⚔️ Attack Simulations

### Local VM Attacks

**Attack 1 — SSH Brute Force**
- Tool: Hydra
- Target: Ubuntu SSH (port 22)
- Detection: Wazuh Rule 5763 — Multiple authentication failures
- MITRE: T1110.001

**Attack 2 — Privilege Escalation**
- Tool: Sudo abuse
- Target: Ubuntu local user
- Detection: Wazuh Rule 5402 — Successful sudo to ROOT
- MITRE: T1548.003

**Attack 3 — Data Exfiltration**
- Tool: curl / netcat
- Target: webhook.site (external)
- Detection: File integrity monitoring + network anomaly
- MITRE: T1041

**Attack 4 — Network Port Scan**
- Tool: Nmap (-A -v aggressive scan)
- Target: Ubuntu VM
- Detection: Suricata IDS — ET POLICY Possible Kali Linux hostname
- MITRE: T1046

### AWS Cloud Attacks

**Attack 5 — AWS Console Brute Force**
- Method: Failed console login attempts
- Detection: CloudTrail ConsoleLogin failed events
- MITRE: T1110

**Attack 6 — AWS Privilege Escalation**
- Method: aws iam attach-user-policy AdministratorAccess
- Detection: CloudTrail AttachUserPolicy AccessDenied
- MITRE: T1078

**Attack 7 — AWS Data Exfiltration**
- Method: S3 bucket enumeration and copy
- Detection: CloudTrail S3 ListBucket + GetObject events
- MITRE: T1530

---

## 📊 Dashboards

Two custom dashboards built in Wazuh:

**SOC Analyst Dashboard:**
- Alert severity breakdown
- Top fired rules chart
- MITRE ATT&CK bar chart
- Active monitored agents count
- Alerts trend over time
- Top monitored endpoint IPs

**Executive Dashboard:**
- Total incidents (24h)
- Severity breakdown donut chart
- Alerts trend line chart

---

## 🚨 Incident Response Playbook

| Step | Action | Owner |
|------|--------|-------|
| 1 | Alert fires in Wazuh Dashboard | SOC Analyst |
| 2 | Triage — confirm not false positive | Analyst |
| 3 | Escalate if High or Critical severity | Analyst → Team Lead |
| 4 | Investigate — review logs, source IP | Security Engineer |
| 5 | Contain — block IP, isolate host | Engineer |
| 6 | Eradicate — remove threat, patch | Engineer |
| 7 | Recover — restore services | Engineer |
| 8 | Document and close ticket | All |

---

## 📁 Repository Structure

soc-threat-visibility-lab/

├── README.md

├── configs/

│   ├── ossec.conf-aws-block.xml

│   └── local_rules.xml

├── screenshots/

│   ├── architecture.png

│   ├── dashboards/

│   ├── attacks/

│   └── aws/

└── playbooks/

└── incident-response-playbook.md

---

## 🧠 Key Learnings

- Real SIEM deployments involve constant troubleshooting
- Cloud log integration requires correct IAM permissions and network access
- Docker container permissions can silently break services
- XML configuration errors crash entire security stacks
- Persistence and switching tools when needed is a core security skill

---

## ⚠️ Challenges Faced

- Spent 5 days on Azure cloud monitoring with zero logs generated — switched to AWS
- Wazuh API disconnections caused by wrong file permissions on ossec.conf
- Docker container had no internet access blocking AWS S3 log pulls
- Duplicate rule IDs crashing wazuh-analysisd
- AWS credentials not accessible to Wazuh container user

---

## 👤 Adebisi Damilola Abiola
**Cybersecurity Project 1 •
Cybersecurity Student | SOC Analyst in Training
2026

---

## 📜 License

MIT License — feel free to use this as a reference for your own home lab.
