# 🔐 Secure Enterprise Network — Cisco Packet Tracer

## 📌 Project Overview

Designed and implemented a segmented enterprise network using Cisco Packet Tracer with a focus on network security, secure device management, and Layer 2 protection.

The network uses VLAN segmentation, Router-on-a-Stick inter-VLAN routing, DHCP, SSH, ACLs, Port Security, DHCP Snooping, Dynamic ARP Inspection, BPDU Guard, PortFast, and centralized Syslog monitoring.

## 🏗️ Network Architecture

The enterprise network consists of:

- 1 × Cisco 2911 Router
- 3 × Cisco 2960 Switches
- 6 × Client PCs
- 1 × Server

### Network Topology
                         ┌─────────────────┐
                         │       R1        │
                         │ Cisco 2911      │
                         │ Router-on-a-Stick│
                         └────────┬────────┘
                                  │
                              802.1Q Trunk
                                  │
                         ┌────────┴────────┐
                         │      SW1        │
                         │   Core Switch   │
                         └───┬─────────┬───┘
                             │         │
                         Trunk       Trunk
                             │         │
                       ┌─────┴───┐ ┌───┴─────┐
                       │  SW2    │ │   SW3   │
                       │ Access  │ │  Access │
                       └─┬─┬─┬─┬─┘ └──┬──┬──┘
                         │ │ │ │       │  │
                        PC1 PC2 PC3 PC4 PC5 PC6

                         SERVER1
                       VLAN 40
                         │
                        SW1

| Device | Role |
|--------|------|
| R1 | Inter-VLAN routing, DHCP, ACL, SSH, Syslog |
| SW1 | Core/distribution switch |
| SW2 | HR/IT access switch |
| SW3 | SALES access switch |
| SERVER1 | Server VLAN + Syslog server |
| PC1–PC6 | Departmental clients |

## 🎯 Objectives

- Segment departments using VLANs
- Enable secure inter-VLAN communication
- Provide centralized DHCP services
- Secure network-device management using SSH
- Protect access ports against unauthorized devices
- Prevent rogue DHCP servers
- Protect against ARP spoofing
- Restrict access to the server network using ACLs
- Centralize network event logging using Syslog
- Disable unused switch ports

## 🛠️ Technologies & Tools

- Cisco Packet Tracer
- Cisco IOS
- VLANs
- 802.1Q Trunking
- Router-on-a-Stick
- DHCP
- SSH v2
- Standard & Extended ACLs
- Port Security
- DHCP Snooping
- Dynamic ARP Inspection (DAI)
- PortFast
- BPDU Guard
- Syslog

## 🏢 Network Departments

| VLAN | Department | Network |
|------|------------|---------|
| 10 | HR | 192.168.10.0/24 |
| 20 | IT | 192.168.20.0/24 |
| 30 | SALES | 192.168.30.0/24 |
| 40 | SERVERS | 192.168.40.0/24 |
| 99 | MANAGEMENT | 192.168.99.0/24 |

## 🔒 Security Features

- SSH version 2 for secure remote administration
- Management VLAN for infrastructure access
- SSH access restricted using an ACL
- Port Security with sticky MAC addresses
- Maximum one MAC address per access port
- Shutdown violation action
- DHCP Snooping
- Dynamic ARP Inspection
- PortFast and BPDU Guard
- Unused switch ports administratively shut down
- Extended ACL protecting the Server VLAN
- Centralized Syslog monitoring
