# Scenario 05 — Windows File Integrity Monitoring

## Objective
Monitor a sensitive Windows directory and detect file creation, modification, and deletion events using Wazuh File Integrity Monitoring (FIM).

## Systems
- Endpoint: Windows 10
- Wazuh Agent: `windows-agent`
- SIEM: Wazuh 4.12.0
- Monitored Directory: `C:\fim-demo`

## FIM Configuration
The Wazuh agent was configured to monitor the test directory in real time and report file content changes:

```xml
<syscheck>
  <directories realtime="yes" report_changes="yes">C:\fim-demo</directories>
</syscheck>
```

## Test 1 — File Creation
A new file was created inside the monitored directory:

```powershell
"Initial SOC Lab Content" | Out-File C:\fim-demo\soc-test.txt
```

### Detection Result
Wazuh generated:

- Description: `File added to the system.`
- Rule ID: `554`
- Rule Level: `5`

## Test 2 — File Modification
The monitored file was modified:

```powershell
"Suspicious configuration change" | Add-Content C:\fim-demo\soc-test.txt
```

### Detection Result
Wazuh generated:

- Description: `Integrity checksum changed.`
- Rule ID: `550`
- Rule Level: `7`

## Test 3 — File Deletion
The monitored file was deleted:

```powershell
Remove-Item C:\fim-demo\soc-test.txt
```

### Detection Result
Wazuh generated:

- Description: `File deleted.`
- Rule ID: `553`
- Rule Level: `7`

## Observed Result
Wazuh successfully monitored the complete lifecycle of the test file inside `C:\fim-demo`.

The endpoint generated separate alerts for file creation, modification, and deletion, demonstrating real-time host-based integrity monitoring.

## Evidence
- `11-wazuh-windows-fim-detection.png` — Wazuh FIM alerts showing file integrity events