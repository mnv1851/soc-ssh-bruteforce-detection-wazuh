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

Scripts are available in the [scripts](./scripts) directory.

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
|Persistence  |Create Account |	T1136|
|Privilege Escalation|	Abuse Elevation Control Mechanism|	T1548|
|Credential Access	|Valid Accounts	|T1078|
|Discovery	|Account Discovery	|T1087|

## Investigation Highlights
1.Identified source IP performing repeated authentication attempts

2.Observed “Failed password” log spikes
3.Correlated timeline of attack activity
4.Confirmed successful login event
5.Tracked post-authentication sudo activity
6.Validated rule triggers and alert levels in Wazuh

⸻

## Skills Demonstrated
1.Attack Simulation (Hydra SSH brute force)
2.Log Analysis (Linux authentication logs)
3.Bash Scripting for detection automation
4.SIEM Investigation (Wazuh)
5.Alert Tuning & Rule Analysis
6.MITRE ATT&CK Mapping
7.Incident Reporting
8.SOC Workflow Documentation

⸻

## Project Structure
- [1. Attack Simulation](./1-attack-simulation)
- [2. Detection Engineering](./2-detection-engineering)
- [3. Manual Log Analysis](./3-manual-log-analysis)
- [4. SIEM Investigation](./4-siem-investigation)
- [5. MITRE ATT&CK Mapping](./5-mitre-mapping)
- [6. Incident Report](./6-incident-report)
- [screenshots](./screenshots)
- [scripts](./scripts)

## Outcome

This project demonstrates hands-on SOC Level 1 capabilities including detection validation, event correlation, incident investigation, and reporting in a simulated enterprise security environment.

The entire lifecycle from attack execution to investigation and documentation was completed independently.
