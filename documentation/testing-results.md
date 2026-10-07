# Testing & Verification Results

The network was tested after configuration to verify connectivity, security controls, and management access.

## 1. DHCP Verification

R1 successfully assigned IP addresses to all six client PCs.

| Department | Clients | Result |
|---|---|---|
| HR | PC1, PC2 | PASS |
| IT | PC3, PC4 | PASS |
| SALES | PC5, PC6 | PASS |

## 2. Server Connectivity

All departmental networks successfully reached SERVER1 (`192.168.40.10`).

| Source | Destination | Result |
|---|---|---|
| PC1 — HR | SERVER1 | PASS — 0% loss |
| PC3 — IT | SERVER1 | PASS — 0% loss |
| PC5 — SALES | SERVER1 | PASS — 0% loss |

## 3. Management VLAN

PC6 was temporarily placed into VLAN 99 to test management access.

PC6 successfully reached SW1:

```text
PC6: 192.168.99.20
SW1: 192.168.99.10
Result: PASS