# 🔐 Secure Enterprise Network — Cisco Packet Tracer

A secure multi-VLAN enterprise network designed and implemented in Cisco Packet Tracer using Cisco routers and switches.

This project demonstrates practical **networking, network security, infrastructure hardening, and troubleshooting** skills using a realistic enterprise-style topology.

---

## 📌 Project Overview

The network represents a small enterprise environment with separate departments, a dedicated server network, and a management network.

The design uses **VLAN segmentation, router-on-a-stick inter-VLAN routing, DHCP, SSH management, ACLs, port security, DHCP Snooping, Dynamic ARP Inspection, and centralized Syslog logging**.

### Main Goals

- Segment departments using VLANs
- Enable inter-VLAN communication
- Provide centralized DHCP
- Secure network-device administration using SSH
- Restrict unauthorized management access
- Protect access ports using Port Security
- Prevent rogue DHCP servers using DHCP Snooping
- Protect against ARP spoofing using Dynamic ARP Inspection
- Control access to the Server VLAN using ACLs
- Centralize network-device logs using Syslog
- Disable unused switch ports
- Validate network connectivity and security controls

---

# 🏗️ Network Architecture

![Network Topology](topology/network-topology.png)

### Devices

| Device | Role |
|---|---|
| R1 | Router / Inter-VLAN Routing / DHCP / ACL |
| SW1 | Core/Distribution Switch |
| SW2 | Access Switch |
| SW3 | Access Switch |
| PC1–PC2 | HR Clients |
| PC3–PC4 | IT Clients |
| PC5–PC6 | Sales Clients |
| SERVER1 | Server / Syslog Server |

---

# 🌐 VLAN & IP Addressing

| VLAN | Department | Network | Gateway |
|---|---|---|---|
| 10 | HR | 192.168.10.0/24 | 192.168.10.1 |
| 20 | IT | 192.168.20.0/24 | 192.168.20.1 |
| 30 | SALES | 192.168.30.0/24 | 192.168.30.1 |
| 40 | SERVERS | 192.168.40.0/24 | 192.168.40.1 |
| 99 | MANAGEMENT | 192.168.99.0/24 | 192.168.99.1 |

### Management IPs

| Device | Management IP |
|---|---|
| SW1 | 192.168.99.10 |
| SW2 | 192.168.99.11 |
| SW3 | 192.168.99.12 |

### Server

| Device | IP Address |
|---|---|
| SERVER1 | 192.168.40.10 |

---

# 🔀 Routing

Inter-VLAN routing is implemented using **Router-on-a-Stick**.

R1 uses the following subinterfaces:

G0/0.10 → VLAN 10 → 192.168.10.1
G0/0.20 → VLAN 20 → 192.168.20.1
G0/0.30 → VLAN 30 → 192.168.30.1
G0/0.40 → VLAN 40 → 192.168.40.1
G0/0.99 → VLAN 99 → 192.168.99.1

---

🛡️ Security Controls

## 1. SSH Management

All network devices are configured for:
- SSH version 2
- Local user authentication
- Privilege level 15
- VTY access restricted to the Management VLAN
Management network:
192.168.99.0/24

---

## 2. Port Security

User-facing switch ports use:
- Maximum 1 MAC address
- Sticky MAC addresses
- Violation mode: Shutdown
Applied to:
SW2 Fa0/2–Fa0/5
SW3 Fa0/2–Fa0/3

---

## 3. DHCP Snooping

DHCP Snooping is enabled on user VLANs to help prevent rogue DHCP servers.
Trusted uplinks:
SW1 ↔ SW2
SW1 ↔ SW3
SW1 ↔ R1

Client-facing ports remain untrusted.

---

## 4. Dynamic ARP Inspection

DAI is enabled on:
VLAN 10
VLAN 20
VLAN 30

This helps protect the network from ARP spoofing and ARP poisoning attacks.

---

## 5. Server VLAN ACL

An extended ACL controls traffic destined for the Server VLAN.
Server network:
192.168.40.0/24

Authorized internal networks include:
HR
IT
SALES
MANAGEMENT

---

## 6. Unused Port Hardening

Unused switch ports were administratively disabled using:
shutdown

This reduces the attack surface of the switches.

---

## 7. PortFast & BPDU Guard

PortFast and BPDU Guard are enabled on end-device access ports.
This helps protect the access layer from accidental or unauthorized BPDU transmission.

---

## 8. Centralized Syslog

Network devices send system logs to:
SERVER1
192.168.40.10

Syslog uses:
UDP 514

This provides centralized visibility into network events and configuration changes.
📡 DHCP
R1 provides DHCP services for:
VLAN 10 — HR
VLAN 20 — IT
VLAN 30 — SALES

Example address allocation:
HR    → 192.168.10.0/24
IT    → 192.168.20.0/24
SALES → 192.168.30.0/24

The Server VLAN uses a static server address.

🧪 Testing & Verification
The following tests were performed successfully:
Test	Result
VLAN configuration	✅ Passed
Trunk configuration	✅ Passed
DHCP allocation	✅ Passed
Inter-VLAN routing	✅ Passed
SSH management	✅ Passed
SSH access restriction	✅ Passed
Port Security	✅ Passed
DHCP Snooping	✅ Passed
Dynamic ARP Inspection	✅ Passed
Server ACL	✅ Passed
Syslog	✅ Passed
Cross-VLAN connectivity	✅ Passed


Connectivity
Successful tests included:
PC1 → SERVER1
PC3 → SERVER1
PC5 → SERVER1

All final connectivity tests completed successfully with:
4/4 replies
0% packet loss

---

🧰 Technologies & Concepts

Networking
- Cisco IOS
- Cisco Packet Tracer
- VLANs
- 802.1Q Trunking
- Router-on-a-Stick
- Inter-VLAN Routing
- DHCP
- IPv4
- Subnetting

Network Security
- SSH
- Access Control Lists
- Port Security
- DHCP Snooping
- Dynamic ARP Inspection
- BPDU Guard
- PortFast
- Unused-port hardening
- Network segmentation

Monitoring
- Syslog
- Centralized logging
- Network troubleshooting

---

🎯 Skills Demonstrated

This project demonstrates practical experience with:
- Cisco IOS configuration
- Switch configuration
- Router configuration
- VLAN design
- IP addressing
- Inter-VLAN routing
- DHCP configuration
- Network security hardening
- Access control
- Secure remote administration
- Layer 2 security
- Network troubleshooting
- Security monitoring
- Documentation

---

🚀 Future Improvements

Possible future improvements include:
- Firewall implementation
- NAT/PAT
- VPN
- Wireless network integration
- Dedicated monitoring server
- SNMP monitoring
- Network intrusion detection
- Redundant gateways
- Spanning Tree optimization
- AAA authentication
- RADIUS/TACACS+ integration

---

👨‍💻 Author

Computer Science Student | Networking & Cybersecurity Enthusiast

Interested in:
- Network Engineering
- Cybersecurity
- SOC Operations
- Network Security
- Infrastructure Security
