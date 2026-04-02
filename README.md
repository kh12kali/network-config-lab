# Network Configuration Lab — DHCP & Routing

**Type:** Network Administration · Router Configuration  
**Tools:** Cisco IOS · Packet Tracer · RIPv2 · DHCP · SSH  
**Topology:** 3 routers · 2 switches · 2 PCs · 2 subnets

---

## Overview

A hands-on networking lab focused on configuring a multi-router topology  
with dynamic IP address assignment via DHCP, inter-router routing using RIPv2,  
and security hardening across all devices.

---

## Network Topology

```
[PC-A] --- [Switch 1] --- [R1] --- [ISP] --- [R2] --- [Switch 2] --- [PC-B]
```

- **R1** — DHCP relay agent, forwards requests to R2
- **R2** — DHCP server for both subnets
- **ISP** — Intermediate router connecting R1 and R2
- **PC-A and PC-B** — DHCP clients on separate subnets

---

## What I Configured

### DHCP Setup
- Configured R2 as the DHCP server for two separate LAN subnets
- Created address pools with excluded IPs for static devices (gateways, servers)
- Set default gateway, DNS server, domain name, and lease duration per pool
- Configured R1 with `ip helper-address` to relay DHCP requests from its LAN to R2
- Verified both PCs received correct IP addresses using `ipconfig /all`

### Dynamic Routing (RIPv2)
- Enabled RIPv2 on R1 and R2 for automatic route sharing
- Configured network statements for all connected subnets
- Disabled auto-summary for classless routing support
- Verified routing tables showed correct paths between all subnets

### Security Hardening
- Set encrypted enable secret password on all routers
- Configured console and VTY line passwords
- Enabled `service password-encryption` to protect all plaintext passwords in config
- Set login banners (MOTD) on all devices
- Configured `logging synchronous` on console lines for clean output
- Set up SSH access on VTY lines (`line vty 0 4`) for secure remote management

### Connectivity Verification
- Ping tests between all routers confirmed full network connectivity
- PC-A successfully pinged PC-B across both subnets
- DHCP lease confirmed on both PCs with correct gateway and DNS

---

## Key Commands Used

```bash
# DHCP Server Configuration on R2
ip dhcp excluded-address 192.168.1.1 192.168.1.10
ip dhcp pool LAN1
 network 192.168.1.0 255.255.255.0
 default-router 192.168.1.1
 dns-server 8.8.8.8
 domain-name lab.local
 lease 7

# DHCP Relay on R1
interface g0/0
 ip helper-address 10.0.0.2

# RIPv2 Configuration
router rip
 version 2
 no auto-summary
 network 192.168.1.0
 network 10.0.0.0

# Security
enable secret strongpassword
service password-encryption
line vty 0 4
 login local
 transport input ssh
```

---

## Results

| Task | Status |
|---|---|
| PC-A received DHCP address | ✅ |
| PC-B received DHCP address | ✅ |
| R1 relay agent working | ✅ |
| RIPv2 routes shared between routers | ✅ |
| Full ping connectivity across all devices | ✅ |
| Passwords encrypted in config | ✅ |
| SSH remote access configured | ✅ |

---

## Skills Demonstrated

- DHCP server and relay agent configuration
- Dynamic routing with RIPv2
- Multi-subnet network topology design
- Router security hardening
- SSH configuration for secure remote access
- Network connectivity testing and verification

---

## Files in This Repository

- `network_lab_report.docx` — Full lab report with screenshots *(uploading soon)*
- `README.md` — This file

---

*Completed as part of networking and infrastructure coursework.*
