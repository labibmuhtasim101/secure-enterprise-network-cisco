# Security Controls

This project implements multiple Cisco security mechanisms across Layer 2, Layer 3, and device management.

The objective is to reduce the attack surface, protect access ports, secure management access, prevent common Layer 2 attacks, and control access to the server network.

---

## 1. VLAN Segmentation

The network is divided into separate VLANs:

| VLAN | Name | Purpose |
|------|------|---------|
| 10 | HR | Human Resources |
| 20 | IT | IT Department |
| 30 | SALES | Sales Department |
| 40 | SERVERS | Server Network |
| 99 | MANAGEMENT | Network Device Management |

VLAN segmentation separates departments into different broadcast domains and provides logical network isolation.

---

## 2. 802.1Q Trunking

802.1Q trunking is used between:

- R1 and SW1
- SW1 and SW2
- SW1 and SW3

The trunks carry the required VLANs:

- VLAN 10
- VLAN 20
- VLAN 30
- VLAN 40
- VLAN 99

SW1's inter-switch trunks were explicitly configured using:

```text
switchport mode trunk
