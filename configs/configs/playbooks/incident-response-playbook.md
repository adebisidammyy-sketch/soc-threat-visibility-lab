# 🚨 Incident Response Playbook
## SOC Threat Visibility Lab — FireOps Systems

---

## General Response Flow

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

## Playbook 1 — Brute Force (SSH)

**Trigger:** Wazuh Rule 5763 — Multiple authentication failures

**Steps:**
1. Check source IP in Wazuh alert
2. Confirm repeated failed logins from same IP
3. Block IP using firewall: `sudo ufw deny from <attacker-ip>`
4. Check if any login succeeded after brute force
5. If successful login found — escalate immediately
6. Document attacker IP, time, and affected host
7. Monitor for further attempts

**MITRE:** T1110.001 — Brute Force: Password Guessing

---

## Playbook 2 — Privilege Escalation

**Trigger:** Wazuh Rule 5402 — Successful sudo to ROOT executed

**Steps:**
1. Identify which user ran sudo command
2. Check if user is authorised to use sudo
3. Review command that was executed
4. If unauthorised — disable user account immediately
5. Check for any files created or modified by the user
6. Review /etc/sudoers for unauthorised changes
7. Escalate to Security Engineer
8. Document and close

**MITRE:** T1548.003 — Sudo and Sudo Caching

---

## Playbook 3 — Data Exfiltration

**Trigger:** File Integrity Monitoring alert or unusual outbound traffic

**Steps:**
1. Identify what data was accessed
2. Check destination IP/URL data was sent to
3. Block outbound connection immediately
4. Isolate affected host from network
5. Check /etc/passwd and sensitive files for access
6. Notify management if sensitive data confirmed exfiltrated
7. Preserve logs as evidence
8. Document and close

**MITRE:** T1041 — Exfiltration Over C2 Channel

---

## Playbook 4 — Network Port Scan (Nmap/Suricata)

**Trigger:** Suricata Rule 86601 — ET POLICY Possible Kali Linux hostname

**Steps:**
1. Identify scanning source IP
2. Check what ports were scanned
3. Determine if any open ports were discovered by attacker
4. Block scanning IP: `sudo ufw deny from <scanner-ip>`
5. Review open ports and close unnecessary ones
6. Check for follow-up attack attempts after scan
7. Document and close

**MITRE:** T1046 — Network Service Discovery

---

## Playbook 5 — AWS Console Brute Force

**Trigger:** CloudTrail ConsoleLogin failed events

**Steps:**
1. Check CloudTrail for source IP of failed logins
2. Count number of failed attempts
3. If more than 5 attempts — block IP in AWS WAF
4. Enable MFA on affected account immediately
5. Check if any login succeeded
6. Review IAM activity after login attempts
7. Document and close

**MITRE:** T1110 — Brute Force

---

## Playbook 6 — AWS Privilege Escalation

**Trigger:** CloudTrail AttachUserPolicy AccessDenied

**Steps:**
1. Identify which IAM user attempted escalation
2. Review all recent IAM actions by that user
3. Check if escalation succeeded anywhere else
4. Revoke user access keys immediately
5. Review IAM policies for unauthorised changes
6. Enable CloudTrail alerts for all IAM changes
7. Document and close

**MITRE:** T1078 — Valid Accounts

---

## Playbook 7 — AWS Data Exfiltration (S3)

**Trigger:** CloudTrail S3 GetObject or unusual ListBucket activity

**Steps:**
1. Identify which S3 bucket was accessed
2. Check what files were downloaded
3. Identify source IP and IAM user
4. Revoke access keys immediately
5. Enable S3 bucket versioning and access logging
6. Check if data was made public
7. Notify management if sensitive data confirmed stolen
8. Document and close

**MITRE:** T1530 — Data from Cloud Storage
