# Incident Response Report
## 1. Executive Summary

On analysis of authentication logs from the Ubuntu server (192.168.100.20), suspicious SSH activity was identified originating from 192.168.100.10.

The activity involved a high volume of failed SSH login attempts followed by a successful authentication and subsequent privilege escalation using sudo. The behavior is consistent with a brute-force attack resulting in valid credential usage.

The incident was contained within a controlled SOC lab environment. No lateral movement or persistence mechanisms were observed.

### Severity Level: Medium

## 2. Incident Overview

- Attack Type: SSH Brute-Force
- Target System: Ubuntu Server (192.168.100.20)
- Attacker System: Kali Linux (192.168.100.10)
- Log Source: /var/log/auth.log
- Monitoring System: Wazuh SIEM

## 3. Initial Detection

The incident was identified through abnormal authentication patterns in /var/log/auth.log, including:

- Multiple “Failed password” entries

- Invalid user login attempts

- PAM authentication failures

- High-frequency login attempts within short time window

A successful login event (“Accepted password”) was later observed from the same source IP.

This pattern indicates a transition from brute-force attempts to successful credential usage.

## 4. Technical Analysis
### 4.1 Log Evidence

The following log artifacts were observed:

- Failed password for invalid user fakeuser from 192.168.100.10

- Failed password for ubuntu from 192.168.100.10

- Accepted password for ubuntu from 192.168.100.10

- Session opened for user ubuntu

- Session opened for user root by ubuntu(uid=0)

These logs confirm:

- Repeated authentication failures

- Successful authentication

- Privilege escalation via sudo

## 5. Timeline of Events
|Time (Relative)	| Event |
|-----------------|--------|
|T1	|SSH port confirmed open via reconnaissance|
|T2	|High volume of failed SSH login attempts|
|T3	|Authentication threshold exceeded|
|T4	|Successful SSH login for user ubuntu|
|T5	|Sudo privilege escalation initiated|
|T6|	Root session opened|

The sequence demonstrates a complete attack chain from initial access attempt to elevated privileges.

## 6. MITRE ATT&CK Mapping
- T1110 – Brute Force

Hydra was used to attempt multiple password combinations against SSH.

- T1021.004 – Remote Services: SSH

SSH was used as the remote access mechanism.

- T1078 – Valid Accounts

Valid credentials were used to successfully authenticate.

- T1548.003 – Abuse Elevation Control Mechanism

The attacker executed sudo -i to escalate privileges to root.

## 7. Impact Assessment

- Unauthorized authentication occurred.

- Privilege escalation to root was achieved.

- No evidence of:

-- Lateral movement

-- Persistence mechanisms

-- Data exfiltration

-- Malware deployment

Since the activity occurred in a controlled lab environment, business impact was negligible. However, in a production environment, this level of access would represent a significant security risk.

## 8. Containment Actions (Recommended for Production Environment)

If this occurred in a live environment, the following actions would be taken:

- Immediately block source IP 192.168.100.10 at firewall level.

- Disable or lock compromised account.

- Force password reset for affected user.

- Review sudo logs for additional malicious commands.

- Inspect system for persistence mechanisms.

- Review SSH configuration settings.

- Enable rate limiting or fail2ban.

- Enforce key-based authentication only.

- Enable multi-factor authentication.

## 9. Lessons Learned

- Brute-force activity is highly visible in authentication logs.

- Correlation between repeated failures and successful login is critical.

- Privilege escalation shortly after login significantly increases incident severity.

- SSH services exposed without rate limiting are high-risk targets.

- Monitoring root session creation should trigger high-priority alerts.

## 10. Incident Classification

Severity: Medium

Rationale:

- Successful authentication and privilege escalation occurred.

- No evidence of further compromise stages.

- Contained within lab environment.

In a production system, this incident would likely be classified as High severity due to root-level access.
