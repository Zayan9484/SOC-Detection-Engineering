# Detection Summary

This document summarizes the detection scenarios implemented in the SOC Detection Engineering Lab.

| # | Scenario | Activity / Attack | Telemetry Source | Detection Engine | Rule ID / SID | Severity | MITRE ATT&CK |
|---|---|---|---|---|---|---|---|
| 1 | ICMP Ping Detection | ICMP echo request sent from Kali to Ubuntu | Suricata network traffic | Suricata + Wazuh | Suricata SID `1000001` / Wazuh Rule `86601` | Level 3 | N/A |
| 2 | TCP SYN Port Scan Detection | Nmap SYN scan against Ubuntu | Suricata network traffic | Suricata + Wazuh | Suricata SID `1000002` / Wazuh Rule `86601` | Level 3 | N/A |
| 3 | SSH Brute-Force Detection | Multiple failed SSH authentication attempts | Ubuntu authentication logs | Wazuh | Rule `5763` | Level 10 | `T1110` — Brute Force |
| 4 | Windows Brute-Force Correlation | Multiple failed Windows logon attempts | Windows Security Event ID `4625` | Wazuh | Custom Rule `100100` | Level 10 | `T1110` — Brute Force |
| 5 | Windows File Integrity Monitoring | File creation, modification, and deletion | Wazuh Syscheck / FIM | Wazuh | Rules `554`, `550`, `553` | Levels 5–7 | N/A |
| 6 | Encoded PowerShell Execution | Sysmon Event ID 1 | Wazuh Rule 92057 | 12 | T1059.001 - PowerShell |

## Detection Coverage

The lab demonstrates multiple detection approaches:

- Network-based detection using Suricata
- Endpoint log monitoring using Wazuh agents
- Authentication failure detection
- Event correlation for brute-force activity
- Custom detection rule engineering
- File Integrity Monitoring
- Centralized alert analysis through the Wazuh dashboard

## Key Outcomes

The project demonstrates the complete detection workflow:

1. Generate controlled malicious or suspicious activity.
2. Collect network or endpoint telemetry.
3. Apply built-in or custom detection logic.
4. Forward and analyze alerts in Wazuh.
5. Validate alerts using rule IDs, severity levels, and supporting evidence.
6. Document detection logic and results in GitHub.

## Detection Rules Developed

Two custom Suricata rules were created:

- SID `1000001` — ICMP Ping Detection
- SID `1000002` — TCP SYN Port Scan Detection

One custom Wazuh correlation rule was created:

- Rule `100100` — Multiple Windows failed logon attempts within 60 seconds

## Evidence

Supporting screenshots for every scenario are stored in:

```text
screenshots/
```

Detailed scenario documentation is stored in:

```text
scenarios/
```

Detection rule files are stored in:

```text
detection-rules/
```