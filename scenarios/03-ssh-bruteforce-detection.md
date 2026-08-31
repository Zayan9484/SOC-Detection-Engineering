# Scenario 03 — SSH Brute-Force Detection

## Objective
Detect repeated failed SSH authentication attempts from Kali Linux against the Ubuntu host and verify that Wazuh correlates the failures into a brute-force alert.

## Systems
- Attacker: Kali Linux — `192.168.23.133`
- Target: Ubuntu 24.04 — `192.168.23.131`
- Service: OpenSSH
- SIEM: Wazuh 4.12.0

## Attack Simulation
From Kali Linux:

```bash
for i in {1..10}; do sshpass -p 'WrongPassword123!' ssh -o StrictHostKeyChecking=no zayan@192.168.23.131 "exit"; done
```

## Wazuh Detection Logic
Individual failed SSH attempts generated:

- Rule ID: `5760`
- Description: `sshd: authentication failed.`
- Level: `5`

Wazuh then correlated repeated failures using its built-in brute-force rule:

- Rule ID: `5763`
- Description: `sshd: brute force trying to get access to the system. Authentication failed.`
- Level: `10`
- Frequency: `8`
- Timeframe: `120 seconds`
- Correlation: Same source IP
- MITRE ATT&CK: `T1110` — Brute Force

An additional PAM correlation alert was also generated:

- Rule ID: `5551`
- Description: `PAM: Multiple failed logins in a small period of time.`
- Level: `10`

## Observed Result
The repeated failed SSH attempts from Kali Linux were successfully detected and correlated by Wazuh into a high-severity brute-force alert.

## Evidence
- `08-wazuh-ssh-bruteforce-detection.png` — Wazuh brute-force correlation alert
- `09-kali-ssh-bruteforce-test.png` — Kali attack command