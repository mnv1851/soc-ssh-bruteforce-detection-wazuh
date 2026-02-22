# soc-ssh-bruteforce-detection-wazuh
Full attack lifecycle SOC lab: simulated SSH brute-force attack, endpoint log analysis using custom Bash scripts, detection and alerting with Wazuh SIEM, investigation workflow, MITRE ATT&amp;CK mapping, and incident response documentation.

## Executive Summary

This project demonstrates a full attack lifecycle simulation and detection of an SSH brute-force attack in a controlled SOC lab environment.

The objective was to simulate a realistic attack, detect it using custom bash-based detection logic and Wazuh SIEM, perform log analysis, map activity to MITRE ATT&CK, and generate a formal incident report.

The lab replicates how a SOC Analyst would handle authentication-based attacks in an enterprise environment.

⸻

## Lab Architecture

Attacker Machine: Kali Linux
Victim Machine: Ubuntu Server (Wazuh Agent Installed)
SIEM Platform: Wazuh Manager & Dashboard
Network: Isolated VirtualBox Lab Environment

## Flow:

Attacker (Hydra) → Ubuntu Victim (SSH Service) → Wazuh Agent → Wazuh Manager → Dashboard Investigation

⸻

## Attack Scenario

An SSH brute-force attack was simulated using Hydra against the Ubuntu victim machine.

The attacker attempted thousands of password combinations targeting the SSH service (port 22).

After multiple authentication failures, a successful login was generated to simulate account compromise.

⸻

## Detection Mechanisms

1. Manual Bash-Based Detection

Custom scripts were developed to:
	•	Detect excessive failed SSH login attempts
	•	Identify potential brute-force patterns
	•	Highlight suspicious IP addresses
	•	Log security alerts locally

Scripts are available in the /scripts directory.

⸻

2. Wazuh SIEM Detection

Wazuh detected:
	•	Multiple failed authentication attempts
	•	Invalid user login attempts
	•	Successful SSH authentication after brute-force activity
	•	Privilege escalation using sudo

Investigation was performed using:
	•	agent.name filtering
	•	SSH log queries
	•	Accepted password event searches
	•	Rule level analysis
	•	Correlation of source IP activity

⸻

## MITRE ATT&CK Mapping
| Stage	           | Technique        	| ID        |
|------------------|--------------------|-----------|
| Initial Access   | Brute Force        | T1110 |
|Persistence (if account created) |Create Account |	T1136|
|Privilege Escalation|	Abuse Elevation Control Mechanism|	T1548|
|Credential Access	|Valid Accounts	|T1078|
|Discovery	|Account Discovery	|T1087|
