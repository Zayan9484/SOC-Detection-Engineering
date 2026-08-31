# SOC Detection Engineering Lab

A hands-on SOC detection engineering lab built using **Wazuh, Suricata, Windows, Ubuntu, and Kali Linux**.

The project demonstrates how suspicious activity can be generated in a controlled environment, collected as network or endpoint telemetry, detected using built-in and custom rules, correlated in a SIEM, and validated through analyst investigation.

---

## Architecture

![SOC Detection Engineering Lab Architecture](docs/images/soc-lab-architecture.png)

The lab combines:

- Network IDS monitoring with Suricata
- Windows and Linux endpoint telemetry
- Wazuh SIEM analysis
- Built-in detection rules
- Custom Suricata signatures
- Custom Wazuh correlation
- File Integrity Monitoring
- Controlled attack simulation
- Alert investigation and evidence collection

Detailed architecture documentation:

[View Lab Architecture](docs/architecture.md)

---

## Detection Scenarios

| # | Detection Scenario | Detection Source | Result |
|---|---|---|---|
| 01 | ICMP Ping Detection | Suricata + Wazuh | Detected |
| 02 | TCP SYN Port Scan Detection | Suricata + Wazuh | Detected |
| 03 | SSH Brute-Force Detection | Wazuh | Correlated — Level 10 |
| 04 | Windows Brute-Force Detection | Windows Event Logs + Wazuh | Custom Correlation — Level 10 |
| 05 | Windows File Integrity Monitoring | Wazuh FIM | Add / Modify / Delete Detected |

### Scenario Documentation

- [01 — ICMP Ping Detection](scenarios/01-icmp-detection.md)
- [02 — TCP SYN Port Scan Detection](scenarios/02-syn-port-scan-detection.md)
- [03 — SSH Brute-Force Detection](scenarios/03-ssh-bruteforce-detection.md)
- [04 — Windows Brute-Force Detection](scenarios/04-windows-bruteforce-detection.md)
- [05 — Windows File Integrity Monitoring](scenarios/05-windows-file-integrity-monitoring.md)

---

## Custom Detection Engineering

### Suricata — ICMP Detection

```text
alert icmp any any -> $HOME_NET any (msg:"LAB ICMP Ping Detected"; itype:8; sid:1000001; rev:1;)
```

### Suricata — TCP SYN Scan Detection

```text
alert tcp any any -> $HOME_NET any (msg:"LAB Possible TCP SYN Port Scan"; flags:S; threshold:type both, track by_src, count 20, seconds 10; sid:1000002; rev:1;)
```

### Wazuh — Windows Brute-Force Correlation

```xml
<rule id="100100" level="10" frequency="5" timeframe="60">
  <if_matched_sid>60122</if_matched_sid>
  <description>Windows: Multiple failed logon attempts detected - Possible brute force attack.</description>
  <mitre>
    <id>T1110</id>
  </mitre>
</rule>
```

This rule correlates repeated Windows failed-logon events into a higher-severity brute-force alert.

---

## Detection Workflow

```text
Generate Activity
        |
        v
Collect Telemetry
        |
        v
Apply Detection Logic
        |
        v
Generate Alert
        |
        v
Investigate in Wazuh
        |
        v
Validate Detection
        |
        v
Document Evidence
```

---

## Lab Environment

| Component | Purpose |
|---|---|
| Wazuh 4.12.0 | SIEM, event analysis, correlation, and investigation |
| Suricata 8.0.0 | Network IDS |
| Kali Linux | Attack simulation |
| Ubuntu 24.04 | Linux endpoint and Suricata sensor |
| Windows 10 | Windows endpoint |
| VMware Workstation 17.5.0 | Virtualization platform |

The environment runs on a Windows 11 host with limited memory, so only the virtual machines required for each scenario are powered on simultaneously.

---

## Detection Coverage

The project demonstrates:

- Network traffic detection
- ICMP monitoring
- Port-scan detection
- Linux authentication monitoring
- SSH brute-force correlation
- Windows Security Event monitoring
- Windows Event ID `4625` analysis
- Custom SIEM correlation
- MITRE ATT&CK mapping
- File Integrity Monitoring
- Detection validation
- Threat hunting in Wazuh

---

## Repository Structure

```text
SOC-Detection-Engineering-Lab/
│
├── configs/
│   └── windows-fim-config.xml
│
├── detection-rules/
│   ├── suricata-local.rules
│   └── wazuh-windows-rules.xml
│
├── docs/
│   ├── images/
│   │   └── soc-lab-architecture.png
│   ├── architecture.md
│   └── lab-environment.md
│
├── results/
│   └── detection-summary.md
│
├── scenarios/
│   ├── 01-icmp-detection.md
│   ├── 02-syn-port-scan-detection.md
│   ├── 03-ssh-bruteforce-detection.md
│   ├── 04-windows-bruteforce-detection.md
│   └── 05-windows-file-integrity-monitoring.md
│
├── screenshots/
│   └── Detection evidence
│
└── README.md
```

---

## Results

The lab successfully demonstrated an end-to-end detection pipeline across both network and endpoint telemetry.

Key detections included:

- Custom Suricata ICMP alert
- Custom Suricata SYN scan alert
- Wazuh SSH brute-force correlation
- Custom Windows brute-force correlation
- Windows file creation detection
- Windows file modification detection
- Windows file deletion detection

A consolidated detection matrix is available here:

[View Detection Summary](results/detection-summary.md)

---

## Disclaimer

This project was created for educational and defensive cybersecurity purposes.

All attack simulations were performed inside an isolated virtual lab against systems owned and controlled by the author.