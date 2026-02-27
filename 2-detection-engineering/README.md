# 2. Detection Engineering – SSH Brute Force Monitoring

## Objective

To design and validate detection logic for identifying SSH brute-force attempts and successful compromises using Wazuh SIEM.

---

## Log Source

- Log File: /var/log/auth.log
- Monitored by: Wazuh Agent (ubuntu-victim-01)
- Log Type: Linux Authentication Logs
- Service: SSHD

Wazuh agent forwards logs from the endpoint to the Wazuh Manager for analysis.

---

## Detection Strategy

The detection focuses on identifying:

1. Multiple failed SSH login attempts
2. Followed by a successful login from the same IP
3. Privilege escalation activity (sudo usage)

---

## Detection Indicators

### Failed Login Attempts

Wazuh Rule Groups Observed:
- sshd
- authentication_failed
- invalid_login

Example Log:

Failed password for invalid user fakeuser from 192.168.100.10 port 42358 ssh2


---

### Successful Authentication

Rule Group:
- authentication_success

Example Log:

Accepted password for ubuntu from 192.168.100.10 port 43904 ssh2


---

### Privilege Escalation

Program: sudo

Example Log:

session opened for user root(uid=0) by ubuntu(uid=1000)


This confirms post-compromise activity.

---

## Detection Logic Flow

1. Monitor repeated failed SSH attempts.
2. Track source IP address.
3. Check for successful login from same IP.
4. Investigate subsequent sudo activity.

This confirms brute-force leading to compromise.

---

## Alert Severity

- Failed attempts → Medium severity
- Successful login after brute force → High severity
- Root escalation → Critical severity

---

## Validation

Attack simulation using Hydra generated:
- 200+ failed authentication attempts
- Successful login event
- Post-login privilege escalation

All events were successfully ingested and searchable in Wazuh Discover.

---

## Outcome

Detection successfully identified:

- Initial access attempt
- Credential brute force
- Successful compromise
- Privilege escalation
