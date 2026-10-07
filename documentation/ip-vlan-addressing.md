# IP & VLAN Addressing

## VLAN Configuration

| VLAN | Name | Network | Default Gateway | Purpose |
|------|------|---------|-----------------|---------|
| 10 | HR | 192.168.10.0/24 | 192.168.10.1 | Human Resources |
| 20 | IT | 192.168.20.0/24 | 192.168.20.1 | IT Department |
| 30 | SALES | 192.168.30.0/24 | 192.168.30.1 | Sales Department |
| 40 | SERVERS | 192.168.40.0/24 | 192.168.40.1 | Server Network |
| 99 | MANAGEMENT | 192.168.99.0/24 | 192.168.99.1 | Network Management |

## Infrastructure Addresses

| Device | Interface | IP Address | Purpose |
|--------|-----------|------------|---------|
| R1 | G0/0.10 | 192.168.10.1 | HR Gateway |
| R1 | G0/0.20 | 192.168.20.1 | IT Gateway |
| R1 | G0/0.30 | 192.168.30.1 | SALES Gateway |
| R1 | G0/0.40 | 192.168.40.1 | Server Gateway |
| R1 | G0/0.99 | 192.168.99.1 | Management Gateway |
| SW1 | VLAN 99 | 192.168.99.10 | Switch Management |
| SW2 | VLAN 99 | 192.168.99.11 | Switch Management |
| SW3 | VLAN 99 | 192.168.99.12 | Switch Management |
| SERVER1 | VLAN 40 | 192.168.40.10 | Server / Syslog |

## Client Addressing

Client PCs use DHCP.

| VLAN | Devices | Address Range |
|------|---------|---------------|
| HR | PC1, PC2 | 192.168.10.0/24 |
| IT | PC3, PC4 | 192.168.20.0/24 |
| SALES | PC5, PC6 | 192.168.30.0/24 |

## DHCP

DHCP is provided by R1 for:

- VLAN 10 — HR
- VLAN 20 — IT
- VLAN 30 — SALES

The server VLAN uses a static IP address.