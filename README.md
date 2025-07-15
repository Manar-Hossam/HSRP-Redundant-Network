# Standby Redundancy Topology with HSRP Protocol

## 🔍 Overview

This project demonstrates a fault-tolerant and highly available Layer 3 network using the **HSRP (Hot Standby Router Protocol)**. It implements **redundant multilayer switches**, **VLAN segmentation**, **DHCP services**, and **EtherChannel aggregation** to simulate a professional enterprise-grade topology.

---

## 🎯 Objectives

- Ensure **gateway redundancy** using HSRP.
- Provide **IP addressing via DHCP** to all VLANs.
- Use **EtherChannel (LACP)** for link aggregation.
- Segment traffic using **multiple VLANs**.
- Enhance manageability and logging using **Syslog**, **NTP**, and **ACLs**.

---

## 🧹 Topology Components

- **2 Multilayer Switches (core1, core2)**
- **3 Access Switches**
- **4 VLANs** (10, 20, 30, 40)
- **2 DHCP Servers**
- **Multiple PCs distributed across VLANs**
- **Syslog & NTP server**
- **Redundant links with EtherChannel**

---

## 🔐 VLAN & HSRP Configuration

| VLAN | Subnet          | HSRP Virtual IP | Active Router | Standby Router |
| ---- | --------------- | --------------- | ------------- | -------------- |
| 10   | 192.168.10.0/24 | 192.168.10.10   | core1         | core2          |
| 20   | 192.168.20.0/24 | 192.168.20.20   | core2         | core1          |
| 30   | 192.168.30.0/24 | 192.168.30.30   | core1         | core2          |
| 40   | 192.168.40.0/24 | 192.168.40.40   | core2         | core1          |

---

## 🔧 Services

- **DHCP Server (192.168.10.50)**
  - Provides IPs to all VLANs via `ip helper-address` on core switches.
- **Syslog Server**
  - Collects logs from both core switches.
- **NTP Server**
  - Synchronizes time across all network devices.

---

## 🔒 Access Control List (ACL)

- Traffic from **VLAN 20 to VLAN 10** is **restricted** to improve security.
- ACLs are applied in inbound direction on **core switches’ VLAN interfaces**.

### Example:

```bash
access-list 101 deny ip 192.168.20.0 0.0.0.255 192.168.10.0 0.0.0.255
access-list 101 permit ip any any
interface vlan20
 ip access-group 101 in
```

---

## 🥺 Redundancy Testing

To simulate failover:

- Disable the active switch interface (e.g. `shutdown vlan10` on core1).
- Verify **HSRP failover** occurs and standby router becomes active.
- Use `show standby` to observe state transitions.

---

## 🏁 Conclusion

This topology offers a **resilient Layer 3 switching infrastructure**. It ensures network availability during device failures and provides dynamic IP allocation, segmented traffic, and enhanced logging for troubleshooting and monitoring.

---

