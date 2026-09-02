# SOC Detection Engineering Lab — Architecture

![SOC Detection Engineering Lab Architecture](images/soc-lab-architecture.png)

## Overview

This lab simulates a small SOC environment where network and endpoint telemetry is collected, analyzed, correlated, and investigated using Suricata, Sysmon, and Wazuh.

The environment combines network-based detection, Windows and Linux endpoint monitoring, enhanced process telemetry, SIEM analysis, and custom correlation rules in a controlled virtual lab.

## Core Components

- **Kali Linux** — Used for attack simulation and controlled security testing
- **Ubuntu 24.04** — Monitored Linux endpoint and Suricata network sensor
- **Windows 10** — Monitored Windows endpoint
- **Suricata IDS** — Detects suspicious network activity using custom IDS rules
- **Sysmon** — Provides detailed Windows endpoint telemetry including process creation and command-line activity
- **Wazuh Agent** — Collects endpoint logs, security events, Sysmon events, Suricata alerts, and FIM telemetry
- **Wazuh Manager** — Analyzes events and applies detection/correlation rules
- **Wazuh Indexer** — Stores security events and alerts
- **Wazuh Dashboard** — Used for threat hunting, alert investigation, and validation

## Detection Flows

### Network Detection

Kali Linux generates controlled network activity against the Ubuntu endpoint.

Suricata analyzes this traffic and writes alerts to:

```text
/var/log/suricata/eve.json
```

The Wazuh agent collects these alerts and forwards them to the Wazuh Manager for centralized analysis and visualization.

This flow is used for:

- ICMP Ping Detection
- TCP SYN Port Scan Detection

### Linux Endpoint Detection

Repeated SSH authentication attempts are generated from Kali Linux against the Ubuntu endpoint.

Ubuntu records failed authentication activity in its authentication logs. The Wazuh agent forwards these events to the Wazuh Manager, where built-in Wazuh correlation rules detect brute-force behavior.

This flow is used for:

- SSH Brute-Force Detection

### Windows Security & FIM Detection

The Windows Wazuh agent collects Windows Security events and File Integrity Monitoring telemetry.

Failed logon events are detected using Windows Event ID `4625`, while Wazuh FIM monitors changes inside:

```text
C:\fim-demo
```

Built-in and custom Wazuh rules are used to identify suspicious activity and correlate repeated authentication failures.

This flow is used for:

- Windows Failed-Logon Detection
- Windows Brute-Force Correlation
- Windows File Integrity Monitoring

### Windows Sysmon Detection

Sysmon is installed on the Windows endpoint to provide enhanced process telemetry.

Sysmon records process-creation activity in:

```text
Microsoft-Windows-Sysmon/Operational
```

The Windows Wazuh agent collects the Sysmon event channel and forwards the telemetry to the Wazuh Manager.

For the encoded PowerShell scenario, the detection flow is:

```text
PowerShell Execution
        |
        v
Sysmon Event ID 1
(Process Create)
        |
        v
Windows Wazuh Agent
        |
        v
Wazuh Manager
        |
        v
Wazuh Rule 92057
        |
        v
Level 12 Alert
```

This flow is used for:

- PowerShell Process Monitoring
- Command-Line Visibility
- Encoded PowerShell Detection

The encoded PowerShell detection is mapped to MITRE ATT&CK technique `T1059.001`.

## Detection Logic

The project uses multiple types of detection logic:

- **Custom Suricata Rules** — Network-based ICMP and TCP SYN scan detection
- **Built-in Wazuh Rules** — Endpoint, authentication, FIM, and Sysmon-based detections
- **Custom Wazuh Correlation** — Repeated Windows failed-logon detection
- **Sysmon Telemetry + Wazuh Analysis** — Detailed Windows process activity analyzed using Wazuh detection rules

## Investigation Workflow

Selected high-severity alerts are investigated after detection.

The analyst workflow used in the lab is:

```text
Security Alert
      |
      v
Review Alert Details
      |
      v
Identify Source & Target
      |
      v
Correlate Underlying Events
      |
      v
Review MITRE Mapping
      |
      v
Determine True / False Positive
      |
      v
Document Remediation
```

Dedicated investigation reports are maintained in the `investigations/` directory.

## Lab Environment

The lab runs in **VMware Workstation 17.5.0** on a **Windows 11** host.

Because of limited system memory, only the virtual machines required for each scenario are powered on simultaneously.

Typical combinations include:

```text
Network / Linux Testing:
Wazuh + Ubuntu + Kali

Windows Testing:
Wazuh + Windows 10
```

This keeps the environment stable while still supporting full telemetry collection, detection, correlation, and investigation.