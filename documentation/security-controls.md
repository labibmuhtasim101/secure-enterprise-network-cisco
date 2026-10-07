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

switchport mode trunk

---

## 3. Router-on-a-Stick

R1 performs inter-VLAN routing using 802.1Q subinterfaces.
Each VLAN has its own gateway:

VLAN 10 → 192.168.10.1
VLAN 20 → 192.168.20.1
VLAN 30 → 192.168.30.1
VLAN 40 → 192.168.40.1
VLAN 99 → 192.168.99.1

---

## 4. SSH Version 2

SSH version 2 is enabled on:
- R1
- SW1
- SW2
- SW3
SSH provides encrypted remote management instead of insecure Telnet.
Verified configuration:

SSH Enabled - version 2.0
Authentication timeout: 120 seconds
Authentication retries: 3

Telnet access is disabled on the configured VTY lines.

---

## 5. Management VLAN

Network device management is separated into VLAN 99.
Management addresses:

R1  → 192.168.99.1
SW1 → 192.168.99.10
SW2 → 192.168.99.11
SW3 → 192.168.99.12

This prevents normal departmental user networks from being used as the primary management network.

---

## 6. SSH Access Control

A standard ACL restricts SSH access to the switches.
ACL:

access-list 10 permit 192.168.99.0 0.0.0.255

The ACL is applied to the VTY lines.
Therefore:

Management VLAN 99 → SSH access allowed
Other VLANs        → SSH access blocked

This was tested successfully.
A PC from VLAN 10 was unable to establish an SSH session with SW1, while a PC temporarily placed in VLAN 99 successfully connected.

---

## 7. Local Authentication

Local username authentication is used for SSH management.
The configured administrative account uses a secret password rather than a plaintext password.
The switches and router also use an enable secret for privileged EXEC access.

---

## 8. Port Security

Port Security is enabled on client-facing access ports.
Configuration includes:

Maximum MAC addresses: 1
MAC learning: Sticky
Violation action: Shutdown

Protected ports include:

SW2
Fa0/2
Fa0/3
Fa0/4
Fa0/5

SW3
Fa0/2
Fa0/3

Port Security verification showed:

Maximum secure addresses: 1
Current secure addresses: 1
Security violations: 0

This helps prevent unauthorized devices from being connected to protected access ports.

---

## 9. DHCP Snooping

DHCP Snooping is enabled on user VLANs.

SW1
VLAN 10
VLAN 20
VLAN 30

SW2
VLAN 10
VLAN 20

SW3
VLAN 30

Trusted interfaces are configured toward legitimate DHCP sources and uplinks.
Client-facing ports are untrusted and rate-limited to:
10 packets per second

DHCP Snooping helps prevent rogue DHCP servers from providing unauthorized network configuration.
Option 82 insertion was disabled because of Packet Tracer compatibility issues encountered during DHCP testing.

---

## 10. Dynamic ARP Inspection

Dynamic ARP Inspection (DAI) is enabled on:

SW1
VLAN 10
VLAN 20
VLAN 30

SW2
VLAN 10
VLAN 20

SW3
VLAN 30

DAI uses DHCP Snooping information to validate ARP traffic and helps protect against ARP spoofing and ARP poisoning attacks.

All switches reported:

Operation: Active
Dropped packets: 0
DHCP drops: 0
ACL drops: 0
IP validation failures: 0

---

## 11. PortFast and BPDU Guard

PortFast and BPDU Guard are enabled on end-device access ports.
PortFast allows end-device ports to transition to the forwarding state quickly.
BPDU Guard protects access ports from receiving unexpected Bridge Protocol Data Units.
This helps protect the Spanning Tree topology from unauthorized switch connections.

---

12. Unused Port Shutdown

Unused switch ports were administratively disabled.

This reduces the physical attack surface and prevents unused interfaces from being used to connect unauthorized devices.

---

13. Server VLAN ACL

An extended ACL named:

SERVER-ACCESS

protects the Server VLAN.
Authorized internal networks are explicitly permitted to access the server network:

192.168.10.0/24 → 192.168.40.0/24
192.168.20.0/24 → 192.168.40.0/24
192.168.30.0/24 → 192.168.40.0/24
192.168.99.0/24 → 192.168.40.0/24

A final deny statement prevents other sources from accessing the Server VLAN:

deny ip any 192.168.40.0 0.0.0.255

The ACL is applied outbound on the Server VLAN subinterface.

---

14. Centralized Syslog

Network devices send Syslog messages to SERVER1.

Syslog Server: 192.168.40.10
Protocol: UDP
Port: 514

Syslog is configured on:
- R1
- SW1
- SW2
- SW3
SERVER1's Syslog service is enabled.

R1 verification showed:
Syslog logging: enabled
Logging to 192.168.40.10
UDP port 514
Link: up
Messages dropped: 0

SERVER1 successfully received network-device log messages.

---

15. Device Password Protection

The infrastructure devices use:
- Enable secret
- Console password
- Local username authentication
- SSH version 2
These controls provide multiple layers of authentication for device management.

---

16. Security Design Summary

The project combines multiple security mechanisms:

VLAN Segmentation
        ↓
Management VLAN
        ↓
SSH v2 + ACL
        ↓
Port Security
        ↓
DHCP Snooping
        ↓
Dynamic ARP Inspection
        ↓
PortFast + BPDU Guard
        ↓
Server VLAN ACL
        ↓
Centralized Syslog

Together, these controls provide layered protection for the enterprise network.
