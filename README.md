# 🌐 Enterprise Network Redundancy with HSRP

## 📌 Overview
This project demonstrates the design and implementation of a **resilient Layer 3 enterprise network** with:
- **VLAN segmentation** for logical isolation
- **Inter-VLAN routing** using Layer 3 switches
- **Hot Standby Router Protocol (HSRP)** for gateway redundancy and high availability

The goal was to ensure **seamless failover** in case of device/link failure, with minimal packet loss and uninterrupted user experience.

Built and tested in **Cisco Packet Tracer**.

---

## 🖥️ Network Topology
![Topology](images/topology.png)

- **L3SW-1**: Primary Layer 3 switch, HSRP Active for VLAN 10
- **L3SW-2**: Secondary Layer 3 switch, HSRP Standby for VLAN 10
- **Access Switch**: Provides connectivity to end devices
- **PCs in VLAN 10**: Connected via redundant default gateway

---

## ⚙️ Key Features
✔️ VLANs for segmentation and traffic isolation  
✔️ Inter-VLAN Routing on L3 switches  
✔️ HSRP for **default gateway redundancy** (`10.0.10.1`)  
✔️ **Preemption + Priority** to force active/standby role assignment  
✔️ High Availability testing with real-time failover  

---

## 🔑 Sample HSRP Configuration
```bash
interface Vlan10
 ip address 10.0.10.2 255.255.255.0
 standby 10 ip 10.0.10.1
 standby 10 priority 110
 standby 10 preempt

