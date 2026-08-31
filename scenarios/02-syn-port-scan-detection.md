# Scenario 02 — TCP SYN Port Scan Detection

## Objective
Detect a TCP SYN port scan launched from Kali Linux against the Ubuntu host using a custom Suricata rule, then verify that the alert is forwarded to Wazuh.

## Systems
- Attacker: Kali Linux — 192.168.23.133
- Target/Sensor: Ubuntu 24.04 — 192.168.23.131
- IDS: Suricata 8.0.0
- SIEM: Wazuh 4.12.0

## Detection Rule

alert tcp any any -> $HOME_NET any (msg:"LAB Possible TCP SYN Port Scan"; flags:S; threshold:type both, track by_src, count 20, seconds 10; sid:1000002; rev:1;) 


## Attack Simulation
From Kali Linux:

sudo nmap -sS -p 1-1000 192.168.23.131

## Observed Result
The scan successfully triggered the custom Suricata detection rule and the resulting alert was forwarded to Wazuh.

- Suricata SID: `1000002`
- Source: `192.168.23.133` (Kali Linux)
- Destination: `192.168.23.131` (Ubuntu)
- Alert: `LAB Possible TCP SYN Port Scan`
- Wazuh Rule ID: `86601`
- Wazuh Rule Level: `3`

## Evidence
- `05-kali-nmap-syn-scan.png` — Nmap SYN scan from Kali
- `06-suricata-syn-scan-detection.png` — Custom Suricata alert
- `07-wazuh-syn-scan-detection.png` — Alert received by Wazuh

