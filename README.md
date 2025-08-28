# Enterprise Network Redundancy with HSRP, OSPF & Centralized DHCP

**High-availability campus + branch network simulation** demonstrating VLAN segmentation, inter-VLAN routing, centralized DHCP with relay, OSPF WAN routing, HSRP gateway redundancy, and basic policy enforcement (ACLs). Built and validated in **Cisco Packet Tracer**.

---

## 🚀 Project Summary
Designed and implemented an enterprise-like network topology with:

- VLAN segmentation (HR, Finance, IT)
- Inter-VLAN routing using Layer-3 switches (SVIs)
- **HSRP** for redundant default gateways (active/standby)
- **Centralized DHCP** on L3 core with **DHCP relay** (`ip helper-address`) at branches
- **OSPF** (Area 0) for dynamic routing across HQ and branch WAN links
- **ACLs** to enforce role-based traffic policies (example: block HR → IT)
- Config exports & failover testing for demonstration

This project is interview-ready: configs, Packet Tracer file, topology image and verification steps are included.

---

## 🗺 Topology (summary)
- **HQ**
  - `L3SW-1` (Primary L3 switch) — HSRP Active, DHCP server, OSPF
  - `L3SW-2` (Standby L3 switch) — HSRP Standby, OSPF
  - `SW-ACC` (Access) — trunks to L3 core, ports for PCs
  - `R-HQ` (Router) — WAN head/router, serial to branches
- **Branches**
  - `R-BR1`, `SW-BR1`, `PCs` (branch 1)
  - `R-BR2`, `SW-BR2`, `PCs` (branch 2)

(See `images/topology.png` in this repo for the workspace screenshot.)

---

## ⚙️ Addressing & VLANs (used in this lab)
- VLAN 10 (HR) → `10.0.10.0/24` (HSRP VIP: `10.0.10.1`)  
- VLAN 20 (Finance) → `10.0.20.0/24` (HSRP VIP: `10.0.20.1`)  
- VLAN 30 (IT) → `10.0.30.0/24` (HSRP VIP: `10.0.30.1`)  
- Branch 1 LAN → `10.1.10.0/24` (default-gw `10.1.10.1`)  
- Branch 2 LAN → `10.2.10.0/24` (default-gw `10.2.10.1`)  
- Core ↔ R-HQ links + serial WANs use `/30` networks as shown in configs.

---

## 🔑 Key configuration highlights

### HSRP (example for VLAN 10 on primary)
```bash
interface Vlan10
 ip address 10.0.10.2 255.255.255.0
 standby 10 ip 10.0.10.1
 standby 10 priority 110
 standby 10 preempt
