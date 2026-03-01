# 3. Manual Log Analysis – Custom SSH Brute-Force Detection
## Objective

This section demonstrates manual detection of SSH brute-force activity using linux commands : grep, awk, sort, uniq, head, and custom Bash scripts.

The goal was to simulate how a SOC analyst performs log-based investigation without relying entirely on SIEM automation.

### Linux command focused on :
- Filtering logs for better tracing of attack.
- Getting overview of how /var/log/auth.log store logs in files using grep-head-command.
  
  ![grep-head-command](../screenshots/log-analysis/grep-head-command.jpeg)
- Picking attacker timestamp, ip, port for further information using grep-awk-sort-uniq-command.
  
  ![grep-awk-sort-uniq-command](../screenshots/log-analysis/grep-awk-sort-uniq-command.jpeg)
  
### The detection logic focuses on:

- Parsing /var/log/auth.log

- Identifying repeated failed SSH login attempts

- Applying time-based filtering

- Applying threshold-based detection

- Correlating failed attempts with successful login events

- Generating local alert logs

## Detection Design

### Two custom scripts were developed:

- ssh_bruteforce_detector.sh

- ssh_compromise_detector.sh

### These scripts implement:

- Log extraction

- Time-window filtering

- IP-based aggregation

- Threshold comparison

- Correlation with successful authentication

- Alert generation

## Script Configuration
LOGFILE="/var/log/auth.log"
THRESHOLD=5
WINDOW_MINUTES=5

- LOGFILE → Source authentication log

- THRESHOLD → Number of failed attempts required to trigger detection

- WINDOW_MINUTES → Time window for detection logic

## Detection Workflow
- 1. Extract Failed SSH Attempts
grep -a "Failed password" $LOGFILE

| This identifies all SSH authentication failures.

- 2. Convert Log Time to Epoch
CURRENT_TIME=$(date +%s)
LOG_SECONDS=$(date -d "$LOG_TIME" +%s)
DIFF=$(( (CURRENT_TIME - LOG_SECONDS) / 60 ))

| This enables time-based filtering to isolate events within the defined window.

- 3. Extract and Count Source IPs
grep -oE '[0-9]+\.[0-9]+\.[0-9]+\.[0-9]+' | sort | uniq -c

| This aggregates failed attempts per IP address.

- 4. Threshold-Based Detection
if [ "$count" -gt "$THRESHOLD" ];

| If failed attempts exceed the configured threshold within the time window, the IP is flagged as suspicious.

- 5. Compromise Correlation
SUCCESS=$(sudo grep -a "Accepted password" $LOGFILE | grep "$ip")

| If a successful login occurs from the same IP after multiple failures:

### Classified as CRITICAL

### Indicates brute-force success and potential account compromise

| If no success occurs:

### Classified as WARNING

### Indicates brute-force attempt without confirmed compromise

## Alert Output

Alerts are written to:

- ssh_security_alerts.log

Example output:

- CRITICAL: SSH brute-force succeeded from 192.168.100.10 (7 failed attempts)
- WARNING: SSH brute-force detected from 192.168.100.10 (6 failed attempts)

## Skills Demonstrated

- Linux log analysis

- Bash scripting for detection

- Time-based event correlation

- IP aggregation logic

- Threshold-based alert engineering

- Manual compromise validation

- SOC-style triage workflow

