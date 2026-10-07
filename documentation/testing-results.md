# Testing & Verification Results

After completing the network configuration, multiple connectivity, security, and management tests were performed in Cisco Packet Tracer.

The objective was to verify that the network operates correctly while the implemented security controls remain active.

---

## 1. VLAN Verification

VLANs were verified on SW1.

| VLAN | Name | Status |
|------|------|--------|
| 10 | HR | Active |
| 20 | IT | Active |
| 30 | SALES | Active |
| 40 | SERVERS | Active |
| 99 | MANAGEMENT | Active |

SERVER1 was confirmed to be connected to VLAN 40 through SW1 Fa0/10.

**Result: PASS**

---

## 2. Trunk Verification

Trunk interfaces were verified on SW1.

| Interface | Connection | Status |
|-----------|------------|--------|
| Fa0/1 | SW2 | Trunking |
| Fa0/2 | SW3 | Trunking |
| G0/1 | R1 | Trunking |

The required VLANs were active and forwarding across the trunks:

10, 20, 30, 40, 99

SW1 Fa0/2 was explicitly configured as a trunk to avoid relying on dynamic trunk negotiation.
Result: PASS

---

## 3. Router Interface Verification

R1 was verified using:
show ip interface brief

All required subinterfaces were operational:

Interface	IP Address	Status
G0/0.10	192.168.10.1	Up/Up
G0/0.20	192.168.20.1	Up/Up
G0/0.30	192.168.30.1	Up/Up
G0/0.40	192.168.40.1	Up/Up
G0/0.99	192.168.99.1	Up/Up

The physical G0/0 interface was also confirmed as Up/Up.
Result: PASS

---

## 4. DHCP Verification

R1 successfully assigned IP addresses to all six client PCs.

Verified DHCP bindings:

Client	VLAN	Assigned IP
PC1	10	192.168.10.2
PC2	10	192.168.10.3
PC3	20	192.168.20.2
PC4	20	192.168.20.3
PC5	30	192.168.30.2
PC6	30	192.168.30.3

Result: PASS

---

## 5. HR → Server Connectivity

PC1 from the HR VLAN was tested against SERVER1.

Source: PC1
Source VLAN: 10
Destination: 192.168.40.10

Ping result:
Packets Sent: 4
Packets Received: 4
Packet Loss: 0%

Result: PASS

---

## 6. IT → Server Connectivity

PC3 from the IT VLAN was tested against SERVER1.

Source: PC3
Source VLAN: 20
Destination: 192.168.40.10

Ping result:
Packets Sent: 4
Packets Received: 4
Packet Loss: 0%

Result: PASS

---

## 7. SALES → Server Connectivity

PC5 from the SALES VLAN was tested against SERVER1.
Source: PC5
Source VLAN: 30
Destination: 192.168.40.10

Ping result:
Packets Sent: 4
Packets Received: 4
Packet Loss: 0%

Result: PASS

---

## 8. Management VLAN Connectivity

PC6 was temporarily moved from SALES VLAN 30 to MANAGEMENT VLAN 99 to test management access.

PC6 was configured with:
IP Address: 192.168.99.20
Subnet Mask: 255.255.255.0
Default Gateway: 192.168.99.1

PC6 successfully reached SW1:
PC6 → 192.168.99.10

The first ping timed out while ARP/MAC information was being learned, followed by successful replies.
Result: PASS

PC6 was subsequently restored to VLAN 30 and DHCP.

---

## 9. Unauthorized SSH Access Test

PC1 from VLAN 10 attempted to connect to SW1 using SSH:
ssh -l admin 192.168.99.10

The connection was closed by the switch.
This was expected because the SSH VTY ACL permits only the Management VLAN:
192.168.99.0/24

Therefore, a device from VLAN 10 was prevented from accessing SW1 through SSH.
Result: PASS — Unauthorized access blocked

---

## 10. Authorized SSH Access Test

PC6 was temporarily placed in Management VLAN 99.

PC6 successfully connected to SW1 using:
ssh -l admin 192.168.99.10

The SSH session successfully reached:
SW1>

After entering the enable password, the session successfully reached:
SW1#

This verified:
- SSH connectivity
- Local user authentication
- SSH version 2
- Privileged EXEC access
- Management VLAN access control
Result: PASS

