# File Integrity Monitoring (FIM)

## Objective

Configure Wazuh to monitor a dedicated directory on a macOS endpoint and detect when files are added, modified, or deleted.

## Environment

* **Endpoint:** macOS
* **Monitoring agent:** Wazuh Agent 4.14.8
* **SIEM platform:** Wazuh 4.14.8
* **Monitored directory:** `/Users/abs/WazuhLab`
* **Monitoring mode:** Scheduled scan
* **Scan frequency:** 60 seconds

## Lab Procedure

### 1. Configure the monitored directory

Configured the Wazuh agent to monitor the dedicated lab directory:

`/Users/abs/WazuhLab`

The agent was restarted to apply the configuration.

### 2. Test file creation

Created a test file named `test-file.txt` inside the monitored directory.

Command used:

`echo "First SOC FIM alert" > ~/WazuhLab/test-file.txt`

**Observed result:** Wazuh generated an alert indicating that a file had been added to the system.

* **Rule ID:** `554`
* **Alert description:** `File added to the system.`
* **Alert level:** `5`
* **Event:** `added`

### 3. Test file modification

Appended new content to the test file.

Command used:

`echo "This file has been modified." >> ~/WazuhLab/test-file.txt`

**Observed result:** Wazuh recorded the file modification in its FIM events.

### 4. Test file deletion

Deleted the test file.

Command used:

`rm ~/WazuhLab/test-file.txt`

**Observed result:** Wazuh recorded the deletion event.

## Investigation Notes

* FIM events can help identify unexpected changes to monitored files.
* File hashes can support investigations by showing whether file contents have changed.
* The monitoring scope matters: files outside the configured directory are not covered by this specific test.
* The dashboard time range must include the event timestamp; otherwise, relevant events may not appear in the selected view.

## Conclusion

Successfully tested the file creation, modification, and deletion lifecycle using Wazuh FIM on a macOS endpoint. This exercise demonstrated how endpoint file activity can be collected and surfaced as security events for investigation.

## Evidence

Add sanitised screenshots of the Wazuh dashboard showing the file-added, file-modified, and file-deleted events.

*Note: The tests were performed in a personal lab using a dedicated test directory.*
