# SOC Detection Engineering Lab

A virtual SOC lab for evaluating and improving security-event detection using Wazuh, Sysmon, Suricata, and MITRE ATT&CK.

## Project Status

**Phase 1: Environment setup and baseline validation — In Progress**

## Objectives

- Build a small virtual SOC monitoring environment.
- Generate controlled security events.
- Evaluate default detection coverage.
- Investigate alerts using endpoint and network telemetry.
- Create and tune custom detection rules.
- Compare baseline and improved detection results.
- Map tested scenarios to MITRE ATT&CK.

## Core Components

- Wazuh SIEM
- Windows 10 endpoint
- Sysmon
- Ubuntu Linux
- Suricata IDS
- Kali Linux
- VMware Workstation

## Detection Workflow

Attack Simulation
        ↓
Endpoint or Network Telemetry
        ↓
Wazuh Collection and Analysis
        ↓
Alert Investigation
        ↓
Detection Tuning
        ↓
Controlled Retest

## Repository Structure

- `docs/` — Architecture and environment documentation
- `scenarios/` — Controlled attack and detection scenarios
- `detection-rules/` — Custom Wazuh and Suricata rules
- `results/` — Baseline and tuned detection results
- `screenshots/` — Supporting evidence
- `configs/` — Relevant configuration files

## Disclaimer

All attack simulations are performed in an isolated, controlled virtual lab for defensive security research and education.
