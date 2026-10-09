# Wazuh Home SOC Lab
## Overview

A hands-on cybersecurity lab documenting my journey learning security monitoring, endpoint detection, and basic SOC investigation using Wazuh.

I built a monitoring pipeline connecting a macOS endpoint to an Ubuntu Server running Wazuh. Through controlled lab exercises, I explored how system activity is collected, analyzed, and turned into security alerts. 

## Lab Architecture

- **SIEM and monitoring platform:** Wazuh 4.14.8
- **Server:** Ubuntu Server 24.04 LTC
- **Endpoint:** macOS running Wazuh Agent 4.14.8
- **Virtualisation:** VMWare Fusion
- **Detection areas:** File Integrity Monitoring (FIM), SSH authentication failures and sudo activity
- **Threat framework:** MITRE ATT&CK

The Ubuntu server hosts the Wazuh Manager, Indexer, and Dashboard. A Wazuh agent on my MacBook sends endpoint security events to the monitoring platform.

## Security Monitoring Pipeline

The lb demonstrates how endpoint and system activity flows through a security monitoring pipeline:

1. Generate a controlled activity or security event
2. Collect the relevant event or log
3. Process the event using Wazuh decoders and detection rules
4. Review the resulting alert in Wazuh
5. Investigate the event and assess its context and potential security impact

### 1. File Integrity Monitoring (FIM)

**Objective:** Detect changes to files in a monitored endpoint directory.

**What I did:**

- Configured Wazuh to monitor a dedicated lab directory endpoint directory on macOS.
- Created a test file an verified the resulting alert.
- Modified the file and observed the corresponding change event.
- Deleted the file and confirmed the deletion event.

**Result:** Successfully tested the file lifecycle - added, modified, deleted and reviewed the resulting events in Wazuh.

### SSH Authentication Monitoring

**Objective:** Generate and investigate a failed SSH authentication event.

**What I did:**

- Installed and verified the OpenSSH server on Ubuntu.
- Generated a single failed SSH password attempt against the local Ubuntu machine.
- Examined the authentication logs using `journalctl`.
- Located the corresponding Wazuh alert.

**Observed detection:**

- Rule ID: `5760`
- Alert description: `sshd: authentication failed.`
- Alert level: `5`
- MITRE ATT&CK: `T1110.001` - Password Guessing
- MITRE ATT&CK: `T1021.004` - SSH

**Result:** Confirmed that Wazuh collected the failed authentication event and generated an alert. The test originated from local host (`127.0.0.1`) and was intentionally performed in my own lab.

### 3. Sudo Activity Monitoring

**Objective:** Explore how Wazuh records privileged command execution.

**What I observed:**

- Wazuh generated a level 3 alert for successful sudo activity.
- Rule ID: `5402`
- Alert description: `Successful sudo to ROOT executed.`
- MITRE ATT&CK: `T1548.003` - Sudo and Sudo Caching

This alert was observed during routine lab troubleshooting. A dedicated privileged-monitoring exercise and further investigation are planned.

### Key Learnings

- Configuring a SIEM-style monitoring environment.
- Registering and troubleshooting an endpoint agent.
- Understanding file integrity monitoring and scheduled scans.
- Reading Linux authentication logs with journalctl.
- Interpreting Wazuh alert levels, rule IDs, decoders, and event fields.
- Connecting security detections to MITRE ATT&CK techniques.
- Distinguishing authorized test activity from potentially suspicious behavior.
- Following a basic alert triage and investigation workflow.

### Next Steps

- Practice detecting repeated failed authentication attempts.
- Investigate sudo activity and privilege-related events.
- Improve alert triage and incident documentation.
- Capture sanitized dashboard screenshots and document investigation findings.
- Expand the lab with additional detection scenarios.

### Disclaimer

This is personal learning lab. All test are conducted on systems under my control. The project is intended for educational purposes and does not represent production SOC experience.
