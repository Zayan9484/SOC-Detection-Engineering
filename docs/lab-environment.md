# Lab Environment

## Purpose

This virtual lab is designed to test and improve security-event detection using Wazuh, Sysmon, Suricata, and controlled attack simulations.

## Host System

| Component | Specification |
|---|---|
| Host OS | Windows 11 |
| CPU | AMD Ryzen 5 PRO 4650U |
| RAM | 16 GB |
| Free storage at project start | 92 GB |
| Hypervisor | VMware Workstation 17.5.0 |

## Virtual Machines

| Virtual Machine | Purpose | RAM | vCPU |
|---|---|---:|---:|
| Wazuh 4.12.0 | SIEM manager, indexer, and dashboard | 6.1 GB | 4 |
| Windows 10 | Monitored Windows endpoint | 2 GB | 2 |
| Ubuntu 24.04.2 LTS | Linux endpoint and Suricata sensor | 2 GB | 2 |
| Kali Linux | Controlled attack simulation | 2 GB | 4 |

## Network Configuration

The environment currently uses:

- NAT for connectivity between the virtual machines and software updates.
- VMnet1 as a custom isolated lab network.
- The Wazuh agents communicate with the Wazuh Manager through its reachable NAT address.

## Resource Constraints

Because the host has 16 GB RAM, all virtual machines will not normally run simultaneously. Only the machines required for each detection scenario will be powered on.

## Wazuh Resource Optimization

The Wazuh Indexer initially failed because its Java heap was too large for the available VM memory. The heap was reduced from approximately 3 GB to 1 GB.

After this change:

- Wazuh Manager was active.
- Wazuh Indexer was active.
- Wazuh Dashboard was accessible.
- The Windows agent successfully connected.

The 1 GB indexer heap is a lab-specific configuration and is not intended as a production sizing recommendation.