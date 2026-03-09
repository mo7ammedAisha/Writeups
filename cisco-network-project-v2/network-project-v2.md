# Cisco Packet Tracer Company Network Project — Version 2

**Author:** Mohammed Abuaisha \ mabuaisha1@smail.ucas.edu.ps

## Introduction

This document is **Version 2** of the Company Network Project built in Cisco Packet Tracer.

Version 1 worked correctly but had several design limitations. Version 2 fixes those problems and adds new features to make the network more realistic, more efficient, and more educational.

**What is new in Version 2:**

- Only **4 routers** instead of 5 — more efficient topology
- Each department switch now serves **2 VLANs** using proper **router-on-a-stick** (1 trunk link, 1 physical cable, multiple VLANs)
- Each VLAN/department now has **at least 3 PCs** (not 2 as before)
- A **5th switch (Switch5)** cascades from Switch1 to extend the HR and IT departments
- **Mixed routing protocols**: RIP v2, OSPF, Static Routes, and a Default Route — all in one topology
- **Redistribution** between OSPF and RIP at the Router2 boundary
- No accidental `redistribute static` commands — every redistribute is intentional

The routers are **Cisco 2911**. The switches are **Cisco 2960**.
Follow each step carefully. You will be able to finish the whole project using only this guide.

> **Note:** Read Section 0 first. It shows you how to add devices and cables in Packet Tracer. Do not skip it.

---

## V1 vs V2 Comparison Table

| Feature | Version 1 | Version 2 |
|---|---|---|
| Number of routers | 5 routers | **4 routers** — more efficient |
| VLANs per switch | Each switch had only 1 VLAN — missed the real purpose of VLANs | **Each main switch has 2 VLANs** — uses router-on-a-stick properly |
| PCs per department | 2 PCs per department | **At least 3 PCs** per department VLAN |
| 5th switch | Switch5 was used for the Server Room | **Switch5 cascades from Switch1** — extends HR and IT departments |
| Server/Management switches | Server Room on a separate switch connected to Router5 | Switch3 for Server Room (VLAN 50), Switch4 for Management (VLAN 60) — both connected to Router4 and Router3 |
| Routing protocols | RIP v2 only (+ static + default in bonus sections) | **RIP v2 + OSPF + Static + Default** all built in from the start |
| OSPF | Not used | **OSPF area 0** on Router4; redistribution at Router2 boundary |
| Redistribution | Accidentally in Router5 config | **Only where needed**: Router2 redistributes between OSPF and RIP |
| WAN topology | Linear chain: R1→R3→R5←R4←R2 | **Hub-and-spoke via Router3**: R1↔R3↔R2, R2↔R4 |
| Static routes | Router3 only (bonus) | **Router3 has 1 explicit static route** alongside RIP — demonstration |
| Default route | Router4 (bonus) | **Router4 has default route** — built into the main design |

---

## Table of Contents

