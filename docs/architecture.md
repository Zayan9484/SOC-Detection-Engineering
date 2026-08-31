# SOC Detection Engineering Lab — Architecture

![SOC Detection Engineering Lab Architecture](images/soc-lab-architecture.png)

## Overview

This lab simulates a small SOC environment where network and endpoint telemetry is collected, analyzed, and correlated using Suricata and Wazuh.

The environment combines network-based detection, endpoint monitoring, SIEM analysis, and custom correlation rules in a controlled virtual lab.

## Core Components

- **Kali Linux** — Used for attack simulation and controlled security testing
- **Ubuntu 24.04** — Monitored Linux endpoint and Suricata network sensor
- **Windows 10** — Monitored Windows endpoint
- **Suricata IDS** — Detects suspicious network activity using custom IDS rules
- **Wazuh Agent** — Collects endpoint logs, security events, and FIM telemetry
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

### Windows Endpoint Detection

The Windows Wazuh agent collects Windows Security events and File Integrity Monitoring telemetry.

Failed logon events are detected using Windows Event ID `4625`, while Wazuh FIM monitors changes inside:

```text
C:\fim-demo
```

Built-in and custom Wazuh rules are then used to identify suspicious activity and correlate repeated authentication failures.

This flow is used for:

- Windows Failed-Logon Detection
- Windows Brute-Force Correlation
- Windows File Integrity Monitoring

## Detection Logic

The project uses three types of detection logic:

- **Custom Suricata Rules** — Network-based detection
- **Built-in Wazuh Rules** — Endpoint and authentication detection
- **Custom Wazuh Correlation** — Repeated Windows failed-logon detection

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