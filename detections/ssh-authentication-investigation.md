# SSH Authentication Failure Investigation

## 1. Objective

Investigate a failed SSH authentication event detected by Wazuh and practise basic SOC alert triage.

## 2. Environment

* **SIEM:** Wazuh 4.14.8
* **Operating system:** Ubuntu Server 24.04 LTS
* **Service:** OpenSSH
* **Test type:** Controlled failed password authentication
* **Source IP:** `127.0.0.1` (localhost)

## 3. Detection Details

Wazuh generated an alert after an intentionally incorrect password was entered during an SSH login attempt against the local Ubuntu server.

| Field             | Value                           |
| ----------------- | ------------------------------- |
| Rule ID           | `5760`                          |
| Alert description | `sshd: authentication failed.`  |
| Alert level       | `5`                             |
| Username          | `daengerzuan`                   |
| Source IP         | `127.0.0.1`                     |
| MITRE ATT&CK      | `T1110.001` — Password Guessing |
| MITRE ATT&CK      | `T1021.004` — SSH               |

## 4. Investigation Process

### Step 1: Generate a controlled event

Attempted to connect to the local SSH service using:

`ssh daengerzuan@127.0.0.1`

An incorrect password was entered, resulting in an authentication failure.

### Step 2: Verify the system logs

Reviewed the Ubuntu SSH service logs using:

`sudo journalctl -u ssh --since "5 minutes ago"`

The logs contained a failed password entry for the test account.

### Step 3: Verify the Wazuh detection

Reviewed the Wazuh manager's alert records in:

`/var/ossec/logs/alerts/alerts.json`

The corresponding event was identified with rule ID `5760` and the description `sshd: authentication failed.`

## 5. SOC Triage Assessment

**Observed activity:** A failed SSH password authentication attempt.

**Source assessment:** The source address was `127.0.0.1`, meaning the connection originated from the same Ubuntu machine.

**Potential security significance:** Failed SSH authentication can indicate an incorrect password, unauthorised access attempts, or password guessing. A single event alone does not establish malicious activity.

**Investigation outcome:** Benign, authorised test activity. The event was deliberately generated as part of this lab exercise.

**Recommended follow-up:** In a real environment, correlate repeated failures with source addresses, usernames, timestamps, and any subsequent successful logins before deciding whether escalation is necessary.

## 6. Key Learnings

* How to generate and identify a controlled SSH authentication failure.
* How to review Linux service logs using `journalctl`.
* How Wazuh decodes authentication events and applies detection rules.
* How to interpret alert severity and MITRE ATT&CK mappings.
* Why alert context is essential when deciding whether activity is suspicious.

## 7. Conclusion

Successfully traced a controlled SSH authentication failure from the Ubuntu system logs to a Wazuh Level 5 alert. This exercise demonstrated a basic SOC workflow covering event generation, log verification, detection review, and contextual triage.

## Evidence

![Wazuh SSH authentication failure alert](../Screenshot%202026-10-12%20at%2012.23.06.png)

The screenshot below shows Wazuh detecting a failed SSH authentication attempt (Rule ID 5760, Level 5).

*This exercise was conducted on a personal lab system under my control.*