0. [Setting Up the Workspace](#0-setting-up-the-workspace)
1. [Network Topology Overview](#1-network-topology-overview)
2. [IP Addressing Scheme](#2-ip-addressing-scheme)
3. [VLAN Plan](#3-vlan-plan)
4. [Cable Connection Table](#4-cable-connection-table)
5. [Router Configuration](#5-router-configuration)
6. [Switch Configuration](#6-switch-configuration)
7. [DHCP Server Configuration](#7-dhcp-server-configuration)
8. [DNS Server Configuration](#8-dns-server-configuration)
9. [Web Server Configuration](#9-web-server-configuration)
10. [Mail Server Configuration](#10-mail-server-configuration)
11. [PC Configuration](#11-pc-configuration)
12. [Testing and Verification](#12-testing-and-verification)

---

## 0. Setting Up the Workspace

This section tells you how to add every device and cable to Packet Tracer.
Do these steps before you type any commands.

### 0.1 How to Open Cisco Packet Tracer

1. Open Cisco Packet Tracer on your computer.
2. Click **File → New** to open a blank workspace.
3. You will see a big empty area in the center. This is where you build the network.
4. At the bottom-left, you will see a panel with small icons. This is the **Device Selection** panel.

---

### 0.2 How to Add a Router (Cisco 2911)

The Cisco 2911 router has 3 built-in GigabitEthernet ports:
- `GigabitEthernet0/0`
- `GigabitEthernet0/1`
- `GigabitEthernet0/2`

**Steps to add one router:**

1. Look at the bottom-left panel.
2. Click the icon that looks like a router. This is **"Network Devices"**.
3. Click on **"Routers"** (the subcategory that appears).
4. A list of router models appears.
5. Find and click on **"2911"**.
6. Move your mouse to the workspace area.
7. Click in the workspace to place the router.
8. The router appears on the screen.

**Repeat 4 times. You need 4 routers total.**

**How to rename a router:**

1. Click on the router icon.
2. A window opens. Click the **"Config"** tab.
3. At the top, find the field that says **"Display Name"**.
4. Type the new name: `Router1`, `Router2`, `Router3`, or `Router4`.
5. Close the window.

---

### 0.3 How to Add a Switch (Cisco 2960)

The Cisco 2960 switch has:
- 24 FastEthernet ports: `FastEthernet0/1` to `FastEthernet0/24`
- 2 GigabitEthernet uplink ports: `GigabitEthernet0/1` and `GigabitEthernet0/2`

**Steps to add one switch:**

1. Look at the bottom-left panel.
2. Click **"Network Devices"**.
3. Click on **"Switches"**.
4. Find and click **"2960"**.
5. Click in the workspace to place the switch.

**Repeat 5 times. You need 5 switches total.**

**Rename them:** Switch1, Switch2, Switch3, Switch4, Switch5.
Use the same steps as renaming a router (click → Config tab → Display Name).

---

### 0.4 How to Add Servers (Server-PT)

You need 4 servers: DHCP-Server, DNS-Server, Web-Server, Mail-Server.

**Steps to add one server:**

1. Look at the bottom-left panel.
2. Click **"End Devices"** (the icon that looks like a computer).
3. Find **"Server"** or **"Server-PT"** in the list.
4. Click on it.
5. Click in the workspace to place the server.

**Repeat 4 times.**

**Rename them:**
- DHCP-Server
- DNS-Server
- Web-Server
- Mail-Server

---

### 0.5 How to Add PCs (PC-PT)

You need 18 PCs total.

**Steps to add one PC:**

1. Look at the bottom-left panel.
2. Click **"End Devices"**.
3. Find **"PC"** or **"PC-PT"** in the list.
4. Click on it.
5. Click in the workspace to place the PC.

**Repeat 18 times.**

**Rename them:**
- HR-PC1, HR-PC2, HR-PC3 (on Switch1 — VLAN 10)
- IT-PC1, IT-PC2, IT-PC3 (on Switch1 — VLAN 20)
- Finance-PC1, Finance-PC2, Finance-PC3 (on Switch2 — VLAN 30)
- Sales-PC1, Sales-PC2, Sales-PC3 (on Switch2 — VLAN 40)
- HR-PC4, HR-PC5 (on Switch5 cascade — VLAN 10)
- IT-PC4 (on Switch5 cascade — VLAN 20)
- Admin-PC1, Admin-PC2 (on Switch4 — VLAN 60)

---

### 0.6 How to Connect Devices with Cables

1. Look at the bottom-left panel.
2. Click the **lightning bolt icon**. This is **"Connections"**.
3. A list of cable types appears.

**Which cable to use:**

| Connection Type           | Cable Type                  |
|---------------------------|-----------------------------|
| Router to Router          | Copper Cross-Over           |
| Router to Switch          | Copper Straight-Through     |
| Switch to PC              | Copper Straight-Through     |
| Switch to Server          | Copper Straight-Through     |
| Switch to Switch (trunk)  | Copper Cross-Over           |

**Steps to connect two devices:**

1. Click the cable type you need.
2. Click the **first device** (e.g., Router1).
3. A list of interfaces appears. Click the correct interface (e.g., `GigabitEthernet0/0`).
4. Now click the **second device** (e.g., Switch1).
5. A list of interfaces appears again. Click the correct interface (e.g., `FastEthernet0/1`).
6. A line appears between the two devices. This is the cable.

**Check the cable dots:**
- Green dot = link is up (good)
- Red dot = link is down (bad — check the interface configuration later)

> **Tip:** The inter-switch trunk cable between Switch1 and Switch5 uses **Cross-Over**, because it is a switch-to-switch connection.

---

## 1. Network Topology Overview

### About the Company

The company has four departments, a server room, and a management network:

| Department  | VLAN | Switch          | Router        |
|-------------|------|-----------------|---------------|
| HR          | 10   | Switch1         | Router1       |
| IT          | 20   | Switch1         | Router1       |
| Finance     | 30   | Switch2         | Router2       |
| Sales       | 40   | Switch2         | Router2       |
| Server Room | 50   | Switch3         | Router4       |
| Management  | 60   | Switch4         | Router3       |

**Key V2 improvement:** In V1, each department had its own separate switch and a separate physical cable to the router. In V2, two departments share one switch and one trunk cable to the router. This is **router-on-a-stick**.

Switch5 cascades from Switch1. It extends the HR and IT departments with additional PCs.

### Device List

| Device  | Model      | Count | Purpose                                                      |
|---------|------------|-------|--------------------------------------------------------------|
| Routers | Cisco 2911 | 4     | Connect networks — RIP v2, OSPF, Static, Default, Redistribution |
| Switches| Cisco 2960 | 5     | Connect PCs and servers within VLANs                         |
| Servers | Server-PT  | 4     | DHCP, DNS, Web, Mail                                         |
| PCs     | PC-PT      | 18    | End user devices (3+ per department)                         |

### Routing Protocol Summary

| Router  | Protocol(s)                           | Role                                           |
|---------|---------------------------------------|------------------------------------------------|
| Router1 | RIP v2                                | Gateway for HR (VLAN 10) and IT (VLAN 20)      |
| Router2 | RIP v2 **+ OSPF area 0**              | Gateway for Finance + Sales; redistribution boundary |
| Router3 | RIP v2 + **1 static route**           | Transit + Management VLAN gateway              |
| Router4 | **OSPF area 0** + Default Route       | Gateway for Server Room (VLAN 50)              |

### Topology Diagram

```
                  +----------+     10.0.3.0/30     +----------+
                  | Router1  |<------------------->| Router2  |
                  | (RIP v2) |                     |(RIP+OSPF)|
                  +----+-----+                     +----+-----+
                       |                                |
               10.0.0.0/30                        10.0.1.0/30
                       |                                |
                  +----+-----+                     +----+-----+
                  | Router3  |                     | Router4  |
                  |(RIP+Stat)|                     |(OSPF+Def)|
                  +----+-----+                     +-----+----+
                       |                                 |
               (connects via Router2 side)          10.0.2.0/30
                                                         |
                                                    +----+-----+
                                                    | Router2  |
                                                    |(R2 is the|
                                                    | RIP/OSPF |
                                                    | boundary)|
                                                    +----------+

WAN Links:
  Router1 <--> Router3  :  10.0.0.0/30
  Router2 <--> Router3  :  10.0.1.0/30
  Router1 <--> Router2  :  10.0.3.0/30  (direct link)
  Router2 <--> Router4  :  10.0.2.0/30

LAN Links:
  Router1 Gig0/0 <--> Switch1  (trunk: VLAN 10 + VLAN 20)
  Router2 Gig0/0 <--> Switch2  (trunk: VLAN 30 + VLAN 40)
  Router3 Gig0/2 <--> Switch4  (trunk: VLAN 60)
  Router4 Gig0/0 <--> Switch3  (trunk: VLAN 50)
  Switch1 Fa0/24 <--> Switch5  (inter-switch trunk cascade)
```

**Full ASCII topology:**

```
[HR-PC1][HR-PC2][HR-PC3]  [IT-PC1][IT-PC2][IT-PC3]
    |        |       |         |       |       |
  Fa0/2   Fa0/3  Fa0/4      Fa0/5  Fa0/6   Fa0/7
    +-------+-------+---------+-------+-------+
                    |  Switch1  |
                    |  VLAN10+20|
                    +-----+-----+
                    Fa0/1 |      | Fa0/24 (trunk to Switch5)
                          |      +------------------+
              Gig0/0 trunk|                         |
                    +-----+-----+             +-----+-----+
                    | Router1   |             |  Switch5  |
                    | RIP v2    |             | VLAN10+20 |
                    +-----+-----+             +-+---+---+-+
                   /      |                    |     |   |
          Gig0/1  /    Gig0/2                Fa0/2 Fa0/3 Fa0/4
   10.0.0.0/30   /  10.0.3.0/30           HR-PC4 HR-PC5 IT-PC4
                /        \
     +---------+--+    +--+---------+
     |  Router3   |    |  Router2   |
     | RIP+Static |    | RIP + OSPF |
     +-----+------+    +----+---+---+
     Gig0/2|         Gig0/0 |   | Gig0/2
    (VLAN60|          trunk |   | 10.0.2.0/30
    trunk) |                |   |
  +--------+------+  +------+---+----+    +----------+
  |   Switch4     |  |   Switch2     |    | Router4  |
  |   VLAN 60     |  |  VLAN 30+40   |    | OSPF+Def |
  +---+-------+---+  +-+--+-+--+-+--+    +----+-----+
    Fa0/2   Fa0/3      |  |  |  |  |          | Gig0/0 trunk
  Admin-PC1 Admin-PC2  |  |  |  |  |     +----+-----+
                       |  |  |  |  |     |  Switch3  |
                 Fa0/2 |  |Fa0/4  |Fa0/6 |  VLAN 50  |
           Finance-PC1 |Finance-PC3 |    +-+--+-+--+--+
                       |      Sales-PC2    |  |  |  |
                  Fa0/3|     Fa0/5| Fa0/7 Fa0/2 | Fa0/4 Fa0/5
              Finance-PC2  Sales-PC1 Sales-PC3 |DHCP  Web  Mail
                                            Fa0/3
                                            DNS
```

---

## 2. IP Addressing Scheme

### VLAN and Subnet Table

| VLAN ID | VLAN Name   | Subnet            | Gateway        | DHCP Range              |
|---------|-------------|-------------------|----------------|-------------------------|
| 10      | HR          | 192.168.10.0/24   | 192.168.10.1   | 192.168.10.100 – .200   |
| 20      | IT          | 192.168.20.0/24   | 192.168.20.1   | 192.168.20.100 – .200   |
| 30      | Finance     | 192.168.30.0/24   | 192.168.30.1   | 192.168.30.100 – .200   |
| 40      | Sales       | 192.168.40.0/24   | 192.168.40.1   | 192.168.40.100 – .200   |
| 50      | Servers     | 192.168.50.0/24   | 192.168.50.1   | Static only (no DHCP)   |
| 60      | Management  | 192.168.60.0/24   | 192.168.60.1   | Static only (no DHCP)   |
| 1       | Native      | —                 | —              | Default/management VLAN |

### WAN Point-to-Point Links (/30 subnets)

| WAN Link  | Network         | Router A End   | Router B End   |
|-----------|-----------------|----------------|----------------|
| R1 ↔ R3   | 10.0.0.0/30     | R1 = 10.0.0.1  | R3 = 10.0.0.2  |
| R2 ↔ R3   | 10.0.1.0/30     | R2 = 10.0.1.1  | R3 = 10.0.1.2  |
| R1 ↔ R2   | 10.0.3.0/30     | R1 = 10.0.3.1  | R2 = 10.0.3.2  |
| R2 ↔ R4   | 10.0.2.0/30     | R2 = 10.0.2.1  | R4 = 10.0.2.2  |

### Server Static IP Addresses

| Server      | IP Address      | VLAN | Service     |
|-------------|-----------------|------|-------------|
| DHCP-Server | 192.168.50.10   | 50   | DHCP        |
| DNS-Server  | 192.168.50.20   | 50   | DNS         |
| Web-Server  | 192.168.50.30   | 50   | HTTP        |
| Mail-Server | 192.168.50.40   | 50   | SMTP / POP3 |

### Router LAN Sub-Interface IPs

> **Why sub-interfaces?** Router1 uses one physical interface (`GigabitEthernet0/0`) to carry traffic for VLAN 10 and VLAN 20. The sub-interface number (`.10`, `.20`) matches the VLAN ID. This is called **router-on-a-stick**. One cable handles multiple VLANs.

| Router  | Interface                | VLAN | IP Address        |
|---------|--------------------------|------|-------------------|
| R1      | GigabitEthernet0/0.10    | 10   | 192.168.10.1/24   |
| R1      | GigabitEthernet0/0.20    | 20   | 192.168.20.1/24   |
| R2      | GigabitEthernet0/0.30    | 30   | 192.168.30.1/24   |
| R2      | GigabitEthernet0/0.40    | 40   | 192.168.40.1/24   |
| R3      | GigabitEthernet0/2.60    | 60   | 192.168.60.1/24   |
| R4      | GigabitEthernet0/0.50    | 50   | 192.168.50.1/24   |

### Full Interface Map

```
Router1 : Gig0/0 -> Switch1 (trunk: VLAN 10 + 20)  [LAN]
          Gig0/1 -> Router3 (WAN 10.0.0.0/30)
          Gig0/2 -> Router2 (WAN 10.0.3.0/30)

Router2 : Gig0/0 -> Switch2 (trunk: VLAN 30 + 40)  [LAN]
          Gig0/1 -> Router3 (WAN 10.0.1.0/30)
          Gig0/2 -> Router4 (WAN 10.0.2.0/30)

Router3 : Gig0/0 -> Router1 (WAN 10.0.0.0/30)
          Gig0/1 -> Router2 (WAN 10.0.1.0/30)
          Gig0/2 -> Switch4 (trunk: VLAN 60)         [LAN]

Router4 : Gig0/0 -> Switch3 (trunk: VLAN 50)        [LAN]
          Gig0/1 -> Router2 (WAN 10.0.2.0/30)
```

---

## 3. VLAN Plan

### VLAN List

| VLAN ID | Name       | Description                    |
|---------|------------|--------------------------------|
| 1       | Native     | Default VLAN (management)      |
| 10      | HR         | Human Resources                |
| 20      | IT         | Information Technology         |
| 30      | Finance    | Finance Department             |
| 40      | Sales      | Sales Department               |
| 50      | Servers    | Server Room (DHCP, DNS, etc.)  |
| 60      | Management | Network Admin / Management     |

### Port Assignment per Switch

#### Switch1 — HR (VLAN 10) + IT (VLAN 20)

> **V2 change:** In V1, HR had its own switch and IT had its own switch. In V2, both departments share Switch1 via a single trunk link to Router1. This demonstrates router-on-a-stick with 2 VLANs.

| Port    | Mode   | VLAN     | Connected To                    |
|---------|--------|----------|---------------------------------|
| Fa0/1   | Trunk  | All      | Router1 GigabitEthernet0/0      |
| Fa0/2   | Access | 10       | HR-PC1                          |
| Fa0/3   | Access | 10       | HR-PC2                          |
| Fa0/4   | Access | 10       | HR-PC3                          |
| Fa0/5   | Access | 20       | IT-PC1                          |
| Fa0/6   | Access | 20       | IT-PC2                          |
| Fa0/7   | Access | 20       | IT-PC3                          |
| Fa0/24  | Trunk  | All      | Switch5 Fa0/1 (cascade uplink)  |

#### Switch2 — Finance (VLAN 30) + Sales (VLAN 40)

> **V2 change:** Same improvement as Switch1. Finance and Sales share one switch via a single trunk link to Router2.

| Port    | Mode   | VLAN     | Connected To                    |
|---------|--------|----------|---------------------------------|
| Fa0/1   | Trunk  | All      | Router2 GigabitEthernet0/0      |
| Fa0/2   | Access | 30       | Finance-PC1                     |
| Fa0/3   | Access | 30       | Finance-PC2                     |
| Fa0/4   | Access | 30       | Finance-PC3                     |
| Fa0/5   | Access | 40       | Sales-PC1                       |
| Fa0/6   | Access | 40       | Sales-PC2                       |
| Fa0/7   | Access | 40       | Sales-PC3                       |

#### Switch3 — Server Room (VLAN 50)

| Port    | Mode   | VLAN     | Connected To                    |
|---------|--------|----------|---------------------------------|
| Fa0/1   | Trunk  | All      | Router4 GigabitEthernet0/0      |
| Fa0/2   | Access | 50       | DHCP-Server                     |
| Fa0/3   | Access | 50       | DNS-Server                      |
| Fa0/4   | Access | 50       | Web-Server                      |
| Fa0/5   | Access | 50       | Mail-Server                     |

#### Switch4 — Management (VLAN 60)

| Port    | Mode   | VLAN     | Connected To                    |
|---------|--------|----------|---------------------------------|
| Fa0/1   | Trunk  | All      | Router3 GigabitEthernet0/2      |
| Fa0/2   | Access | 60       | Admin-PC1                       |
| Fa0/3   | Access | 60       | Admin-PC2                       |

#### Switch5 — HR + IT Extension (VLAN 10 + VLAN 20) — Cascade from Switch1

> **V2 addition:** Switch5 is a new 5th switch that cascades from Switch1 via a trunk link. It extends the HR and IT departments with additional PCs. The trunk between Switch1 (Fa0/24) and Switch5 (Fa0/1) carries both VLAN 10 and VLAN 20.

| Port    | Mode   | VLAN     | Connected To                    |
|---------|--------|----------|---------------------------------|
| Fa0/1   | Trunk  | All      | Switch1 Fa0/24 (cascade uplink) |
| Fa0/2   | Access | 10       | HR-PC4                          |
| Fa0/3   | Access | 10       | HR-PC5                          |
| Fa0/4   | Access | 20       | IT-PC4                          |

---

## 4. Cable Connection Table

Use this table to connect all devices.
Connect every cable before you start typing commands.

| Cable # | From Device | From Interface      | To Device   | To Interface           | Cable Type        |
|---------|-------------|---------------------|-------------|------------------------|-------------------|
| 1       | Router1     | GigabitEthernet0/0  | Switch1     | FastEthernet0/1        | Straight-Through  |
| 2       | Router1     | GigabitEthernet0/1  | Router3     | GigabitEthernet0/0     | Cross-Over        |
| 3       | Router1     | GigabitEthernet0/2  | Router2     | GigabitEthernet0/2     | Cross-Over        |
| 4       | Router2     | GigabitEthernet0/0  | Switch2     | FastEthernet0/1        | Straight-Through  |
| 5       | Router2     | GigabitEthernet0/1  | Router3     | GigabitEthernet0/1     | Cross-Over        |
| 6       | Router2     | GigabitEthernet0/2  | Router4     | GigabitEthernet0/1     | Cross-Over        |
| 7       | Router3     | GigabitEthernet0/2  | Switch4     | FastEthernet0/1        | Straight-Through  |
| 8       | Router4     | GigabitEthernet0/0  | Switch3     | FastEthernet0/1        | Straight-Through  |
| 9       | Switch1     | FastEthernet0/24    | Switch5     | FastEthernet0/1        | Cross-Over        |
| 10      | Switch1     | FastEthernet0/2     | HR-PC1      | FastEthernet0          | Straight-Through  |
| 11      | Switch1     | FastEthernet0/3     | HR-PC2      | FastEthernet0          | Straight-Through  |
| 12      | Switch1     | FastEthernet0/4     | HR-PC3      | FastEthernet0          | Straight-Through  |
| 13      | Switch1     | FastEthernet0/5     | IT-PC1      | FastEthernet0          | Straight-Through  |
| 14      | Switch1     | FastEthernet0/6     | IT-PC2      | FastEthernet0          | Straight-Through  |
| 15      | Switch1     | FastEthernet0/7     | IT-PC3      | FastEthernet0          | Straight-Through  |
| 16      | Switch2     | FastEthernet0/2     | Finance-PC1 | FastEthernet0          | Straight-Through  |
| 17      | Switch2     | FastEthernet0/3     | Finance-PC2 | FastEthernet0          | Straight-Through  |
| 18      | Switch2     | FastEthernet0/4     | Finance-PC3 | FastEthernet0          | Straight-Through  |
| 19      | Switch2     | FastEthernet0/5     | Sales-PC1   | FastEthernet0          | Straight-Through  |
| 20      | Switch2     | FastEthernet0/6     | Sales-PC2   | FastEthernet0          | Straight-Through  |
| 21      | Switch2     | FastEthernet0/7     | Sales-PC3   | FastEthernet0          | Straight-Through  |
| 22      | Switch3     | FastEthernet0/2     | DHCP-Server | FastEthernet0          | Straight-Through  |
| 23      | Switch3     | FastEthernet0/3     | DNS-Server  | FastEthernet0          | Straight-Through  |
| 24      | Switch3     | FastEthernet0/4     | Web-Server  | FastEthernet0          | Straight-Through  |
| 25      | Switch3     | FastEthernet0/5     | Mail-Server | FastEthernet0          | Straight-Through  |
| 26      | Switch4     | FastEthernet0/2     | Admin-PC1   | FastEthernet0          | Straight-Through  |
| 27      | Switch4     | FastEthernet0/3     | Admin-PC2   | FastEthernet0          | Straight-Through  |
| 28      | Switch5     | FastEthernet0/2     | HR-PC4      | FastEthernet0          | Straight-Through  |
| 29      | Switch5     | FastEthernet0/3     | HR-PC5      | FastEthernet0          | Straight-Through  |
| 30      | Switch5     | FastEthernet0/4     | IT-PC4      | FastEthernet0          | Straight-Through  |

> **Note for PC-PT and Server-PT:** Their network interface is named `FastEthernet0` (no slash number).

---

## 5. Router Configuration

### Important Concepts for V2

#### What is Router-on-a-Stick?

One physical cable connects the router to a switch trunk port.
The router uses **sub-interfaces** (like `GigabitEthernet0/0.10`) to handle each VLAN separately.
Both VLANs share the same physical cable — this is the key improvement over V1.

#### What is DHCP Relay (ip helper-address)?

When a PC sends a DHCP request, it sends a broadcast.
Broadcasts do not cross routers.
`ip helper-address` tells the router to forward the DHCP broadcast to the DHCP server at `192.168.50.10`.

#### Routing Protocols Used

- **Router1 and Router3**: RIP version 2 — dynamic routing, learns routes automatically
- **Router2**: RIP v2 **and** OSPF area 0 — the redistribution boundary
- **Router3**: RIP v2 — also has one **static route** for demonstration
- **Router4**: OSPF area 0 **and** a **default route** — for the Server Room

#### What is Redistribution?

Router2 sits at the boundary between the RIP domain (Router1, Router3) and the OSPF domain (Router4).
- `redistribute ospf 1 metric 2` inside the RIP config: OSPF routes (VLAN 50) flow into RIP, so Router1 and Router3 learn them.
- `redistribute rip metric 20 subnets` inside the OSPF config: RIP routes (VLAN 10, 20, 30, 40, 60) flow into OSPF, so Router4 learns them.

#### What is a Default Route?

`ip route 0.0.0.0 0.0.0.0 <next-hop>` is a catch-all route.
Router4 uses this so that any traffic destined for a network Router4 doesn't know specifically will be sent to Router2 (next hop). Router2 knows all routes and handles the forwarding.

---

### Router 1 Configuration

**Role:** Gateway for HR (VLAN 10) and IT (VLAN 20) — RIP v2
**Interfaces:**
- `GigabitEthernet0/0` → Switch1 (trunk for VLAN 10 + VLAN 20)
- `GigabitEthernet0/1` → Router3 (WAN)
- `GigabitEthernet0/2` → Router2 (WAN direct link)

**Step 1:** Open the CLI. Click Router1 → **CLI** tab → press Enter.

**Step 2:** Set hostname and configure WAN interfaces.

```
Router> enable
Router# configure terminal
Router(config)# hostname Router1

Router1(config)# interface GigabitEthernet0/1
Router1(config-if)# ip address 10.0.0.1 255.255.255.252
Router1(config-if)# no shutdown
Router1(config-if)# exit

Router1(config)# interface GigabitEthernet0/2
Router1(config-if)# ip address 10.0.3.1 255.255.255.252
Router1(config-if)# no shutdown
Router1(config-if)# exit
```

**Step 3:** Configure the trunk LAN interface for Switch1 (VLAN 10 + VLAN 20).

```
Router1(config)# interface GigabitEthernet0/0
Router1(config-if)# no shutdown
Router1(config-if)# exit

Router1(config)# interface GigabitEthernet0/0.10
Router1(config-subif)# encapsulation dot1Q 10
Router1(config-subif)# ip address 192.168.10.1 255.255.255.0
Router1(config-subif)# ip helper-address 192.168.50.10
Router1(config-subif)# exit

Router1(config)# interface GigabitEthernet0/0.20
Router1(config-subif)# encapsulation dot1Q 20
Router1(config-subif)# ip address 192.168.20.1 255.255.255.0
Router1(config-subif)# ip helper-address 192.168.50.10
Router1(config-subif)# exit
```

**Step 4:** Configure RIP version 2.

```
Router1(config)# router rip
Router1(config-router)# version 2
Router1(config-router)# no auto-summary
Router1(config-router)# network 10.0.0.0
Router1(config-router)# network 192.168.10.0
Router1(config-router)# network 192.168.20.0
Router1(config-router)# exit
```

> **Note:** `network 10.0.0.0` is a Class A network statement. With `no auto-summary`, RIP activates on **all** interfaces whose IP address falls in the 10.x.x.x range. On Router1 this means both the WAN link to Router3 (10.0.0.1/30) **and** the direct WAN link to Router2 (10.0.3.1/30). A single `network 10.0.0.0` statement is enough — you do not need to add a separate `network 10.0.3.0` statement.

**Step 5:** Save.

```
Router1(config)# end
Router1# write memory
```

---

### Router 2 Configuration

**Role:** Gateway for Finance (VLAN 30) and Sales (VLAN 40) — **RIP v2 + OSPF Redistribution Boundary**
**Interfaces:**
- `GigabitEthernet0/0` → Switch2 (trunk for VLAN 30 + VLAN 40)
- `GigabitEthernet0/1` → Router3 (WAN)
- `GigabitEthernet0/2` → Router4 (WAN)

**Step 1:** Open the CLI. Click Router2 → CLI tab → press Enter.

**Step 2:** Set hostname and configure WAN interfaces.

```
Router> enable
Router# configure terminal
Router(config)# hostname Router2

Router2(config)# interface GigabitEthernet0/1
Router2(config-if)# ip address 10.0.1.1 255.255.255.252
Router2(config-if)# no shutdown
Router2(config-if)# exit

Router2(config)# interface GigabitEthernet0/2
Router2(config-if)# ip address 10.0.2.1 255.255.255.252
Router2(config-if)# no shutdown
Router2(config-if)# exit
```

**Step 3:** Configure the trunk LAN interface for Switch2 (VLAN 30 + VLAN 40).

```
Router2(config)# interface GigabitEthernet0/0
Router2(config-if)# no shutdown
Router2(config-if)# exit

Router2(config)# interface GigabitEthernet0/0.30
Router2(config-subif)# encapsulation dot1Q 30
Router2(config-subif)# ip address 192.168.30.1 255.255.255.0
Router2(config-subif)# ip helper-address 192.168.50.10
Router2(config-subif)# exit

Router2(config)# interface GigabitEthernet0/0.40
Router2(config-subif)# encapsulation dot1Q 40
Router2(config-subif)# ip address 192.168.40.1 255.255.255.0
Router2(config-subif)# ip helper-address 192.168.50.10
Router2(config-subif)# exit
```

**Step 4:** Configure RIP version 2 with OSPF redistribution.

```
Router2(config)# router rip
Router2(config-router)# version 2
Router2(config-router)# no auto-summary
Router2(config-router)# network 10.0.0.0
Router2(config-router)# network 192.168.30.0
Router2(config-router)# network 192.168.40.0
Router2(config-router)# redistribute ospf 1 metric 2
Router2(config-router)# exit
```

> **Explanation of `redistribute ospf 1 metric 2`:** This injects routes that Router2 learned via OSPF (such as VLAN 50 — Server Room) into the RIP domain. Now Router1 and Router3 will learn about VLAN 50 through RIP. The `metric 2` means these redistributed routes appear as 2 hops away in RIP.

**Step 5:** Configure OSPF area 0 with RIP redistribution.

```
Router2(config)# router ospf 1
Router2(config-router)# network 10.0.2.0 0.0.0.3 area 0
Router2(config-router)# redistribute rip metric 20 subnets
Router2(config-router)# exit
```

> **Explanation of `redistribute rip metric 20 subnets`:** This injects routes learned from RIP (VLAN 10, 20, 30, 40, 60 and the WAN links) into the OSPF domain. Now Router4 will learn all department VLANs via OSPF from Router2. The `metric 20` is the OSPF cost assigned to redistributed routes. The `subnets` keyword is required to redistribute classless (non-classful) routes.

**Step 6:** Save.

```
Router2(config)# end
Router2# write memory
```

---

### Router 3 Configuration

**Role:** Transit router connecting Router1 and Router2 + Management VLAN gateway — **RIP v2 + 1 Static Route**
**Interfaces:**
- `GigabitEthernet0/0` → Router1 (WAN)
- `GigabitEthernet0/1` → Router2 (WAN)
- `GigabitEthernet0/2` → Switch4 (trunk for VLAN 60 Management)

**Step 1:** Open the CLI. Click Router3 → CLI tab → press Enter.

**Step 2:** Set hostname and configure WAN interfaces.

```
Router> enable
Router# configure terminal
Router(config)# hostname Router3

Router3(config)# interface GigabitEthernet0/0
Router3(config-if)# ip address 10.0.0.2 255.255.255.252
Router3(config-if)# no shutdown
Router3(config-if)# exit

Router3(config)# interface GigabitEthernet0/1
Router3(config-if)# ip address 10.0.1.2 255.255.255.252
Router3(config-if)# no shutdown
Router3(config-if)# exit
```

**Step 3:** Configure the trunk LAN interface for Switch4 (VLAN 60 Management).

```
Router3(config)# interface GigabitEthernet0/2
Router3(config-if)# no shutdown
Router3(config-if)# exit

Router3(config)# interface GigabitEthernet0/2.60
Router3(config-subif)# encapsulation dot1Q 60
Router3(config-subif)# ip address 192.168.60.1 255.255.255.0
Router3(config-subif)# exit
```

**Step 4:** Configure RIP version 2.

```
Router3(config)# router rip
Router3(config-router)# version 2
Router3(config-router)# no auto-summary
Router3(config-router)# network 10.0.0.0
Router3(config-router)# network 192.168.60.0
Router3(config-router)# exit
```

> **Note:** `network 10.0.0.0` covers both WAN interfaces (10.0.0.2 to Router1 and 10.0.1.2 to Router2). Router3 forms RIP adjacencies with Router1 and Router2. It will learn all VLAN routes from them (including VLAN 50 redistributed from OSPF by Router2) and advertise VLAN 60.

**Step 5:** Add the static route (demonstration of static routing alongside RIP).

```
Router3(config)# ip route 10.0.2.0 255.255.255.252 10.0.1.1
```

> **Explanation:** This static route explicitly tells Router3 to reach the Router2–Router4 WAN link (10.0.2.0/30) via Router2 (10.0.1.1). Router3 also learns this subnet dynamically via RIP from Router2. The static route (administrative distance = 1) takes precedence over the RIP route (AD = 120). This demonstrates how a static route can override a dynamic routing protocol for a specific subnet.

**Step 6:** Save.

```
Router3(config)# end
Router3# write memory
```

---

### Router 4 Configuration

**Role:** Gateway for Server Room (VLAN 50) — **OSPF area 0 + Default Route**
**Interfaces:**
- `GigabitEthernet0/0` → Switch3 (trunk for VLAN 50)
- `GigabitEthernet0/1` → Router2 (WAN)

**Step 1:** Open the CLI. Click Router4 → CLI tab → press Enter.

**Step 2:** Set hostname and configure the WAN interface.

```
Router> enable
Router# configure terminal
Router(config)# hostname Router4

Router4(config)# interface GigabitEthernet0/1
Router4(config-if)# ip address 10.0.2.2 255.255.255.252
Router4(config-if)# no shutdown
Router4(config-if)# exit
```

**Step 3:** Configure the trunk LAN interface for Switch3 (VLAN 50 Server Room).

```
Router4(config)# interface GigabitEthernet0/0
Router4(config-if)# no shutdown
Router4(config-if)# exit

Router4(config)# interface GigabitEthernet0/0.50
Router4(config-subif)# encapsulation dot1Q 50
Router4(config-subif)# ip address 192.168.50.1 255.255.255.0
Router4(config-subif)# exit
```

**Step 4:** Configure OSPF area 0.

```
Router4(config)# router ospf 1
Router4(config-router)# network 192.168.50.0 0.0.0.255 area 0
Router4(config-router)# network 10.0.2.0 0.0.0.3 area 0
Router4(config-router)# exit
```

> **Note:** Router4 advertises the Server Room subnet (192.168.50.0/24) and its WAN link (10.0.2.0/30) into OSPF area 0. Router2 receives these OSPF routes and redistributes them into RIP so other routers learn about the Server Room.

**Step 5:** Add the default route.

```
Router4(config)# ip route 0.0.0.0 0.0.0.0 10.0.2.1
```

> **Explanation:** This is the default route. Any traffic that Router4 does not have a specific route for (such as VLAN 10, 20, 30, 40, or 60) is forwarded to Router2 (10.0.2.1). Router2 knows all routes (via RIP) and will forward the packet to the correct destination.

**Step 6:** Save.

```
Router4(config)# end
Router4# write memory
```

---

## 6. Switch Configuration

For each switch, you need to:
1. Create the VLANs.
2. Set the uplink port (to the router or to the parent switch) as a **trunk** port.
3. Set the PC/server ports as **access** ports with the correct VLAN.

---

### Switch 1 Configuration (HR — VLAN 10 + IT — VLAN 20)

Click on Switch1 → CLI tab → press Enter.

```
Switch> enable
Switch# configure terminal
Switch(config)# hostname Switch1

! Create VLAN 10
Switch1(config)# vlan 10
Switch1(config-vlan)# name HR
Switch1(config-vlan)# exit

! Create VLAN 20
Switch1(config)# vlan 20
Switch1(config-vlan)# name IT
Switch1(config-vlan)# exit

! Set trunk port (uplink to Router1 GigabitEthernet0/0)
Switch1(config)# interface FastEthernet0/1
Switch1(config-if)# switchport mode trunk
Switch1(config-if)# exit

! Set trunk port (cascade to Switch5)
Switch1(config)# interface FastEthernet0/24
Switch1(config-if)# switchport mode trunk
Switch1(config-if)# exit

! Set access ports for HR (VLAN 10)
Switch1(config)# interface FastEthernet0/2
Switch1(config-if)# switchport mode access
Switch1(config-if)# switchport access vlan 10
Switch1(config-if)# exit

Switch1(config)# interface FastEthernet0/3
Switch1(config-if)# switchport mode access
Switch1(config-if)# switchport access vlan 10
Switch1(config-if)# exit

Switch1(config)# interface FastEthernet0/4
Switch1(config-if)# switchport mode access
Switch1(config-if)# switchport access vlan 10
Switch1(config-if)# exit

! Set access ports for IT (VLAN 20)
Switch1(config)# interface FastEthernet0/5
Switch1(config-if)# switchport mode access
Switch1(config-if)# switchport access vlan 20
Switch1(config-if)# exit

Switch1(config)# interface FastEthernet0/6
Switch1(config-if)# switchport mode access
Switch1(config-if)# switchport access vlan 20
Switch1(config-if)# exit

Switch1(config)# interface FastEthernet0/7
Switch1(config-if)# switchport mode access
Switch1(config-if)# switchport access vlan 20
Switch1(config-if)# exit

Switch1(config)# end
Switch1# write memory
```

---

### Switch 2 Configuration (Finance — VLAN 30 + Sales — VLAN 40)

Click on Switch2 → CLI tab → press Enter.

```
Switch> enable
Switch# configure terminal
Switch(config)# hostname Switch2

! Create VLAN 30
Switch2(config)# vlan 30
Switch2(config-vlan)# name Finance
Switch2(config-vlan)# exit

! Create VLAN 40
Switch2(config)# vlan 40
Switch2(config-vlan)# name Sales
Switch2(config-vlan)# exit

! Set trunk port (uplink to Router2 GigabitEthernet0/0)
Switch2(config)# interface FastEthernet0/1
Switch2(config-if)# switchport mode trunk
Switch2(config-if)# exit

! Set access ports for Finance (VLAN 30)
Switch2(config)# interface FastEthernet0/2
Switch2(config-if)# switchport mode access
Switch2(config-if)# switchport access vlan 30
Switch2(config-if)# exit

Switch2(config)# interface FastEthernet0/3
Switch2(config-if)# switchport mode access
Switch2(config-if)# switchport access vlan 30
Switch2(config-if)# exit

Switch2(config)# interface FastEthernet0/4
Switch2(config-if)# switchport mode access
Switch2(config-if)# switchport access vlan 30
Switch2(config-if)# exit

! Set access ports for Sales (VLAN 40)
Switch2(config)# interface FastEthernet0/5
Switch2(config-if)# switchport mode access
Switch2(config-if)# switchport access vlan 40
Switch2(config-if)# exit

Switch2(config)# interface FastEthernet0/6
Switch2(config-if)# switchport mode access
Switch2(config-if)# switchport access vlan 40
Switch2(config-if)# exit

Switch2(config)# interface FastEthernet0/7
Switch2(config-if)# switchport mode access
Switch2(config-if)# switchport access vlan 40
Switch2(config-if)# exit

Switch2(config)# end
Switch2# write memory
```

---

### Switch 3 Configuration (Server Room — VLAN 50)

Click on Switch3 → CLI tab → press Enter.

```
Switch> enable
Switch# configure terminal
Switch(config)# hostname Switch3

! Create VLAN 50
Switch3(config)# vlan 50
Switch3(config-vlan)# name Servers
Switch3(config-vlan)# exit

! Set trunk port (uplink to Router4 GigabitEthernet0/0)
Switch3(config)# interface FastEthernet0/1
Switch3(config-if)# switchport mode trunk
Switch3(config-if)# exit

! Set access ports for servers (VLAN 50)
Switch3(config)# interface FastEthernet0/2
Switch3(config-if)# switchport mode access
Switch3(config-if)# switchport access vlan 50
Switch3(config-if)# exit

Switch3(config)# interface FastEthernet0/3
Switch3(config-if)# switchport mode access
Switch3(config-if)# switchport access vlan 50
Switch3(config-if)# exit

Switch3(config)# interface FastEthernet0/4
Switch3(config-if)# switchport mode access
Switch3(config-if)# switchport access vlan 50
Switch3(config-if)# exit

Switch3(config)# interface FastEthernet0/5
Switch3(config-if)# switchport mode access
Switch3(config-if)# switchport access vlan 50
Switch3(config-if)# exit

Switch3(config)# end
Switch3# write memory
```

---

### Switch 4 Configuration (Management — VLAN 60)

Click on Switch4 → CLI tab → press Enter.

```
Switch> enable
Switch# configure terminal
Switch(config)# hostname Switch4

! Create VLAN 60
Switch4(config)# vlan 60
Switch4(config-vlan)# name Management
Switch4(config-vlan)# exit

! Set trunk port (uplink to Router3 GigabitEthernet0/2)
Switch4(config)# interface FastEthernet0/1
Switch4(config-if)# switchport mode trunk
Switch4(config-if)# exit

! Set access ports for Admin PCs (VLAN 60)
Switch4(config)# interface FastEthernet0/2
Switch4(config-if)# switchport mode access
Switch4(config-if)# switchport access vlan 60
Switch4(config-if)# exit

Switch4(config)# interface FastEthernet0/3
Switch4(config-if)# switchport mode access
Switch4(config-if)# switchport access vlan 60
Switch4(config-if)# exit

Switch4(config)# end
Switch4# write memory
```

---

### Switch 5 Configuration (HR + IT Extension — Cascade from Switch1)

> **About Switch5:** This is the cascading switch added in V2. It is connected to Switch1 (not to a router directly). The trunk between Switch1 Fa0/24 and Switch5 Fa0/1 carries VLAN 10 and VLAN 20. PCs connected to Switch5 behave exactly like PCs connected to Switch1 — they get DHCP from the same pools and use the same gateways.

Click on Switch5 → CLI tab → press Enter.

```
Switch> enable
Switch# configure terminal
Switch(config)# hostname Switch5

! Create VLAN 10
Switch5(config)# vlan 10
Switch5(config-vlan)# name HR
Switch5(config-vlan)# exit

! Create VLAN 20
Switch5(config)# vlan 20
Switch5(config-vlan)# name IT
Switch5(config-vlan)# exit

! Set trunk port (uplink to Switch1 FastEthernet0/24)
Switch5(config)# interface FastEthernet0/1
Switch5(config-if)# switchport mode trunk
Switch5(config-if)# exit

! Set access ports for additional HR PCs (VLAN 10)
Switch5(config)# interface FastEthernet0/2
Switch5(config-if)# switchport mode access
Switch5(config-if)# switchport access vlan 10
Switch5(config-if)# exit

Switch5(config)# interface FastEthernet0/3
Switch5(config-if)# switchport mode access
Switch5(config-if)# switchport access vlan 10
Switch5(config-if)# exit

! Set access port for additional IT PC (VLAN 20)
Switch5(config)# interface FastEthernet0/4
Switch5(config-if)# switchport mode access
Switch5(config-if)# switchport access vlan 20
Switch5(config-if)# exit

Switch5(config)# end
Switch5# write memory
```

---

## 7. DHCP Server Configuration

The DHCP server assigns IP addresses automatically to PCs.
You configure it through the Packet Tracer GUI (not CLI).

**Step 1:** Click on **DHCP-Server**.

**Step 2:** Click the **"Services"** tab.

**Step 3:** Click **"DHCP"** in the left menu.

**Step 4:** Make sure DHCP service is **ON**.

**Step 5:** Delete the default pool if one exists. Then add the following pools one by one.

For each pool, fill in the fields and click **"Add"**.

---

#### Pool: HR-Pool

| Field             | Value             |
|-------------------|-------------------|
| Pool Name         | HR-Pool           |
| Default Gateway   | 192.168.10.1      |
| DNS Server        | 192.168.50.20     |
| Start IP Address  | 192.168.10.100    |
| Subnet Mask       | 255.255.255.0     |
| Max Users         | 101               |

---

#### Pool: IT-Pool

| Field             | Value             |
|-------------------|-------------------|
| Pool Name         | IT-Pool           |
| Default Gateway   | 192.168.20.1      |
| DNS Server        | 192.168.50.20     |
| Start IP Address  | 192.168.20.100    |
| Subnet Mask       | 255.255.255.0     |
| Max Users         | 101               |

---

#### Pool: Finance-Pool

| Field             | Value             |
|-------------------|-------------------|
| Pool Name         | Finance-Pool      |
| Default Gateway   | 192.168.30.1      |
| DNS Server        | 192.168.50.20     |
| Start IP Address  | 192.168.30.100    |
| Subnet Mask       | 255.255.255.0     |
| Max Users         | 101               |

---

#### Pool: Sales-Pool

| Field             | Value             |
|-------------------|-------------------|
| Pool Name         | Sales-Pool        |
| Default Gateway   | 192.168.40.1      |
| DNS Server        | 192.168.50.20     |
| Start IP Address  | 192.168.40.100    |
| Subnet Mask       | 255.255.255.0     |
| Max Users         | 101               |

> **Note:** Servers (VLAN 50) and Admin PCs (VLAN 60) use static IP addresses. No DHCP pool is needed for them.

> **Why do PCs on Switch5 get DHCP?** The DHCP request from HR-PC4 or IT-PC4 travels through the Switch5 trunk → Switch1 trunk → Router1. Router1's sub-interface (Gig0/0.10 or Gig0/0.20) has `ip helper-address 192.168.50.10` which forwards the request to the DHCP server. The server responds with an IP from HR-Pool or IT-Pool.

---

## 8. DNS Server Configuration

The DNS server resolves hostnames (like `www.company.local`) to IP addresses.

**Step 1:** Click on **DNS-Server**.

**Step 2:** Click the **"Services"** tab.

**Step 3:** Click **"DNS"** in the left menu.

**Step 4:** Make sure DNS service is **ON**.

**Step 5:** Add DNS records:

| Name                  | Type   | Address          |
|-----------------------|--------|------------------|
| www.company.local     | A      | 192.168.50.30    |
| mail.company.local    | A      | 192.168.50.40    |
| dhcp.company.local    | A      | 192.168.50.10    |
| dns.company.local     | A      | 192.168.50.20    |

For each record:
1. Type the **Name** in the Name field.
2. Select **A Record** from the Type dropdown.
3. Type the **Address** (IP address).
4. Click **"Add"**.

**Step 6:** Click **"Save"** if available.

---

## 9. Web Server Configuration

**Step 1:** Click on **Web-Server**.

**Step 2:** Click the **"Services"** tab.

**Step 3:** Click **"HTTP"** in the left menu.

**Step 4:** Make sure HTTP service is **ON**.

**Step 5:** You can edit the `index.html` page content if you want to display a custom message. The default page works fine for testing.

---

## 10. Mail Server Configuration

**Step 1:** Click on **Mail-Server**.

**Step 2:** Click the **"Services"** tab.

**Step 3:** Click **"EMAIL"** in the left menu.

**Step 4:** Make sure SMTP and POP3 services are **ON**.

**Step 5:** Set the domain name:
- **Domain Name:** `company.local`

**Step 6:** Add mail user accounts (optional, for testing):

| Username  | Password  |
|-----------|-----------|
| hr.user   | pass1234  |
| it.user   | pass1234  |
| finance   | pass1234  |
| sales     | pass1234  |

Click **"Set"** or **"Add"** for each user.

---

## 11. PC Configuration

PCs in the department VLANs (10, 20, 30, 40) use DHCP.
Admin PCs in VLAN 60 use static IP addresses.

### How to Set a PC to DHCP

1. Click on the PC.
2. Click the **"Desktop"** tab.
3. Click **"IP Configuration"**.
4. Select **"DHCP"**.
5. Wait a few seconds. The PC will receive an IP address from the DHCP server.
6. If no IP appears, check that the router sub-interface and the DHCP pool are configured correctly.

---

### Department PC Settings (DHCP)

| PC           | VLAN | Switch  | Port   | Expected IP Range        |
|--------------|------|---------|--------|--------------------------|
| HR-PC1       | 10   | Switch1 | Fa0/2  | 192.168.10.100 – .200    |
| HR-PC2       | 10   | Switch1 | Fa0/3  | 192.168.10.100 – .200    |
| HR-PC3       | 10   | Switch1 | Fa0/4  | 192.168.10.100 – .200    |
| IT-PC1       | 20   | Switch1 | Fa0/5  | 192.168.20.100 – .200    |
| IT-PC2       | 20   | Switch1 | Fa0/6  | 192.168.20.100 – .200    |
| IT-PC3       | 20   | Switch1 | Fa0/7  | 192.168.20.100 – .200    |
| Finance-PC1  | 30   | Switch2 | Fa0/2  | 192.168.30.100 – .200    |
| Finance-PC2  | 30   | Switch2 | Fa0/3  | 192.168.30.100 – .200    |
| Finance-PC3  | 30   | Switch2 | Fa0/4  | 192.168.30.100 – .200    |
| Sales-PC1    | 40   | Switch2 | Fa0/5  | 192.168.40.100 – .200    |
| Sales-PC2    | 40   | Switch2 | Fa0/6  | 192.168.40.100 – .200    |
| Sales-PC3    | 40   | Switch2 | Fa0/7  | 192.168.40.100 – .200    |
| HR-PC4       | 10   | Switch5 | Fa0/2  | 192.168.10.100 – .200    |
| HR-PC5       | 10   | Switch5 | Fa0/3  | 192.168.10.100 – .200    |
| IT-PC4       | 20   | Switch5 | Fa0/4  | 192.168.20.100 – .200    |

---

### Admin PC Static IP Settings

Admin PCs in VLAN 60 use static IPs. Do not use DHCP for these.

**Admin-PC1:**

1. Click Admin-PC1 → **Desktop** tab → **IP Configuration**.
2. Select **Static**.
3. Enter:
   - IP Address: `192.168.60.10`
   - Subnet Mask: `255.255.255.0`
   - Default Gateway: `192.168.60.1`
   - DNS Server: `192.168.50.20`

**Admin-PC2:**

1. Click Admin-PC2 → **Desktop** tab → **IP Configuration**.
2. Select **Static**.
3. Enter:
   - IP Address: `192.168.60.20`
   - Subnet Mask: `255.255.255.0`
   - Default Gateway: `192.168.60.1`
   - DNS Server: `192.168.50.20`

---

### Server Static IP Settings

**DHCP-Server:**
- IP Address: `192.168.50.10`
- Subnet Mask: `255.255.255.0`
- Default Gateway: `192.168.50.1`
- DNS Server: `192.168.50.20`

**DNS-Server:**
- IP Address: `192.168.50.20`
- Subnet Mask: `255.255.255.0`
- Default Gateway: `192.168.50.1`

**Web-Server:**
- IP Address: `192.168.50.30`
- Subnet Mask: `255.255.255.0`
- Default Gateway: `192.168.50.1`
- DNS Server: `192.168.50.20`

**Mail-Server:**
- IP Address: `192.168.50.40`
- Subnet Mask: `255.255.255.0`
- Default Gateway: `192.168.50.1`
- DNS Server: `192.168.50.20`

To set static IPs on servers:
1. Click the server.
2. Click **"Desktop"** tab.
3. Click **"IP Configuration"**.
4. Select **"Static"**.
5. Enter the values above.

---

## 12. Testing and Verification

### 12.1 Verify Router Interfaces

Run this on each router to see if all interfaces are up:

```
show ip interface brief
```

All relevant interfaces should show **up / up** (not administratively down or down).

---

### 12.2 Verify Routing Tables

Run this on each router to see the routes it has learned:

```
show ip route
```

**On Router1,** you should see routes with codes:
- `C` — Connected (192.168.10.0, 192.168.20.0, 10.0.0.0/30, 10.0.3.0/30)
- `R` — RIP (192.168.30.0, 192.168.40.0, 192.168.50.0, 192.168.60.0, and WAN subnets)

**On Router2,** you should see:
- `C` — Connected
- `R` — RIP
- `O` — OSPF (routes from Router4)

**On Router3,** you should see:
- `C` — Connected
- `R` — RIP
- `S` — Static (10.0.2.0/30)

**On Router4,** you should see:
- `C` — Connected (192.168.50.0, 10.0.2.0/30)
- `O` — OSPF (all redistributed routes from Router2)
- `S*` — Default route (0.0.0.0/0 via 10.0.2.1)

---

### 12.3 Verify RIP

Run on Router1, Router2, or Router3:

```
show ip rip database
```

You should see all subnet entries that RIP has learned.

Run on Router2 (which has redistribution):

```
show ip protocols
```

Look for:
- `Routing Protocol is "rip"`
- `Outgoing update filter list for all interfaces is not set`
- `Redistributing: ospf 1`

And:
- `Routing Protocol is "ospf 1"`
- `Redistributing: rip`

---

### 12.4 Verify OSPF

Run on Router2 or Router4:

```
show ip ospf neighbor
```

You should see the OSPF neighbor relationship between Router2 and Router4:
- Router2 sees Router4 as a neighbor.
- Router4 sees Router2 as a neighbor.
- State should be **FULL**.

Run:

```
show ip ospf database
```

This shows all LSAs (Link State Advertisements) in the OSPF database.

---

### 12.5 Verify VLANs on Switches

Run on any switch:

```
show vlan brief
```

You should see all configured VLANs listed and their active ports.

Run:

```
show interfaces trunk
```

This shows which ports are trunk ports and which VLANs are allowed on each trunk.

---

### 12.6 Router-to-Router Ping Tests

Open the CLI on each router and ping the neighboring router's interface.

**From Router1:**

```
ping 10.0.0.2
ping 10.0.3.2
ping 192.168.30.1
ping 192.168.50.1
```

**From Router4:**

```
ping 10.0.2.1
ping 192.168.10.1
ping 192.168.60.1
```

All pings should succeed. If a ping fails, check the routing table on that router with `show ip route`.

---

### 12.7 PC Ping Tests

Open the **Command Prompt** on any PC (click PC → Desktop → Command Prompt).

**From HR-PC1 (VLAN 10):**

```
ping 192.168.10.1
ping 192.168.20.1
ping 192.168.30.1
ping 192.168.40.1
ping 192.168.50.10
ping 192.168.60.10
```

All pings should succeed.

**From HR-PC4 (on Switch5 cascade):**

```
ping 192.168.10.100
ping 192.168.50.10
ping 192.168.60.10
```

HR-PC4 should reach HR-PC1, the DHCP server, and the Admin PC. This confirms the Switch5 cascade works.

**From Admin-PC1 (VLAN 60):**

```
ping 192.168.60.1
ping 192.168.10.100
ping 192.168.50.10
```

---

### 12.8 Web Browser Test

1. Click on any PC.
2. Click the **"Desktop"** tab.
3. Click **"Web Browser"**.
4. Type: `http://192.168.50.30` and press Enter.
5. The web page from Web-Server should load.
6. You can also try: `http://www.company.local` (if DNS is configured correctly).

---

### 12.9 Troubleshooting Guide

| Problem | Likely Cause | Fix |
|---|---|---|
| PC gets no DHCP IP | Router helper-address missing | Add `ip helper-address 192.168.50.10` to the correct sub-interface on the router |
| PC gets no DHCP IP | DHCP pool not created | Add the correct pool on DHCP-Server |
| Ping between VLANs fails | Missing sub-interface or encapsulation | Check router sub-interfaces with `show ip interface brief` |
| Router interface shows Down | No `no shutdown` applied | Enter the interface and type `no shutdown` |
| RIP routes not learned | Wrong network statement | Check `show ip protocols` on each RIP router |
| OSPF neighbor not forming | Interfaces not in same area | Check OSPF config on Router2 and Router4 |
| Switch5 PCs get wrong VLAN IP | Switch5 trunk not configured | Check `show interfaces trunk` on Switch1 and Switch5 |
| Static route not in routing table | Wrong next-hop IP | Verify the next-hop is reachable (`ping 10.0.1.1` from Router3) |
| Redistribution not working | Missing `subnets` keyword | Add `subnets` to `redistribute rip` in OSPF config on Router2 |

---

*Author: Mohammed Abuaisha \ mabuaisha1@smail.ucas.edu.ps*
