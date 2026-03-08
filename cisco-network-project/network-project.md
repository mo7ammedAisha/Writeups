# Cisco Packet Tracer Company Network Project

## Introduction

This document explains how to build a company network using Cisco Packet Tracer.
The network has 5 routers, 5 switches, 4 servers, and 8 PCs.
It uses VLANs, RIP routing (with demonstrations of Static Routing and Default Route), DHCP, DNS, Web, and Mail services.

The routers are **Cisco 2911**. The switches are **Cisco 2960**.
Follow each step carefully. You will be able to finish the whole project using only this guide.

> **Note:** Read Section 0 first. It shows you how to add devices and cables in Packet Tracer. Do not skip it.

---

## Table of Contents

0. [Setting Up the Workspace](#0-setting-up-the-workspace)
1. [Network Topology Overview](#1-network-topology-overview)
2. [IP Addressing Scheme](#2-ip-addressing-scheme)
3. [VLAN Plan](#3-vlan-plan)
4. [Cable Connection Table](#4-cable-connection-table)
5. [Router Configuration (RIP)](#5-router-configuration-rip)
6. [Switch Configuration](#6-switch-configuration)
7. [DHCP Server Configuration](#7-dhcp-server-configuration)
8. [DNS Server Configuration](#8-dns-server-configuration)
9. [Web Server Configuration](#9-web-server-configuration)
10. [Mail Server Configuration](#10-mail-server-configuration)
11. [PC Configuration](#11-pc-configuration)
12. [Testing and Verification](#12-testing-and-verification)
13. [Bonus: Convert Router3 to Static Routing](#13-bonus-convert-router3-to-static-routing)
14. [Bonus: Add a Default Route on Router4](#14-bonus-add-a-default-route-on-router4)
15. [Live Presentation & Demo Guide](#15-live-presentation--demo-guide)

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

**Repeat 5 times. You need 5 routers total.**

**How to rename a router:**

1. Click on the router icon.
2. A window opens. Click the **"Config"** tab.
3. At the top, find the field that says **"Display Name"**.
4. Type the new name: `Router1`, `Router2`, `Router3`, `Router4`, or `Router5`.
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

Use the same steps: click the server → Config tab → Display Name.

---

### 0.5 How to Add PCs (PC-PT)

You need 8 PCs. There are 2 PCs for each department.

**Steps to add one PC:**

1. Look at the bottom-left panel.
2. Click **"End Devices"**.
3. Find **"PC"** or **"PC-PT"** in the list.
4. Click on it.
5. Click in the workspace to place the PC.

**Repeat 8 times.**

**Rename them:**
- HR-PC1, HR-PC2
- IT-PC1, IT-PC2
- Finance-PC1, Finance-PC2
- Sales-PC1, Sales-PC2

---

### 0.6 How to Connect Devices with Cables

1. Look at the bottom-left panel.
2. Click the **lightning bolt icon**. This is **"Connections"**.
3. A list of cable types appears.

**Which cable to use:**

| Connection Type         | Cable Type                |
|-------------------------|---------------------------|
| Router to Router        | Copper Cross-Over         |
| Router to Switch        | Copper Straight-Through   |
| Switch to PC            | Copper Straight-Through   |
| Switch to Server        | Copper Straight-Through   |

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

---

### 0.7 How to Add a Hardware Module to a Router (HWIC-2T) — Optional / Advanced

> **Skip this section if you are following this project.** The Cisco 2911 has 3 built-in GigabitEthernet ports, which is enough for this design. This section is only for projects where a router needs more than 3 interfaces.

The HWIC-2T module adds 2 serial interfaces to a router.
After adding it, the new interfaces are named `Serial0/0/0` and `Serial0/0/1`.

**Steps to add HWIC-2T:**

1. Click on the router.
2. Click the **"Physical"** tab.
3. You will see an image of the router.
4. Find the **power button** (green circle) on the router image.
5. Click the power button to **turn OFF** the router.
   > **Important:** You MUST turn off the router before adding modules.
6. Look at the left side of the Physical tab. You will see a list of modules.
7. Find **"HWIC-2T"** in the list.
8. Drag it into an empty HWIC slot on the router image.
9. Click the power button again to **turn the router back ON**.
10. Wait a few seconds for the router to boot.
11. Click the **"CLI"** tab.
12. Type: `show ip interface brief`
13. You will see the new Serial interfaces in the list.

---

## 1. Network Topology Overview

### About the Company

The company has four departments and one server room:

| Department  | VLAN | Switch  | Router        |
|-------------|------|---------|---------------|
| HR          | 10   | Switch1 | Router1       |
| IT          | 20   | Switch2 | Router1       |
| Finance     | 30   | Switch3 | Router2       |
| Sales       | 40   | Switch4 | Router2       |
| Server Room | 50   | Switch5 | Router5       |

Each department is on a separate VLAN.
All departments can reach the servers through the routers.

### Device List

| Device      | Model      | Count | Purpose                          |
|-------------|------------|-------|----------------------------------|
| Routers     | Cisco 2911 | 5     | Connect networks, RIP + Static + Default routing |
| Switches    | Cisco 2960 | 5     | Connect PCs within departments   |
| Servers     | Server-PT  | 4     | DHCP, DNS, Web, Mail             |
| PCs         | PC-PT      | 8     | End user devices (2 per dept)    |

### Topology Design

This network uses a **chain topology** for WAN links:

```
                       +--------------------+
                       |      Router5       |
                       |   (Core Router)    |
                       | Gig0/0: 10.0.2.2   |
                       | Gig0/1: 10.0.3.2   |
                       | Gig0/2 → Switch5   |
                       +---+----------+-----+
                           |          |
               10.0.2.0/30 |          | 10.0.3.0/30
                           |          |
                    +------+--+    +--+------+
                    | Router3 |    | Router4 |
                    | Gig0/0: |    | Gig0/0: |
                    | 10.0.0.2|    | 10.0.1.2|
                    | Gig0/1: |    | Gig0/1: |
                    | 10.0.2.1|    | 10.0.3.1|
                    +----+----+    +----+----+
                         |              |
             10.0.0.0/30 |              | 10.0.1.0/30
                         |              |
                  +------+--+    +------+--+
                  | Router1 |    | Router2 |
                  | Gig0/0  |    | Gig0/0  |
                  | → SW1   |    | → SW3   |
                  | Gig0/1  |    | Gig0/1  |
                  | → R3    |    | → R4    |
                  | Gig0/2  |    | Gig0/2  |
                  | → SW2   |    | → SW4   |
                  +--+---+--+    +--+---+--+
                     |   |          |   |
               +-----+   +--+  +---+   +-----+
               |             |  |             |
          +----+----+  +------++ ++------+  +-+-------+
          | Switch1 |  |Switch2| |Switch3|  | Switch4 |
          | VLAN 10 |  |VLAN 20| |VLAN 30|  | VLAN 40 |
          |  (HR)   |  |  (IT) | |(Fin.) |  | (Sales) |
          +---+---+-+  +--+--+-+ +-+--+--+  +-+----+--+
              |   |       |  |     |  |        |    |
           [HR  [HR    [IT  [IT  [Fin [Fin  [Sales [Sales
           PC1] PC2]   PC1] PC2] PC1] PC2] PC1]  PC2]

                       +-------------------+
                       |      Switch5      |
                       |    VLAN 50        |
                       |  (Server Room)    |
                       +---+--+--+--+------+
                           |  |  |  |
               +-----------+  |  |  +------------+
               |        +-----+  +------+         |
          +----+------+ +------+ +-----+--+ +-----+---+
          |DHCP-Server| |  DNS | |  Web   | |  Mail   |
          |192.168.   | |Server| | Server | | Server  |
          |50.10      | |.20   | | .30    | | .40     |
          +-----------+ +------+ +--------+ +---------+
```

**Summary of connections:**

- Router1 connects to Switch1 (HR), Switch2 (IT), and Router3
- Router2 connects to Switch3 (Finance), Switch4 (Sales), and Router4
- Router3 connects to Router1 and Router5 (it is a transit router)
- Router4 connects to Router2 and Router5 (it is a transit router)
- Router5 connects to Router3, Router4, and Switch5 (Server Room)

---

## 2. IP Addressing Scheme

### VLAN and Subnet Table

| VLAN ID | VLAN Name   | Subnet           | Gateway       | DHCP Range              |
|---------|-------------|------------------|---------------|-------------------------|
| 10      | HR          | 192.168.10.0/24  | 192.168.10.1  | 192.168.10.100 – .200   |
| 20      | IT          | 192.168.20.0/24  | 192.168.20.1  | 192.168.20.100 – .200   |
| 30      | Finance     | 192.168.30.0/24  | 192.168.30.1  | 192.168.30.100 – .200   |
| 40      | Sales       | 192.168.40.0/24  | 192.168.40.1  | 192.168.40.100 – .200   |
| 50      | Servers     | 192.168.50.0/24  | 192.168.50.1  | Static only (no DHCP)   |
| 1       | Native      | —                | —             | Default/management VLAN |

### WAN Point-to-Point Links (/30 subnets)

| WAN Link  | Network        | Router A End  | Router B End  |
|-----------|----------------|---------------|---------------|
| R1 ↔ R3   | 10.0.0.0/30    | R1 = 10.0.0.1 | R3 = 10.0.0.2 |
| R2 ↔ R4   | 10.0.1.0/30    | R2 = 10.0.1.1 | R4 = 10.0.1.2 |
| R3 ↔ R5   | 10.0.2.0/30    | R3 = 10.0.2.1 | R5 = 10.0.2.2 |
| R4 ↔ R5   | 10.0.3.0/30    | R4 = 10.0.3.1 | R5 = 10.0.3.2 |

### Server Static IP Addresses

| Server      | IP Address      | VLAN | Service     |
|-------------|-----------------|------|-------------|
| DHCP-Server | 192.168.50.10   | 50   | DHCP        |
| DNS-Server  | 192.168.50.20   | 50   | DNS         |
| Web-Server  | 192.168.50.30   | 50   | HTTP        |
| Mail-Server | 192.168.50.40   | 50   | SMTP / POP3 |

### Router LAN Sub-Interface IPs

> **Why different physical interfaces?** Router1 uses `GigabitEthernet0/0` for Switch1 (HR) and `GigabitEthernet0/2` for Switch2 (IT) because each switch has its own cable to the router. `GigabitEthernet0/1` is used for the WAN link to Router3. The sub-interface number (`.10`, `.20`, `.30`) matches the VLAN ID for easy identification.

| Router  | Interface              | VLAN | IP Address       |
|---------|------------------------|------|------------------|
| R1      | GigabitEthernet0/0.10  | 10   | 192.168.10.1/24  |
| R1      | GigabitEthernet0/2.20  | 20   | 192.168.20.1/24  |
| R2      | GigabitEthernet0/0.30  | 30   | 192.168.30.1/24  |
| R2      | GigabitEthernet0/2.40  | 40   | 192.168.40.1/24  |
| R5      | GigabitEthernet0/2.50  | 50   | 192.168.50.1/24  |

---

## 3. VLAN Plan

### VLAN List

| VLAN ID | Name    | Description               |
|---------|---------|---------------------------|
| 1       | Native  | Default VLAN (management) |
| 10      | HR      | Human Resources           |
| 20      | IT      | Information Technology    |
| 30      | Finance | Finance Department        |
| 40      | Sales   | Sales Department          |
| 50      | Servers | Server Room               |

### Port Assignment per Switch

#### Switch1 — HR Department (VLAN 10)

| Port      | Mode   | VLAN | Connected To              |
|-----------|--------|------|---------------------------|
| Fa0/1     | Trunk  | All  | Router1 GigabitEthernet0/0 |
| Fa0/2     | Access | 10   | HR-PC1                    |
| Fa0/3     | Access | 10   | HR-PC2                    |

#### Switch2 — IT Department (VLAN 20)

| Port      | Mode   | VLAN | Connected To              |
|-----------|--------|------|---------------------------|
| Fa0/1     | Trunk  | All  | Router1 GigabitEthernet0/2 |
| Fa0/2     | Access | 20   | IT-PC1                    |
| Fa0/3     | Access | 20   | IT-PC2                    |

#### Switch3 — Finance Department (VLAN 30)

| Port      | Mode   | VLAN | Connected To              |
|-----------|--------|------|---------------------------|
| Fa0/1     | Trunk  | All  | Router2 GigabitEthernet0/0 |
| Fa0/2     | Access | 30   | Finance-PC1               |
| Fa0/3     | Access | 30   | Finance-PC2               |

#### Switch4 — Sales Department (VLAN 40)

| Port      | Mode   | VLAN | Connected To              |
|-----------|--------|------|---------------------------|
| Fa0/1     | Trunk  | All  | Router2 GigabitEthernet0/2 |
| Fa0/2     | Access | 40   | Sales-PC1                 |
| Fa0/3     | Access | 40   | Sales-PC2                 |

#### Switch5 — Server Room (VLAN 50)

| Port      | Mode   | VLAN | Connected To              |
|-----------|--------|------|---------------------------|
| Fa0/1     | Trunk  | All  | Router5 GigabitEthernet0/2 |
| Fa0/2     | Access | 50   | DHCP-Server               |
| Fa0/3     | Access | 50   | DNS-Server                |
| Fa0/4     | Access | 50   | Web-Server                |
| Fa0/5     | Access | 50   | Mail-Server               |

---

## 4. Cable Connection Table

Use this table to connect all devices.
Connect every cable before you start typing commands.

| Cable # | From Device | From Interface         | To Device   | To Interface            | Cable Type         |
|---------|-------------|------------------------|-------------|-------------------------|--------------------|
| 1       | Router1     | GigabitEthernet0/0     | Switch1     | FastEthernet0/1         | Straight-Through   |
| 2       | Router1     | GigabitEthernet0/1     | Router3     | GigabitEthernet0/0      | Cross-Over         |
| 3       | Router1     | GigabitEthernet0/2     | Switch2     | FastEthernet0/1         | Straight-Through   |
| 4       | Router2     | GigabitEthernet0/0     | Switch3     | FastEthernet0/1         | Straight-Through   |
| 5       | Router2     | GigabitEthernet0/1     | Router4     | GigabitEthernet0/0      | Cross-Over         |
| 6       | Router2     | GigabitEthernet0/2     | Switch4     | FastEthernet0/1         | Straight-Through   |
| 7       | Router3     | GigabitEthernet0/1     | Router5     | GigabitEthernet0/0      | Cross-Over         |
| 8       | Router4     | GigabitEthernet0/1     | Router5     | GigabitEthernet0/1      | Cross-Over         |
| 9       | Router5     | GigabitEthernet0/2     | Switch5     | FastEthernet0/1         | Straight-Through   |
| 10      | Switch1     | FastEthernet0/2        | HR-PC1      | FastEthernet0           | Straight-Through   |
| 11      | Switch1     | FastEthernet0/3        | HR-PC2      | FastEthernet0           | Straight-Through   |
| 12      | Switch2     | FastEthernet0/2        | IT-PC1      | FastEthernet0           | Straight-Through   |
| 13      | Switch2     | FastEthernet0/3        | IT-PC2      | FastEthernet0           | Straight-Through   |
| 14      | Switch3     | FastEthernet0/2        | Finance-PC1 | FastEthernet0           | Straight-Through   |
| 15      | Switch3     | FastEthernet0/3        | Finance-PC2 | FastEthernet0           | Straight-Through   |
| 16      | Switch4     | FastEthernet0/2        | Sales-PC1   | FastEthernet0           | Straight-Through   |
| 17      | Switch4     | FastEthernet0/3        | Sales-PC2   | FastEthernet0           | Straight-Through   |
| 18      | Switch5     | FastEthernet0/2        | DHCP-Server | FastEthernet0           | Straight-Through   |
| 19      | Switch5     | FastEthernet0/3        | DNS-Server  | FastEthernet0           | Straight-Through   |
| 20      | Switch5     | FastEthernet0/4        | Web-Server  | FastEthernet0           | Straight-Through   |
| 21      | Switch5     | FastEthernet0/5        | Mail-Server | FastEthernet0           | Straight-Through   |

> **Note for PC-PT and Server-PT:** Their network interface is named `FastEthernet0` (no slash number).

---

## 5. Router Configuration (RIP)

### What is RIP?

RIP stands for Routing Information Protocol.
It is a simple routing protocol.
Each router shares its network list with its neighbors.
We use RIP version 2. This version supports subnet masks.

### What is Router-on-a-Stick?

One physical cable connects the router to a switch trunk port.
The router uses **sub-interfaces** to handle each VLAN.
Example: `GigabitEthernet0/0.10` is the sub-interface for VLAN 10.
This allows one cable to carry traffic for many VLANs.

### What is DHCP Relay (ip helper-address)?

When a PC sends a DHCP request, it sends a broadcast.
Broadcasts do not cross routers.
`ip helper-address` tells the router to forward the DHCP broadcast to the DHCP server.
The DHCP server is at `192.168.50.10`.

---

### Router 1 Configuration

**Role:** Gateway for HR (VLAN 10) and IT (VLAN 20)
**Interfaces:**
- `GigabitEthernet0/0` → Switch1 (HR)
- `GigabitEthernet0/1` → Router3 (WAN)
- `GigabitEthernet0/2` → Switch2 (IT)

**Step 1:** Open the CLI.

Click on Router1 → click the **CLI** tab → press Enter.

**Step 2:** Set the hostname.

```
Router> enable
Router# configure terminal
Router(config)# hostname Router1
```

**Step 3:** Configure the WAN interface to Router3.

```
Router1(config)# interface GigabitEthernet0/1
Router1(config-if)# ip address 10.0.0.1 255.255.255.252
Router1(config-if)# no shutdown
Router1(config-if)# exit
```

**Step 4:** Configure the LAN interface for HR (Switch1, VLAN 10).

```
Router1(config)# interface GigabitEthernet0/0
Router1(config-if)# no shutdown
Router1(config-if)# exit

Router1(config)# interface GigabitEthernet0/0.10
Router1(config-subif)# encapsulation dot1Q 10
Router1(config-subif)# ip address 192.168.10.1 255.255.255.0
Router1(config-subif)# ip helper-address 192.168.50.10
Router1(config-subif)# exit
```

**Step 5:** Configure the LAN interface for IT (Switch2, VLAN 20).

```
Router1(config)# interface GigabitEthernet0/2
Router1(config-if)# no shutdown
Router1(config-if)# exit

Router1(config)# interface GigabitEthernet0/2.20
Router1(config-subif)# encapsulation dot1Q 20
Router1(config-subif)# ip address 192.168.20.1 255.255.255.0
Router1(config-subif)# ip helper-address 192.168.50.10
Router1(config-subif)# exit
```

**Step 6:** Configure RIP version 2.

```
Router1(config)# router rip
Router1(config-router)# version 2
Router1(config-router)# no auto-summary
Router1(config-router)# network 10.0.0.0
Router1(config-router)# network 192.168.10.0
Router1(config-router)# network 192.168.20.0
Router1(config-router)# exit
```

**Step 7:** Save the configuration.

```
Router1(config)# end
Router1# write memory
```

---

### Router 2 Configuration

**Role:** Gateway for Finance (VLAN 30) and Sales (VLAN 40)
**Interfaces:**
- `GigabitEthernet0/0` → Switch3 (Finance)
- `GigabitEthernet0/1` → Router4 (WAN)
- `GigabitEthernet0/2` → Switch4 (Sales)

**Step 1:** Open the CLI. Click Router2 → CLI tab → press Enter.

**Step 2:** Set the hostname and configure WAN.

```
Router> enable
Router# configure terminal
Router(config)# hostname Router2

Router2(config)# interface GigabitEthernet0/1
Router2(config-if)# ip address 10.0.1.1 255.255.255.252
Router2(config-if)# no shutdown
Router2(config-if)# exit
```

**Step 3:** Configure the LAN interface for Finance (Switch3, VLAN 30).

```
Router2(config)# interface GigabitEthernet0/0
Router2(config-if)# no shutdown
Router2(config-if)# exit

Router2(config)# interface GigabitEthernet0/0.30
Router2(config-subif)# encapsulation dot1Q 30
Router2(config-subif)# ip address 192.168.30.1 255.255.255.0
Router2(config-subif)# ip helper-address 192.168.50.10
Router2(config-subif)# exit
```

**Step 4:** Configure the LAN interface for Sales (Switch4, VLAN 40).

```
Router2(config)# interface GigabitEthernet0/2
Router2(config-if)# no shutdown
Router2(config-if)# exit

Router2(config)# interface GigabitEthernet0/2.40
Router2(config-subif)# encapsulation dot1Q 40
Router2(config-subif)# ip address 192.168.40.1 255.255.255.0
Router2(config-subif)# ip helper-address 192.168.50.10
Router2(config-subif)# exit
```

**Step 5:** Configure RIP version 2.

```
Router2(config)# router rip
Router2(config-router)# version 2
Router2(config-router)# no auto-summary
Router2(config-router)# network 10.0.1.0
Router2(config-router)# network 192.168.30.0
Router2(config-router)# network 192.168.40.0
Router2(config-router)# exit
```

**Step 6:** Save.

```
Router2(config)# end
Router2# write memory
```

---

### Router 3 Configuration

**Role:** Transit router (connects Router1 to Router5)
**Interfaces:**
- `GigabitEthernet0/0` → Router1 (WAN)
- `GigabitEthernet0/1` → Router5 (WAN)

**Step 1:** Open the CLI. Click Router3 → CLI tab → press Enter.

**Step 2:** Configure the router.

```
Router> enable
Router# configure terminal
Router(config)# hostname Router3

Router3(config)# interface GigabitEthernet0/0
Router3(config-if)# ip address 10.0.0.2 255.255.255.252
Router3(config-if)# no shutdown
Router3(config-if)# exit

Router3(config)# interface GigabitEthernet0/1
Router3(config-if)# ip address 10.0.2.1 255.255.255.252
Router3(config-if)# no shutdown
Router3(config-if)# exit

Router3(config)# router rip
Router3(config-router)# version 2
Router3(config-router)# no auto-summary
Router3(config-router)# network 10.0.0.0
Router3(config-router)# network 10.0.2.0
Router3(config-router)# exit

Router3(config)# end
Router3# write memory
```

---

### Router 4 Configuration

**Role:** Transit router (connects Router2 to Router5)
**Interfaces:**
- `GigabitEthernet0/0` → Router2 (WAN)
- `GigabitEthernet0/1` → Router5 (WAN)

**Step 1:** Open the CLI. Click Router4 → CLI tab → press Enter.

**Step 2:** Configure the router.

```
Router> enable
Router# configure terminal
Router(config)# hostname Router4

Router4(config)# interface GigabitEthernet0/0
Router4(config-if)# ip address 10.0.1.2 255.255.255.252
Router4(config-if)# no shutdown
Router4(config-if)# exit

Router4(config)# interface GigabitEthernet0/1
Router4(config-if)# ip address 10.0.3.1 255.255.255.252
Router4(config-if)# no shutdown
Router4(config-if)# exit

Router4(config)# router rip
Router4(config-router)# version 2
Router4(config-router)# no auto-summary
Router4(config-router)# network 10.0.1.0
Router4(config-router)# network 10.0.3.0
Router4(config-router)# exit

Router4(config)# end
Router4# write memory
```

---

### Router 5 Configuration

**Role:** Core router — connects Router3, Router4, and the Server Room
**Interfaces:**
- `GigabitEthernet0/0` → Router3 (WAN)
- `GigabitEthernet0/1` → Router4 (WAN)
- `GigabitEthernet0/2` → Switch5 (Server Room, VLAN 50)

**Step 1:** Open the CLI. Click Router5 → CLI tab → press Enter.

**Step 2:** Configure the router.

```
Router> enable
Router# configure terminal
Router(config)# hostname Router5

Router5(config)# interface GigabitEthernet0/0
Router5(config-if)# ip address 10.0.2.2 255.255.255.252
Router5(config-if)# no shutdown
Router5(config-if)# exit

Router5(config)# interface GigabitEthernet0/1
Router5(config-if)# ip address 10.0.3.2 255.255.255.252
Router5(config-if)# no shutdown
Router5(config-if)# exit

Router5(config)# interface GigabitEthernet0/2
Router5(config-if)# no shutdown
Router5(config-if)# exit

Router5(config)# interface GigabitEthernet0/2.50
Router5(config-subif)# encapsulation dot1Q 50
Router5(config-subif)# ip address 192.168.50.1 255.255.255.0
Router5(config-subif)# exit

Router5(config)# router rip
Router5(config-router)# version 2
Router5(config-router)# no auto-summary
Router5(config-router)# network 10.0.2.0
Router5(config-router)# network 10.0.3.0
Router5(config-router)# network 192.168.50.0
Router5(config-router)# exit

Router5(config)# end
Router5# write memory
```

---

## 6. Switch Configuration

For each switch, you need to:
1. Create the VLANs.
2. Set the uplink port (to the router) as a **trunk** port.
3. Set the PC ports as **access** ports with the correct VLAN.

---

### Switch 1 Configuration (HR — VLAN 10)

Click on Switch1 → CLI tab → press Enter.

```
Switch> enable
Switch# configure terminal
Switch(config)# hostname Switch1

! Create VLAN 10
Switch1(config)# vlan 10
Switch1(config-vlan)# name HR
Switch1(config-vlan)# exit

! Set trunk port (uplink to Router1 GigabitEthernet0/0)
Switch1(config)# interface FastEthernet0/1
Switch1(config-if)# switchport mode trunk
Switch1(config-if)# exit

! Set access port for HR-PC1
Switch1(config)# interface FastEthernet0/2
Switch1(config-if)# switchport mode access
Switch1(config-if)# switchport access vlan 10
Switch1(config-if)# exit

! Set access port for HR-PC2
Switch1(config)# interface FastEthernet0/3
Switch1(config-if)# switchport mode access
Switch1(config-if)# switchport access vlan 10
Switch1(config-if)# exit

Switch1(config)# end
Switch1# write memory
```

---

### Switch 2 Configuration (IT — VLAN 20)

Click on Switch2 → CLI tab → press Enter.

```
Switch> enable
Switch# configure terminal
Switch(config)# hostname Switch2

! Create VLAN 20
Switch2(config)# vlan 20
Switch2(config-vlan)# name IT
Switch2(config-vlan)# exit

! Set trunk port (uplink to Router1 GigabitEthernet0/2)
Switch2(config)# interface FastEthernet0/1
Switch2(config-if)# switchport mode trunk
Switch2(config-if)# exit

! Set access port for IT-PC1
Switch2(config)# interface FastEthernet0/2
Switch2(config-if)# switchport mode access
Switch2(config-if)# switchport access vlan 20
Switch2(config-if)# exit

! Set access port for IT-PC2
Switch2(config)# interface FastEthernet0/3
Switch2(config-if)# switchport mode access
Switch2(config-if)# switchport access vlan 20
Switch2(config-if)# exit

Switch2(config)# end
Switch2# write memory
```

---

### Switch 3 Configuration (Finance — VLAN 30)

Click on Switch3 → CLI tab → press Enter.

```
Switch> enable
Switch# configure terminal
Switch(config)# hostname Switch3

! Create VLAN 30
Switch3(config)# vlan 30
Switch3(config-vlan)# name Finance
Switch3(config-vlan)# exit

! Set trunk port (uplink to Router2 GigabitEthernet0/0)
Switch3(config)# interface FastEthernet0/1
Switch3(config-if)# switchport mode trunk
Switch3(config-if)# exit

! Set access port for Finance-PC1
Switch3(config)# interface FastEthernet0/2
Switch3(config-if)# switchport mode access
Switch3(config-if)# switchport access vlan 30
Switch3(config-if)# exit

! Set access port for Finance-PC2
Switch3(config)# interface FastEthernet0/3
Switch3(config-if)# switchport mode access
Switch3(config-if)# switchport access vlan 30
Switch3(config-if)# exit

Switch3(config)# end
Switch3# write memory
```

---

### Switch 4 Configuration (Sales — VLAN 40)

Click on Switch4 → CLI tab → press Enter.

```
Switch> enable
Switch# configure terminal
Switch(config)# hostname Switch4

! Create VLAN 40
Switch4(config)# vlan 40
Switch4(config-vlan)# name Sales
Switch4(config-vlan)# exit

! Set trunk port (uplink to Router2 GigabitEthernet0/2)
Switch4(config)# interface FastEthernet0/1
Switch4(config-if)# switchport mode trunk
Switch4(config-if)# exit

! Set access port for Sales-PC1
Switch4(config)# interface FastEthernet0/2
Switch4(config-if)# switchport mode access
Switch4(config-if)# switchport access vlan 40
Switch4(config-if)# exit

! Set access port for Sales-PC2
Switch4(config)# interface FastEthernet0/3
Switch4(config-if)# switchport mode access
Switch4(config-if)# switchport access vlan 40
Switch4(config-if)# exit

Switch4(config)# end
Switch4# write memory
```

---

### Switch 5 Configuration (Server Room — VLAN 50)

Click on Switch5 → CLI tab → press Enter.

```
Switch> enable
Switch# configure terminal
Switch(config)# hostname Switch5

! Create VLAN 50
Switch5(config)# vlan 50
Switch5(config-vlan)# name Servers
Switch5(config-vlan)# exit

! Set trunk port (uplink to Router5 GigabitEthernet0/2)
Switch5(config)# interface FastEthernet0/1
Switch5(config-if)# switchport mode trunk
Switch5(config-if)# exit

! Set access port for DHCP-Server
Switch5(config)# interface FastEthernet0/2
Switch5(config-if)# switchport mode access
Switch5(config-if)# switchport access vlan 50
Switch5(config-if)# exit

! Set access port for DNS-Server
Switch5(config)# interface FastEthernet0/3
Switch5(config-if)# switchport mode access
Switch5(config-if)# switchport access vlan 50
Switch5(config-if)# exit

! Set access port for Web-Server
Switch5(config)# interface FastEthernet0/4
Switch5(config-if)# switchport mode access
Switch5(config-if)# switchport access vlan 50
Switch5(config-if)# exit

! Set access port for Mail-Server
Switch5(config)# interface FastEthernet0/5
Switch5(config-if)# switchport mode access
Switch5(config-if)# switchport access vlan 50
Switch5(config-if)# exit

Switch5(config)# end
Switch5# write memory
```

---

## 7. DHCP Server Configuration

The DHCP server gives IP addresses to PCs automatically.
It has one pool for each department VLAN.

**Steps:**

1. Click on **DHCP-Server**.
2. Click the **"Desktop"** tab.
3. Click **"IP Configuration"**.
4. Set a static IP address:
   - IP Address: `192.168.50.10`
   - Subnet Mask: `255.255.255.0`
   - Default Gateway: `192.168.50.1`
   - DNS Server: `192.168.50.20`
5. Close the IP Configuration window.

**Configure DHCP pools:**

1. Click on **DHCP-Server**.
2. Click the **"Services"** tab.
3. Click **"DHCP"** on the left side.
4. Make sure **"On"** is selected at the top.

**Add a pool for each VLAN:**

Click the **"Add"** button after filling each pool.
You may need to delete the default pool named `serverPool` first.

**Pool 1 — HR (VLAN 10):**

| Field           | Value             |
|-----------------|-------------------|
| Pool Name       | HR-Pool           |
| Default Gateway | 192.168.10.1      |
| DNS Server      | 192.168.50.20     |
| Start IP Address| 192.168.10.100    |
| Subnet Mask     | 255.255.255.0     |
| Max Users       | 101               |

**Pool 2 — IT (VLAN 20):**

| Field           | Value             |
|-----------------|-------------------|
| Pool Name       | IT-Pool           |
| Default Gateway | 192.168.20.1      |
| DNS Server      | 192.168.50.20     |
| Start IP Address| 192.168.20.100    |
| Subnet Mask     | 255.255.255.0     |
| Max Users       | 101               |

**Pool 3 — Finance (VLAN 30):**

| Field           | Value             |
|-----------------|-------------------|
| Pool Name       | Finance-Pool      |
| Default Gateway | 192.168.30.1      |
| DNS Server      | 192.168.50.20     |
| Start IP Address| 192.168.30.100    |
| Subnet Mask     | 255.255.255.0     |
| Max Users       | 101               |

**Pool 4 — Sales (VLAN 40):**

| Field           | Value             |
|-----------------|-------------------|
| Pool Name       | Sales-Pool        |
| Default Gateway | 192.168.40.1      |
| DNS Server      | 192.168.50.20     |
| Start IP Address| 192.168.40.100    |
| Subnet Mask     | 255.255.255.0     |
| Max Users       | 101               |

---

## 8. DNS Server Configuration

The DNS server converts domain names (like `company.com`) to IP addresses.

**Steps:**

1. Click on **DNS-Server**.
2. Click the **"Desktop"** tab.
3. Click **"IP Configuration"**.
4. Set a static IP:
   - IP Address: `192.168.50.20`
   - Subnet Mask: `255.255.255.0`
   - Default Gateway: `192.168.50.1`
5. Close the window.

**Add DNS records:**

1. Click on **DNS-Server**.
2. Click the **"Services"** tab.
3. Click **"DNS"** on the left side.
4. Make sure **"On"** is selected.

**Add these records (click Add after each):**

| Name           | Type | Address        |
|----------------|------|----------------|
| www.company.com| A    | 192.168.50.30  |
| mail.company.com| A   | 192.168.50.40  |

---

## 9. Web Server Configuration

The Web server shows web pages when a user opens a browser.

**Steps:**

1. Click on **Web-Server**.
2. Click the **"Desktop"** tab.
3. Click **"IP Configuration"**.
4. Set a static IP:
   - IP Address: `192.168.50.30`
   - Subnet Mask: `255.255.255.0`
   - Default Gateway: `192.168.50.1`
   - DNS Server: `192.168.50.20`
5. Close the window.

**Enable HTTP:**

1. Click on **Web-Server**.
2. Click the **"Services"** tab.
3. Click **"HTTP"** on the left side.
4. Make sure **"On"** is selected next to HTTP.

---

## 10. Mail Server Configuration

The Mail server sends and receives email.

**Steps:**

1. Click on **Mail-Server**.
2. Click the **"Desktop"** tab.
3. Click **"IP Configuration"**.
4. Set a static IP:
   - IP Address: `192.168.50.40`
   - Subnet Mask: `255.255.255.0`
   - Default Gateway: `192.168.50.1`
   - DNS Server: `192.168.50.20`
5. Close the window.

**Configure mail service:**

1. Click on **Mail-Server**.
2. Click the **"Services"** tab.
3. Click **"EMAIL"** on the left side.
4. Set the domain name: `company.com`
5. Click **"Set"**.
6. Add a user: name = `admin`, password = `admin123`, click **"+"**.

---

## 11. PC Configuration

Each PC gets its IP address from DHCP automatically.
You only need to set the DNS server (or let DHCP set it).

**Steps for each PC:**

1. Click on the PC (for example, HR-PC1).
2. Click the **"Desktop"** tab.
3. Click **"IP Configuration"**.
4. Select **"DHCP"**.
5. Wait a few seconds. The PC will get an IP address automatically.
6. Check that:
   - IP Address is in the correct range (e.g., 192.168.10.100–200 for HR)
   - Default Gateway is correct (e.g., 192.168.10.1 for HR)

**Repeat for all 8 PCs.**

---

## 12. Testing and Verification

### Test 1: Check Router Interfaces

On each router, type this command to see all interfaces:

```
show ip interface brief
```

All interfaces should show **"up"** in both the Status and Protocol columns.
If a port says "down", check the cable and check the `no shutdown` command.

### Test 2: Check Routing Table

On each router, type:

```
show ip route
```

You should see routes with the letter **R** (learned by RIP) for all remote networks.
If you do not see RIP routes, wait 30 seconds. RIP needs time to share routes.

### Test 3: Ping from PC to Gateway

1. Click on HR-PC1.
2. Click **Desktop** tab → **Command Prompt**.
3. Type:

```
ping 192.168.10.1
```

You should see: `Reply from 192.168.10.1`

If you see `Request timed out`, check the switch configuration and router sub-interface.

### Test 4: Ping Between Departments

From HR-PC1, ping a PC in the Finance department:

```
ping 192.168.30.100
```

You should get a reply.
If not, check the RIP configuration on all routers.

### Test 5: Ping the DHCP Server

From any PC:

```
ping 192.168.50.10
```

You should get a reply.

### Test 6: Open a Web Page

1. Click on HR-PC1.
2. Click **Desktop** tab → **Web Browser**.
3. Type in the address bar: `http://www.company.com`
4. Press Enter.
5. You should see a web page.

### Test 7: Check DHCP

1. Click on HR-PC1 → Desktop → IP Configuration.
2. Select **DHCP**.
3. Check that the PC gets an IP address in the 192.168.10.100–200 range.

### Troubleshooting Tips

| Problem                              | Fix                                                             |
|--------------------------------------|-----------------------------------------------------------------|
| Interface is down                    | Type `no shutdown` on that interface                            |
| PC does not get IP from DHCP         | Check `ip helper-address` on the router sub-interface           |
| Cannot ping across routers           | Check RIP config — run `show ip route` on each router           |
| Trunk port not working               | Check `switchport mode trunk` on the switch uplink port         |
| Sub-interface not working            | Check `encapsulation dot1Q <vlan-id>` on the router sub-interface |
| Web page not loading                 | Check DNS and HTTP service on servers                           |

---

## 13. Bonus: Convert Router3 to Static Routing

### What is Static Routing?

Static routing means you manually tell the router the path to each network.
The router does not learn routes automatically from other routers.
You type each route yourself using the `ip route` command.

Static routing is good when:
- The network is small
- You want full control over the routing path
- You want to learn how routing works step by step

### Why Router3?

Router3 is the simplest router in this network.
It has only 2 connections:
- **Left side:** GigabitEthernet0/0 → Router1 (next-hop: `10.0.0.1`)
- **Right side:** GigabitEthernet0/1 → Router5 (next-hop: `10.0.2.2`)

It has no VLANs and no sub-interfaces.
Every packet either goes left (toward Router1) or right (toward Router5).
This makes static routes simple and clear.

### Static Route Table for Router3

| # | Destination Network | Subnet Mask       | Next-Hop IP | Direction | Explanation                     |
|---|--------------------|--------------------|-------------|-----------|----------------------------------|
| 1 | 192.168.10.0       | 255.255.255.0     | 10.0.0.1    | ← Left   | HR VLAN — via Router1           |
| 2 | 192.168.20.0       | 255.255.255.0     | 10.0.0.1    | ← Left   | IT VLAN — via Router1           |
| 3 | 192.168.30.0       | 255.255.255.0     | 10.0.2.2    | → Right  | Finance VLAN — via Router5      |
| 4 | 192.168.40.0       | 255.255.255.0     | 10.0.2.2    | → Right  | Sales VLAN — via Router5        |
| 5 | 192.168.50.0       | 255.255.255.0     | 10.0.2.2    | → Right  | Server Room VLAN — via Router5  |
| 6 | 10.0.1.0           | 255.255.255.252   | 10.0.2.2    | → Right  | R2↔R4 WAN link — via Router5   |
| 7 | 10.0.3.0           | 255.255.255.252   | 10.0.2.2    | → Right  | R4↔R5 WAN link — via Router5   |

### Step-by-Step Instructions

**Step 1:** Open the CLI.

Click on Router3 → click the **CLI** tab → press Enter.

**Step 2:** Enter configuration mode.

```
Router3> enable
Router3# configure terminal
```

**Step 3:** Remove RIP from Router3.

```
Router3(config)# no router rip
```

> This command removes all RIP configuration from Router3. After this, Router3 has no routing protocol. It only knows about its directly connected networks.

**Step 4:** Add all 7 static routes.

```
! Route to HR VLAN (via Router1)
Router3(config)# ip route 192.168.10.0 255.255.255.0 10.0.0.1

! Route to IT VLAN (via Router1)
Router3(config)# ip route 192.168.20.0 255.255.255.0 10.0.0.1

! Route to Finance VLAN (via Router5)
Router3(config)# ip route 192.168.30.0 255.255.255.0 10.0.2.2

! Route to Sales VLAN (via Router5)
Router3(config)# ip route 192.168.40.0 255.255.255.0 10.0.2.2

! Route to Server Room VLAN (via Router5)
Router3(config)# ip route 192.168.50.0 255.255.255.0 10.0.2.2

! Route to R2-R4 WAN link (via Router5)
Router3(config)# ip route 10.0.1.0 255.255.255.252 10.0.2.2

! Route to R4-R5 WAN link (via Router5)
Router3(config)# ip route 10.0.3.0 255.255.255.252 10.0.2.2
```

**Step 5:** Save the configuration.

```
Router3(config)# end
Router3# write memory
```

**Step 6:** Verify the routing table.

```
Router3# show ip route
```

You should see:
- Routes with the letter **S** — these are static routes (the ones you just added)
- Routes with the letter **C** — these are directly connected networks (`10.0.0.0/30` and `10.0.2.0/30`)
- You should NOT see any routes with the letter **R** (RIP routes are gone)

### Important Notes

> **Do NOT change Router1, Router2, Router4, or Router5.** They all keep using RIP. No changes needed.
>
> Router3's directly connected networks (`10.0.0.0/30` and `10.0.2.0/30`) are already advertised by Router1 and Router5 in their RIP configurations. So the rest of the network already knows how to reach Router3.

### Verification

1. Open **HR-PC1** → Desktop → Command Prompt.
2. Type: `ping 192.168.30.100` (Finance PC).
3. You should get a reply. This means the static routes on Router3 are working correctly.
4. Also try: `ping 192.168.50.10` (DHCP Server).
5. You should get a reply.

If the ping fails, check:
- Did you type `no router rip` on Router3?
- Did you add all 7 static routes?
- Did you save with `write memory`?
- Run `show ip route` on Router3 to check.

---

## 14. Bonus: Add a Default Route on Router4

### What is a Default Route?

A **default route** (also called the "gateway of last resort") is a special route that tells a router:
> "If you don't know where to send a packet, send it here."

Instead of adding a specific route for every remote network, you configure one default route that covers everything unknown.

In Cisco IOS, a default route looks like this:

```
ip route 0.0.0.0 0.0.0.0 <next-hop-ip>
```

The `0.0.0.0 0.0.0.0` means "match any destination" — it is the lowest-priority route and is only used when no more-specific route matches.

In the routing table, a default route appears as:
```
S*    0.0.0.0/0 [1/0] via <next-hop-ip>
```
The **star (\*)** means it is the gateway of last resort.

---

### Why Router4?

Router4 is a **transit router** with only 2 interfaces:
- `GigabitEthernet0/0` = `10.0.1.2/30` → connects to **Router2**
- `GigabitEthernet0/1` = `10.0.3.1/30` → connects to **Router5**

Router4 has only **one path to most of the network**: through Router5 (next-hop `10.0.3.2`).

The only exception is Router2's LANs — Finance VLAN (`192.168.30.0/24`) and Sales VLAN (`192.168.40.0/24`) — which are reachable directly via Router2 (next-hop `10.0.1.1`).

**Strategy:**
- Keep 2 **specific static routes** for Router2's LANs (Finance and Sales)
- Use one **default route** for everything else → Router5

This is more efficient than listing every remote network individually.

---

### Static Route Table for Router4

| # | Destination Network | Subnet Mask       | Next-Hop IP | Type    | Explanation                                    |
|---|--------------------|--------------------|-------------|---------|------------------------------------------------|
| 1 | 192.168.30.0       | 255.255.255.0     | 10.0.1.1    | Static  | Finance VLAN — via Router2                    |
| 2 | 192.168.40.0       | 255.255.255.0     | 10.0.1.1    | Static  | Sales VLAN — via Router2                      |
| 3 | 0.0.0.0            | 0.0.0.0           | 10.0.3.2    | Default | Everything else — via Router5                  |

---

### Step-by-Step Instructions

**Step 1:** Open the CLI.

Click on Router4 → click the **CLI** tab → press Enter.

**Step 2:** Enter configuration mode.

```
Router4> enable
Router4# configure terminal
```

**Step 3:** Remove RIP from Router4.

```
Router4(config)# no router rip
```

> This removes all RIP configuration from Router4. After this, Router4 has no routing protocol. It only knows about its directly connected networks (`10.0.1.0/30` and `10.0.3.0/30`).

**Step 4:** Add 2 specific static routes for Router2's LANs.

```
Router4(config)# ip route 192.168.30.0 255.255.255.0 10.0.1.1
Router4(config)# ip route 192.168.40.0 255.255.255.0 10.0.1.1
```

**Step 5:** Add the default route (for everything else → Router5).

```
Router4(config)# ip route 0.0.0.0 0.0.0.0 10.0.3.2
```

**Step 6:** Save the configuration.

```
Router4(config)# end
Router4# write memory
```

**Step 7:** Verify the routing table.

```
Router4# show ip route
```

You should see:
- `S*   0.0.0.0/0 [1/0] via 10.0.3.2` — the default route (the star means gateway of last resort)
- `S    192.168.30.0/24 [1/0] via 10.0.1.1` — specific static route for Finance
- `S    192.168.40.0/24 [1/0] via 10.0.1.1` — specific static route for Sales
- `C    10.0.1.0/30` and `C    10.0.3.0/30` — directly connected networks

---

### Verification

1. Open **HR-PC1** → Desktop → Command Prompt.
2. Type: `ping 192.168.30.100` (Finance PC). You should get a reply.
3. Type: `ping 192.168.40.100` (Sales PC). You should get a reply.
4. Type: `ping 192.168.50.10` (DHCP Server). You should get a reply.

If the ping fails, check:
- Did you type `no router rip` on Router4?
- Did you add both specific static routes and the default route?
- Did you save with `write memory`?
- Run `show ip route` on Router4 to check.

---

### Important Notes

> **Do NOT change Router1, Router2, Router3, or Router5.** Only Router4 is modified in this section.
>
> Router4's directly connected networks (`10.0.1.0/30` and `10.0.3.0/30`) are already known by adjacent routers. The default route ensures that all other traffic is forwarded to Router5, which has full knowledge of the network through RIP.

---

## 15. Live Presentation & Demo Guide

*(شرح المشروع)*

This guide helps you present the project to your class or instructor. Each part tells you what to do, which commands to run, and what to say. Arabic translation hints are included to help you explain in Arabic if needed.

**Estimated total time: ~15 minutes**

---

### Part 1: Opening (30 seconds)

**What to do:** Point to the topology diagram in Packet Tracer.

**What to say:**
> "This is a company network with 4 departments, 5 routers, 5 switches, 4 servers, and 8 PCs."

*(بالعربي: "هذه شبكة شركة تحتوي على 4 أقسام، 5 راوترات، 5 سويتشات، 4 سيرفرات، و8 أجهزة كمبيوتر.")*

> "The network uses a chain design: Router1 and Router2 connect the departments, and they link through Router3 and Router4 to Router5, which connects the servers."

*(بالعربي: "الشبكة مصممة على شكل سلسلة: Router1 وRouter2 يوصلان الأقسام، وهما متصلان عبر Router3 وRouter4 بـ Router5 الذي يوصل السيرفرات.")*

---

### Part 2: Show the Physical Topology (1 minute)

**What to do:** Click on each device and show its type.

**What to say:**
> "These are Cisco 2911 routers. They handle routing between networks."

*(بالعربي: "هذه راوترات Cisco 2911. تقوم بتوجيه البيانات بين الشبكات.")*

> "These are Cisco 2960 switches. They connect the PCs inside each department."

*(بالعربي: "هذه سويتشات Cisco 2960. تربط الأجهزة داخل كل قسم.")*

> "The cables between routers are crossover cables. The cables from routers to switches are straight-through cables."

*(بالعربي: "الكابلات بين الراوترات كروس-أوفر. الكابلات من الراوتر إلى السويتش ستريت-ثرو.")*

---

### Part 3: Explain VLANs (2 minutes)

**What to do:** Click on Switch1 → CLI tab.

**Commands to run:**
```
Switch1# show vlan brief
```

**What to say:**
> "Each department is on a separate VLAN for security and organization."

*(بالعربي: "كل قسم على VLAN منفصل للأمان والتنظيم.")*

- VLAN 10 = HR
- VLAN 20 = IT
- VLAN 30 = Finance
- VLAN 40 = Sales
- VLAN 50 = Servers

**Then run:**
```
Switch1# show interfaces trunk
```

**What to say:**
> "The trunk port carries traffic for all VLANs between the switch and router. One cable, multiple VLANs."

*(بالعربي: "البورت الترانك يحمل كل الـVLANs على كابل واحد بين السويتش والراوتر.")*

---

### Part 4: Explain Router-on-a-Stick (1 minute)

**What to do:** Click on Router1 → CLI tab.

**Commands to run:**
```
Router1# show ip interface brief
```

**What to say:**
> "Router-on-a-stick means one physical cable carries multiple VLANs using sub-interfaces."

*(بالعربي: "Router-on-a-stick يعني كابل واحد يحمل عدة VLANs باستخدام Sub-interfaces.")*

> "GigabitEthernet0/0.10 handles VLAN 10 (HR), and GigabitEthernet0/2.20 handles VLAN 20 (IT)."

> "Each sub-interface uses encapsulation dot1Q to tag the VLAN traffic."

*(بالعربي: "كل Sub-interface يستخدم dot1Q لتمييز الـVLAN.")*

---

### Part 5: Explain IP Addressing (1 minute)

**What to say:**
> "LAN networks use /24 subnets — that gives 254 usable addresses for PCs."

*(بالعربي: "شبكات الـLAN تستخدم /24 — توفر 254 عنوان للأجهزة.")*

> "WAN links between routers use /30 subnets — that gives only 2 usable addresses, which is exactly what we need for a point-to-point link."

*(بالعربي: "الـWAN بين الراوترات تستخدم /30 — توفر عنوانين فقط لأن الـWAN نقطة لنقطة.")*

> "The servers have static IPs in the 192.168.50.x range."

*(بالعربي: "السيرفرات لها عناوين ثابتة في نطاق 192.168.50.x.")*

---

### Part 6: Show RIP Routing (2 minutes)

**What to do:** Click on Router1 → CLI tab.

**Commands to run:**
```
Router1# show ip route
```

**What to say:**
> "The letter R means the route was learned by RIP — the router discovered it automatically."

*(بالعربي: "الحرف R يعني أن الراوتر تعلّم هذا المسار تلقائياً عبر بروتوكول RIP.")*

> "Router1 can see remote networks: 192.168.30.0 (Finance), 192.168.40.0 (Sales), 192.168.50.0 (Servers) — even though it is not directly connected to them."

**Then run:**
```
Router1# show ip rip database
```

**What to say:**
> "RIP version 2 supports subnet masks and is simple to configure. It shares routing information with neighboring routers every 30 seconds."

*(بالعربي: "RIP نسخة 2 يدعم الـSubnet Masks وسهل الإعداد. يشارك معلومات التوجيه مع الراوترات المجاورة كل 30 ثانية.")*

---

### Part 7: Show Static Routing on Router3 (1 minute)

> **Note:** This step applies only if you completed Section 13 (converting Router3 to static routes). If Router3 still uses RIP, skip this part.

**What to do:** Click on Router3 → CLI tab.

**Commands to run:**
```
Router3# show ip route
```

**What to say:**
> "The letter S means static route — I configured each route manually instead of using a routing protocol."

*(بالعربي: "الحرف S يعني مسار ثابت — قمت بإعداده يدوياً بدلاً من استخدام بروتوكول توجيه.")*

> "There are no R routes on Router3 — it does not use RIP. All routes were added by hand."

*(بالعربي: "لا يوجد حرف R على Router3 — لا يستخدم RIP. كل المسارات تم إضافتها يدوياً.")*

---

### Part 8: Show Default Route on Router4 (1 minute)

> **Note:** This step applies only if you completed Section 14 (adding a default route on Router4). If Router4 still uses RIP, skip this part.

**What to do:** Click on Router4 → CLI tab.

**Commands to run:**
```
Router4# show ip route
```

**What to say:**
> "S* means default route — the gateway of last resort."

*(بالعربي: "S* تعني المسار الافتراضي — البوابة الأخيرة.")*

> "Instead of adding a route for every network, one default route sends all unknown traffic to Router5. It is simpler and more scalable."

*(بالعربي: "بدلاً من إضافة مسار لكل شبكة، مسار افتراضي واحد يرسل كل حركة المرور غير المعروفة إلى Router5.")*

---

### Part 9: Show DHCP Working (1 minute)

**What to do:** Click on HR-PC1 → Desktop tab → IP Configuration.

1. Select **DHCP**.
2. Wait a few seconds.
3. Show that the PC received an IP address automatically (e.g., `192.168.10.100`).

**What to say:**
> "DHCP gives IP addresses automatically. The DHCP server is on VLAN 50, but it serves all VLANs because the routers use ip helper-address to forward DHCP requests."

*(بالعربي: "DHCP يعطي عناوين IP تلقائياً. سيرفر الـDHCP في VLAN 50 لكنه يخدم كل الـVLANs لأن الراوترات تستخدم ip helper-address لإعادة توجيه الطلبات.")*

---

### Part 10: Show DNS Working (1 minute)

**What to do:** Click on any PC → Desktop → Command Prompt.

**Command to run:**
```
nslookup www.company.com
```

**What to say:**
> "The DNS server translates the domain name www.company.com into the IP address 192.168.50.30. Without DNS, you would have to remember IP addresses instead of names."

*(بالعربي: "سيرفر DNS يترجم اسم النطاق www.company.com إلى عنوان IP 192.168.50.30.")*

---

### Part 11: Show Web Server Working (30 seconds)

**What to do:** Click on any PC → Desktop → Web Browser.

**Type in the address bar:**
```
http://www.company.com
```

**What to say:**
> "The PC asks the DNS server for the IP, then connects to the web server. The web page loads successfully."

*(بالعربي: "الجهاز يسأل DNS عن الـIP ثم يتصل بسيرفر الويب. الصفحة تظهر بنجاح.")*

---

### Part 12: Show Email Server Working (30 seconds)

**What to do:** Click on Mail-Server → Services tab → EMAIL.

**What to show:**
- Domain: `company.com`
- User account: `admin` (or similar)

**What to say:**
> "The mail server uses SMTP to send emails and POP3 to receive them. The domain is company.com."

*(بالعربي: "سيرفر البريد يستخدم SMTP للإرسال وPOP3 للاستقبال. النطاق هو company.com.")*

---

### Part 13: Test Connectivity Between Departments (2 minutes)

**This is the most important part of the demo.**

**What to do:** Click on HR-PC1 → Desktop → Command Prompt.

**Run these ping commands:**

```
ping 192.168.10.1
```
> Expected: Success — this is the gateway (Router1).

*(بالعربي: "هذا البينج للـGateway — يجب أن ينجح.")*

```
ping 192.168.20.100
```
> Expected: Success — this is an IT PC.

*(بالعربي: "بينج لجهاز في قسم IT.")*

```
ping 192.168.30.100
```
> Expected: Success — this is a Finance PC.

*(بالعربي: "بينج لجهاز في قسم المالية.")*

```
ping 192.168.50.10
```
> Expected: Success — this is the DHCP server.

*(بالعربي: "بينج لسيرفر الـDHCP.")*

```
ping 192.168.50.30
```
> Expected: Success — this is the Web server.

*(بالعربي: "بينج لسيرفر الويب.")*

**What to say:**
> "All departments can communicate with each other and with the servers. The VLANs provide logical separation, and the routers connect them all together."

*(بالعربي: "كل الأقسام تستطيع التواصل مع بعضها ومع السيرفرات. الـVLANs توفر الفصل المنطقي، والراوترات تربطهم جميعاً.")*

---

### Part 14: Summary Statement (30 seconds)

**What to say:**
> "This project demonstrates:
> - VLANs for network segmentation
> - Router-on-a-stick for inter-VLAN routing
> - Three types of routing: RIP (dynamic), Static Routes, and Default Route
> - DHCP for automatic IP assignment with relay across VLANs
> - DNS for name resolution
> - Web and Mail server services
> - Full connectivity between all departments and servers"

*(بالعربي: "هذا المشروع يوضح: الـVLANs لتقسيم الشبكة، الـRouter-on-a-stick للتوجيه بين الـVLANs، ثلاثة أنواع من التوجيه (RIP الديناميكي، والمسارات الثابتة، والمسار الافتراضي)، الـDHCP للـIPs التلقائية، الـDNS لترجمة الأسماء، وسيرفرات الويب والبريد، وتواصل كامل بين جميع الأقسام والسيرفرات.")*

---

### Quick-Reference Checklist

| Step | What to Show | Command / Action | What to Say |
|------|-------------|-----------------|-------------|
| 1 | Topology | Point to diagram | "Company network, 4 departments..." |
| 2 | VLANs | `show vlan brief` on Switch1 | "Each dept has its own VLAN" |
| 3 | Trunk | `show interfaces trunk` on Switch1 | "Trunk carries all VLANs" |
| 4 | Router-on-a-Stick | `show ip interface brief` on Router1 | "Sub-interfaces for each VLAN" |
| 5 | RIP | `show ip route` on Router1 | "R = learned by RIP" |
| 6 | Static Routes | `show ip route` on Router3 | "S = manual static route" |
| 7 | Default Route | `show ip route` on Router4 | "S* = default route" |
| 8 | DHCP | IP Config on HR-PC1 | "Auto IP via DHCP relay" |
| 9 | DNS | `nslookup www.company.com` | "DNS resolves names" |
| 10 | Web | Browser → www.company.com | "Web server works" |
| 11 | Ping Test | `ping` from HR-PC1 | "Full connectivity proven" |

---

*End of document*
