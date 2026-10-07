# Security Controls

This project implements multiple Cisco security mechanisms to protect the enterprise network at both Layer 2 and Layer 3.

## 1. VLAN Segmentation

The network is divided into separate VLANs:

- VLAN 10 — HR
- VLAN 20 — IT
- VLAN 30 — SALES
- VLAN 40 — SERVERS
- VLAN 99 — MANAGEMENT

VLAN segmentation reduces unnecessary broadcast traffic and provides logical separation between departments.

## 2. SSH Version 2

All network devices use SSH version 2 for secure remote administration.

Configured on:

- R1
- SW1
- SW2
- SW3

Telnet is disabled on the VTY lines.

## 3. Management VLAN

Network-device management interfaces use VLAN 99.

| Device | Management IP |
|--------|---------------|
| R1 | 192.168.99.1 |
| SW1 | 192.168.99.10 |
| SW2 | 192.168.99.11 |
| SW3 | 192.168.99.12 |

## 4. SSH Access Control

A standard ACL restricts SSH access to the management network:

```text
permit 192.168.99.0 0.0.0.255