# File Integrity Monitoring (FIM)

## 1. Objective

Configure Wazuh to monitor a dedicated directory on a macOS endpoint and detect when files are added, modified, or deleted.

## 2. Lab Environment

- **Endpoint:** macOS
- **Monitoring agent:** Wazuh Agent 4.14.8
- **SIEM platform:** Wazuh 4.14.8
- **Monitored directory:** `/Users/abs/WazuhLab`
- **Monitoring mode:** Scheduled scan
- **Scan interval:** 60 seconds

## 3. Lab Procedure

### 3.1 Configure the Monitored Directory

Configured the Wazuh agent to monitor the dedicated lab directory:

`/Users/abs/WazuhLab`

The agent was restarted to apply the configuration.

### 3.2 Test File Creation

Created a test file named `test-file.txt` inside the monitored directory.

**Command:**

```bash
echo "First SOC FIM alert" > ~/WazuhLab/test-file.txt
```

**Observed result:**

Wazuh generated an alert indicating that a file had been added to the system.

- **Rule ID:** `554`
- **Alert description:** `File added to the system.`
- **Alert level:** `5`
- **Event type:** `added`

### 3.3 Test File Modification

Appended new content to the existing test file.

**Command:**

```bash
echo "This file has been modified." >> ~/WazuhLab/test-file.txt
```

**Observed result:**

Wazuh recorded a file modification event. This demonstrated how changes to an existing file within the monitored directory can be detected.

### 3.4 Test File Deletion

Deleted the test file.

**Command:**

```bash
rm ~/WazuhLab/test-file.txt
```

**Observed result:**

Wazuh recorded the file deletion event.

## 4. Investigation Notes

- **Monitoring scope:** Only files within the configured monitoring scope are covered by this exercise.
- **File integrity:** File hashes can help analysts identify changes in file contents and support further investigation.
- **Scheduled scans:** Detection timing depends on the configured monitoring mode and scan interval.
- **Event visibility:** The selected dashboard time range must include the event timestamp for the event to appear in the current view.
- **Security context:** File changes are not inherently malicious. Analysts must consider the file, the change, and the surrounding activity before assessing potential risk.

## 5. Conclusion

Successfully tested file creation, modification, and deletion within a dedicated macOS lab directory monitored by Wazuh FIM.

The exercise demonstrated how endpoint file activity can generate security events that analysts can review during monitoring and investigation. It also reinforced the importance of monitoring scope, event timestamps, and contextual analysis when interpreting security alerts.

## 6. Evidence

- [View the FIM alert screenshot](../screenshots/wazuh-fim-alert.png)

The screenshot provides supporting evidence for the FIM exercise. Individual modification and deletion alert details can be documented separately if additional evidence is captured.

**Lab note:** All tests were performed in a personal lab using a dedicated test directory on a system under my control.

