# IP & VLAN Addressing

## 1. VLAN Configuration

The enterprise network is logically segmented into separate VLANs based on department and network function.

| VLAN ID | VLAN Name | Network | Subnet Mask | Default Gateway | Purpose |
|---------|-----------|---------|-------------|-----------------|---------|
| 10 | HR | 192.168.10.0/24 | 255.255.255.0 | 192.168.10.1 | Human Resources |
| 20 | IT | 192.168.20.0/24 | 255.255.255.0 | 192.168.20.1 | IT Department |
| 30 | SALES | 192.168.30.0/24 | 255.255.255.0 | 192.168.30.1 | Sales Department |
| 40 | SERVERS | 192.168.40.0/24 | 255.255.255.0 | 192.168.40.1 | Server Network |
| 99 | MANAGEMENT | 192.168.99.0/24 | 255.255.255.0 | 192.168.99.1 | Network Device Management |

---

## 2. Router-on-a-Stick Configuration

R1 provides inter-VLAN routing using 802.1Q subinterfaces.

| R1 Interface | VLAN | IP Address | Purpose |
|--------------|------|------------|---------|
| G0/0.10 | 10 | 192.168.10.1 | HR Gateway |
| G0/0.20 | 20 | 192.168.20.1 | IT Gateway |
| G0/0.30 | 30 | 192.168.30.1 | SALES Gateway |
| G0/0.40 | 40 | 192.168.40.1 | Server Gateway |
| G0/0.99 | 99 | 192.168.99.1 | Management Gateway |

The physical interface `G0/0` operates as an 802.1Q trunk toward SW1.

---

## 3. Switch Management Addresses

Management interfaces on the switches use VLAN 99.

| Device | Management Interface | IP Address | Default Gateway |
|--------|----------------------|------------|-----------------|
| SW1 | VLAN 99 | 192.168.99.10 | 192.168.99.1 |
| SW2 | VLAN 99 | 192.168.99.11 | 192.168.99.1 |
| SW3 | VLAN 99 | 192.168.99.12 | 192.168.99.1 |

---

## 4. Server Addressing

SERVER1 is located in VLAN 40 and uses a static IP address.

| Device | VLAN | IP Address | Subnet Mask | Gateway | Purpose |
|--------|------|------------|-------------|---------|---------|
| SERVER1 | 40 | 192.168.40.10 | 255.255.255.0 | 192.168.40.1 | Server / Syslog |

SERVER1 also provides the centralized Syslog destination for the network devices.

Syslog server:

192.168.40.10
UDP Port 514

---

## 5. Client Addressing

| Device | Department | VLAN | Addressing |
|--------|------------|------|------------|
| PC1 | HR | 10 | DHCP |
| PC2 | HR | 10 | DHCP |
| PC3 | IT | 20 | DHCP |
| PC4 | IT | 20 | DHCP |
| PC5 | SALES | 30 | DHCP |
| PC6 | SALES | 30 | DHCP |

---

## 6. DHCP Addressing

Client PCs receive their IPv4 addresses dynamically from DHCP running on R1.

| Device | Department | VLAN | Addressing |
|--------|------------|------|------------|
| PC1 | HR | 10 | DHCP |
| PC2 | HR | 10 | DHCP |
| PC3 | IT | 20 | DHCP |
| PC4 | IT | 20 | DHCP |
| PC5 | SALES | 30 | DHCP |
| PC6 | SALES | 30 | DHCP |

---

## 7. Physical Port Assignments

| Port | Connection | Configuration |
|------|------------|---------------|
| G0/1 | R1 G0/0 | Trunk |
| Fa0/1 | SW2 Fa0/1 | Trunk |
| Fa0/2 | SW3 Fa0/1 | Trunk |
| Fa0/10 | SERVER1 | Access VLAN 40 |

| Port | Connection | Configuration |
|------|------------|---------------|
| Fa0/1 | SW1 Fa0/1 | Trunk |
| Fa0/2 | PC1 | Access VLAN 10 |
| Fa0/3 | PC2 | Access VLAN 10 |
| Fa0/4 | PC3 | Access VLAN 20 |
| Fa0/5 | PC4 | Access VLAN 20 |

| Port | Connection | Configuration |
|------|------------|---------------|
| Fa0/1 | SW1 Fa0/2 | Trunk |
| Fa0/2 | PC5 | Access VLAN 30 |
| Fa0/3 | PC6 | Access VLAN 30 |

---

## 8.Network Addressing Summary

HR          → 192.168.10.0/24
IT          → 192.168.20.0/24
SALES       → 192.168.30.0/24
SERVERS     → 192.168.40.0/24
MANAGEMENT  → 192.168.99.0/24
