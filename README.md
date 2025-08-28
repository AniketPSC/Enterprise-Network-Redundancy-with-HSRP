# Enterprise Network Redundancy with HSRP, OSPF & Centralized DHCP

**High-availability enterprise network simulation** demonstrating VLAN segmentation, inter-VLAN routing, centralized DHCP, OSPF WAN routing, and HSRP-based gateway redundancy.  
Implemented & tested in **Cisco Packet Tracer**.

---

## 🔍 Project Overview
This repository contains a practical, interview-ready network lab that shows how to design and operate a resilient campus + branch network:

- **VLAN segmentation** for HR, Finance, IT
- **Inter-VLAN routing** on Layer-3 switches (SVIs)
- **HSRP** for default-gateway redundancy (Active/Standby)
- **Centralized DHCP** on L3 core + **DHCP relay** (`ip helper-address`) at branch routers
- **OSPF** for dynamic routing across WAN links (Area 0)
- **ACLs** to enforce intra-site policies (example: block HR → IT)
- Config backup via **TFTP** (optional)

---

## 🖼 Topology (brief)
- HQ: two multilayer switches (`L3SW-1`, `L3SW-2`) in HSRP pair + access switch `SW-ACC` and router `R-HQ` for WAN.  
- Branches: `R-BR1` + `SW-BR1`, `R-BR2` + `SW-BR2` connected via serial WAN to `R-HQ`.  
- DHCP runs on `L3SW-1` (centralized); branches forward DHCP with `ip helper-address` to `L3SW-1` loopback.

*(See `images/topology.png` for a diagram — include a screenshot from Packet Tracer.)*

---

## 🔧 What’s included
- `hsrp_network.pkt` — Packet Tracer simulation file (open in Packet Tracer).  
- `configs/` — text copies of device configs (for quick review / copy-paste).  
- `images/` — topology screenshot & failover test screenshot.  
- `docs/report.pdf` — optional short report (one page) describing design decisions and results.

---

## ⚙️ Key configuration highlights (concise)
- **HSRP (VLAN 10)**  
  `standby 10 ip 10.0.10.1` on both L3 switches (L3SW-1 priority 110, L3SW-2 priority 100) — hosts use `10.0.10.1` as gateway.

- **Centralized DHCP (L3SW-1)**: pools for VLANs + branch networks.  
  Example pool:
  ```bash
  ip dhcp pool VLAN10-HR
   network 10.0.10.0 255.255.255.0
   default-router 10.0.10.1
   dns-server 8.8.8.8