---

## 11. SSH Version Verification

SSH version 2 was verified on all infrastructure devices.

Device	SSH Version	Result
R1	Version 2	PASS
SW1	Version 2	PASS
SW2	Version 2	PASS
SW3	Version 2	PASS


Verified configuration:
SSH Enabled - version 2.0
Authentication timeout: 120 secs
Authentication retries: 3

Result: PASS

---

## 12. Port Security Verification

Port Security was verified on SW2 and SW3.
SW2
Port	Maximum MAC	Current MAC	Violations	Action
Fa0/2	1	1	0	Shutdown
Fa0/3	1	1	0	Shutdown
Fa0/4	1	1	0	Shutdown
Fa0/5	1	1	0	Shutdown

SW3
Port	Maximum MAC	Current MAC	Violations	Action
Fa0/2	1	1	0	Shutdown
Fa0/3	1	1	0	Shutdown

This confirms that each protected access port allows a maximum of one secure MAC address and currently has zero security violations.

Result: PASS

---

## 13. DHCP Snooping Verification

DHCP Snooping was verified on all three switches.
SW1
VLANs: 10, 20, 30
Status: Enabled
Trusted uplinks: Configured
Client ports: Untrusted
Option 82: Disabled

SW2
VLANs: 10, 20
Status: Enabled
Trusted uplink: Fa0/1
Client ports: Fa0/2–Fa0/5
Rate limit: 10 pps
Option 82: Disabled

SW3
VLAN: 30
Status: Enabled
Trusted uplink: Fa0/1
Client ports: Fa0/2–Fa0/3
Rate limit: 10 pps
Option 82: Disabled

DHCP Snooping successfully maintained DHCP bindings for client devices.
Result: PASS

---

## 14. Dynamic ARP Inspection Verification

Dynamic ARP Inspection was verified on all three switches.
Switch	Protected VLANs	Operation
SW1	10, 20, 30	Active
SW2	10, 20	Active
SW3	30	Active

Verification showed:
Dropped packets: 0
DHCP drops: 0
ACL drops: 0
Source MAC failures: 0
IP validation failures: 0

Normal network traffic continued to work successfully after DAI was enabled.
Result: PASS

---

## 15. Server ACL Verification

The SERVER-ACCESS extended ACL was verified on R1.
The ACL permits authorized internal networks to access the Server VLAN:
HR         → SERVER
IT         → SERVER
SALES      → SERVER
MANAGEMENT → SERVER

The final rule denies other sources:
deny ip any 192.168.40.0 0.0.0.255

The ACL was successfully processed during the server connectivity tests.
The deny rule had zero matches during testing because all tested traffic originated from authorized networks.
Result: PASS

---

## 16. Syslog Verification

SERVER1 was configured as the centralized Syslog server.
Server IP: 192.168.40.10
Protocol: UDP
Port: 514
Service: Enabled

R1 verification showed:
Syslog logging: enabled
Logging to 192.168.40.10
UDP port 514
Link: up
Messages dropped: 0

SERVER1 successfully displayed received Syslog messages from network devices.

R1 also generated interface and configuration events that appeared in the local logging buffer and were sent toward the Syslog server.

Result: PASS

---

## 17. End-to-End Test Summary

Test	Result
VLAN configuration	PASS
802.1Q trunking	PASS
Router-on-a-Stick	PASS
DHCP	PASS
HR → Server	PASS
IT → Server	PASS
SALES → Server	PASS
Management VLAN	PASS
Unauthorized SSH blocking	PASS
Authorized SSH access	PASS
SSH v2	PASS
Port Security	PASS
DHCP Snooping	PASS
Dynamic ARP Inspection	PASS
Server ACL	PASS
Syslog	PASS

---

## 18. Overall Result

The Secure Enterprise Network successfully passed the major connectivity, security, and management verification tests.

The final topology provides:
- Departmental VLAN segmentation
- Inter-VLAN routing
- Centralized DHCP
- Secure SSH management
- Management VLAN isolation
- Port Security
- DHCP Snooping
- Dynamic ARP Inspection
- BPDU Guard and PortFast
- Server network ACL protection
- Centralized Syslog monitoring
Overall Project Status: VERIFIED AND OPERATIONAL
