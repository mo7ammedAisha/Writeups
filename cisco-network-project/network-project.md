# Cisco Packet Tracer Company Network Project

## Introduction

This document explains how to build a company network using Cisco Packet Tracer.
The network has 5 routers, 5 switches, and several servers.
It uses VLANs, RIP routing, DHCP, DNS, Web, and Mail services.

Follow each step carefully. You will be able to finish the whole project using only this guide.

---

## Table of Contents

1. [Network Topology Overview](#1-network-topology-overview)
2. [IP Addressing Scheme](#2-ip-addressing-scheme)
3. [VLAN Plan](#3-vlan-plan)
4. [Router Configuration (RIP)](#4-router-configuration-rip)
5. [Switch Configuration](#5-switch-configuration)
6. [DHCP Server Configuration](#6-dhcp-server-configuration)
7. [DNS Server Configuration](#7-dns-server-configuration)
8. [Web Server Configuration](#8-web-server-configuration)
9. [Mail Server Configuration](#9-mail-server-configuration)
10. [Testing and Verification](#10-testing-and-verification)

---

## 1. Network Topology Overview

### About the Company

The company has four departments:

| Department | VLAN |
|------------|------|
| HR         | 10   |
| IT         | 20   |
| Finance    | 30   |
| Sales      | 40   |
| Server Room| 50   |

Each department is on a separate VLAN.
All departments share the internet through the routers.

### Devices Used

| Device   | Count | Purpose                        |
|----------|-------|--------------------------------|
| Routers  | 5     | Connect networks, RIP routing  |
| Switches | 5     | Connect PCs within departments |
| Servers  | 4     | DHCP, DNS, Web, Mail           |
| PCs      | 8+    | End user devices               |

### ASCII Network Topology Diagram

```
                             +-------------------+
                             |     Router5       |
                             |  (Core Router)    |
                             | Fa0/0: 10.0.0.1   |
                             | Fa0/1: 10.0.1.1   |
                             | Fa1/0: 10.0.2.1   |
                             | Fa1/1: 10.0.3.1   |
                             | Fa2/0.50:         |
                             | 192.168.50.1/24   |
                             +---+---+---+---+---+
                               /  |       |  \
             10.0.0.0/30 ------   |       |   ------- 10.0.1.0/30
           10.0.2.0/30 --------   |       |   ------- 10.0.3.0/30
                                  |       |
                    +-------------+       +--------------+
                    |                                    |
         +----------+----------+             +-----------+---------+
         |       Router1       |             |       Router2       |
         | WAN: 10.0.0.2/30    |             | WAN: 10.0.1.2/30    |
         | Fa0/0.10:           |             | Fa0/0.30:           |
         |   192.168.10.1/24   |             |   192.168.30.1/24   |
         | Fa0/0.20:           |             | Fa0/0.40:           |
         |   192.168.20.1/24   |             |   192.168.40.1/24   |
         +----+----------+-----+             +------+----------+---+
              |          |                          |          |
   +----------+--+  +----+---------+     +----------+--+  +---+----------+
   |  Switch1    |  |  Switch2     |     |  Switch3    |  |  Switch4     |
   |  VLAN 10    |  |  VLAN 20     |     |  VLAN 30    |  |  VLAN 40     |
   |   (HR)      |  |   (IT)       |     |  (Finance)  |  |   (Sales)    |
   +------+------+  +------+-------+     +------+------+  +------+-------+
          |                |                    |                 |
   [HR PCs x2]       [IT PCs x2]         [Finance PCs x2]  [Sales PCs x2]

         +----------+----------+
         |       Router3       |
         | WAN: 10.0.2.2/30    |  <-- Extra router (redundancy)
         | (no LAN subnet)     |
         +---------------------+

         +----------+----------+
         |       Router4       |
         | WAN: 10.0.3.2/30    |  <-- Extra router (redundancy)
         | (no LAN subnet)     |
         +---------------------+

                             +-------------------+
                             |     Switch5       |
                             |  VLAN 50 (Servers)|
                             +---+---+---+---+---+
                                 |   |   |   |
               +-----------------+   |   |   +-----------------+
               |           +-----+   +---+---------+           |
    +----------+--+  +------+------+  +------+------+  +-------+------+
    | DHCP Server |  | DNS Server  |  | Web Server  |  | Mail Server  |
    | 192.168.50.10| |192.168.50.20|  |192.168.50.30|  |192.168.50.40 |
    +-------------+  +-------------+  +-------------+  +--------------+
```

**Note:** Router5 is the core router. It connects to all other routers via /30 WAN links. Router1 is the gateway for HR (VLAN 10) and IT (VLAN 20). Router2 is the gateway for Finance (VLAN 30) and Sales (VLAN 40). Router3 and Router4 are extra routers that add redundancy. Router5 is also the gateway for the Server Room (VLAN 50) via Switch5.

---

## 2. IP Addressing Scheme

### VLAN and Subnet Table

| VLAN ID | VLAN Name   | Subnet           | Gateway       | DHCP Range                  |
|---------|-------------|------------------|---------------|-----------------------------|
| 10      | HR          | 192.168.10.0/24  | 192.168.10.1  | 192.168.10.100 – .200       |
| 20      | IT          | 192.168.20.0/24  | 192.168.20.1  | 192.168.20.100 – .200       |
| 30      | Finance     | 192.168.30.0/24  | 192.168.30.1  | 192.168.30.100 – .200       |
| 40      | Sales       | 192.168.40.0/24  | 192.168.40.1  | 192.168.40.100 – .200       |
| 50      | Servers     | 192.168.50.0/24  | 192.168.50.1  | Static only (no DHCP)       |
| 1       | Native      | —                | —             | Default/management VLAN     |

### Router WAN (Point-to-Point) Links

| Link             | Network        | Router5 End   | Other Router End |
|------------------|----------------|---------------|------------------|
| R5 ↔ R1          | 10.0.0.0/30    | 10.0.0.1      | 10.0.0.2         |
| R5 ↔ R2          | 10.0.1.0/30    | 10.0.1.1      | 10.0.1.2         |
| R5 ↔ R3          | 10.0.2.0/30    | 10.0.2.1      | 10.0.2.2         |
| R5 ↔ R4          | 10.0.3.0/30    | 10.0.3.1      | 10.0.3.2         |

### Server Static IP Addresses

| Server      | IP Address      | VLAN | Service         |
|-------------|-----------------|------|-----------------|
| DHCP Server | 192.168.50.10   | 50   | DHCP            |
| DNS Server  | 192.168.50.20   | 50   | DNS             |
| Web Server  | 192.168.50.30   | 50   | HTTP            |
| Mail Server | 192.168.50.40   | 50   | SMTP / POP3     |

### Router LAN Interface IPs (Sub-interfaces for Router-on-a-Stick)

| Router | Interface        | VLAN | IP Address       |
|--------|------------------|------|------------------|
| R1     | Fa0/0.10         | 10   | 192.168.10.1/24  |
| R1     | Fa0/0.20         | 20   | 192.168.20.1/24  |
| R2     | Fa0/0.30         | 30   | 192.168.30.1/24  |
| R2     | Fa0/0.40         | 40   | 192.168.40.1/24  |
| R5     | Fa2/0.50         | 50   | 192.168.50.1/24  |

**Note:** R1 handles HR (VLAN 10) and IT (VLAN 20). R2 handles Finance (VLAN 30) and Sales (VLAN 40). R3 and R4 are extra routers — they only have WAN interfaces and add redundancy. R5 handles the Server Room (VLAN 50) and connects all routers.

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

#### Switch1 — HR Department

| Port   | Mode   | VLAN | Connected To         |
|--------|--------|------|----------------------|
| Fa0/1  | Trunk  | All  | Router1 (uplink)     |
| Fa0/2  | Access | 10   | HR-PC1               |
| Fa0/3  | Access | 10   | HR-PC2               |
| Fa0/24 | Trunk  | All  | Switch2 (if needed)  |

#### Switch2 — IT Department

| Port   | Mode   | VLAN | Connected To         |
|--------|--------|------|----------------------|
| Fa0/1  | Trunk  | All  | Router1 (uplink)     |
| Fa0/2  | Access | 20   | IT-PC1               |
| Fa0/3  | Access | 20   | IT-PC2               |

#### Switch3 — Finance Department

| Port   | Mode   | VLAN | Connected To         |
|--------|--------|------|----------------------|
| Fa0/1  | Trunk  | All  | Router2 (uplink)     |
| Fa0/2  | Access | 30   | Finance-PC1          |
| Fa0/3  | Access | 30   | Finance-PC2          |

#### Switch4 — Sales Department

| Port   | Mode   | VLAN | Connected To         |
|--------|--------|------|----------------------|
| Fa0/1  | Trunk  | All  | Router2 (uplink)     |
| Fa0/2  | Access | 40   | Sales-PC1            |
| Fa0/3  | Access | 40   | Sales-PC2            |

#### Switch5 — Server Room

| Port   | Mode   | VLAN | Connected To         |
|--------|--------|------|----------------------|
| Fa0/1  | Trunk  | All  | Router5 (uplink)     |
| Fa0/2  | Access | 50   | DHCP-Server          |
| Fa0/3  | Access | 50   | DNS-Server           |
| Fa0/4  | Access | 50   | Web-Server           |
| Fa0/5  | Access | 50   | Mail-Server          |

### Trunk Configuration

Trunk ports carry all VLANs between switches and routers.
Native VLAN is VLAN 1 on all trunk ports.

---

## 4. Router Configuration (RIP)

This section explains how to configure each router.
We use RIP version 2 for dynamic routing.
We use sub-interfaces (router-on-a-stick) for inter-VLAN routing.

### What is RIP?

RIP stands for Routing Information Protocol.
It is a simple routing protocol.
Each router tells its neighbors about its networks.
Version 2 supports subnet masks (classless).

### What is Router-on-a-Stick?

One physical interface connects to a trunk switch port.
The router uses sub-interfaces (Fa0/0.10, Fa0/0.20, etc.).
Each sub-interface handles one VLAN.
This allows inter-VLAN routing with one cable.

---

### Router 1 Configuration (HR and IT Gateway)

Router1 connects to Switch1 (HR) and Switch2 (IT).
It also connects to Router5 (core).

**Step 1:** Set the hostname.

```
Router> enable
Router# configure terminal
Router(config)# hostname Router1
```

**Step 2:** Configure the WAN interface to Router5.

```
Router1(config)# interface FastEthernet0/1
Router1(config-if)# ip address 10.0.0.2 255.255.255.252
Router1(config-if)# no shutdown
Router1(config-if)# exit
```

**Step 3:** Configure the LAN interface as a trunk (sub-interfaces).

```
Router1(config)# interface FastEthernet0/0
Router1(config-if)# no shutdown
Router1(config-if)# exit

! Sub-interface for HR (VLAN 10)
Router1(config)# interface FastEthernet0/0.10
Router1(config-subif)# encapsulation dot1Q 10
Router1(config-subif)# ip address 192.168.10.1 255.255.255.0
Router1(config-subif)# exit

! Sub-interface for IT (VLAN 20)
Router1(config)# interface FastEthernet0/0.20
Router1(config-subif)# encapsulation dot1Q 20
Router1(config-subif)# ip address 192.168.20.1 255.255.255.0
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

**Step 5:** Save the configuration.

```
Router1# write memory
```

---

### Router 2 Configuration (Finance and Sales Gateway)

Router2 connects to Switch3 (Finance) and Switch4 (Sales).
It also connects to Router5 (core).

```
Router> enable
Router# configure terminal
Router(config)# hostname Router2

! WAN interface to Router5
Router2(config)# interface FastEthernet0/1
Router2(config-if)# ip address 10.0.1.2 255.255.255.252
Router2(config-if)# no shutdown
Router2(config-if)# exit

! Enable the LAN interface
Router2(config)# interface FastEthernet0/0
Router2(config-if)# no shutdown
Router2(config-if)# exit

! Sub-interface for Finance (VLAN 30)
Router2(config)# interface FastEthernet0/0.30
Router2(config-subif)# encapsulation dot1Q 30
Router2(config-subif)# ip address 192.168.30.1 255.255.255.0
Router2(config-subif)# exit

! Sub-interface for Sales (VLAN 40)
Router2(config)# interface FastEthernet0/0.40
Router2(config-subif)# encapsulation dot1Q 40
Router2(config-subif)# ip address 192.168.40.1 255.255.255.0
Router2(config-subif)# exit

! RIP routing
Router2(config)# router rip
Router2(config-router)# version 2
Router2(config-router)# no auto-summary
Router2(config-router)# network 10.0.1.0
Router2(config-router)# network 192.168.30.0
Router2(config-router)# network 192.168.40.0
Router2(config-router)# exit

Router2# write memory
```

---

### Router 3 Configuration (Backup/Extra Finance Router)

Router3 provides an extra path to the Finance network.
It connects to Router5 and can serve as a backup.

```
Router> enable
Router# configure terminal
Router(config)# hostname Router3

! WAN interface to Router5
Router3(config)# interface FastEthernet0/1
Router3(config-if)# ip address 10.0.2.2 255.255.255.252
Router3(config-if)# no shutdown
Router3(config-if)# exit

! RIP routing (advertise the WAN link)
Router3(config)# router rip
Router3(config-router)# version 2
Router3(config-router)# no auto-summary
Router3(config-router)# network 10.0.2.0
Router3(config-router)# exit

Router3# write memory
```

---

### Router 4 Configuration (Backup/Extra Sales Router)

Router4 provides an extra path to the Sales network.

```
Router> enable
Router# configure terminal
Router(config)# hostname Router4

! WAN interface to Router5
Router4(config)# interface FastEthernet0/1
Router4(config-if)# ip address 10.0.3.2 255.255.255.252
Router4(config-if)# no shutdown
Router4(config-if)# exit

! RIP routing
Router4(config)# router rip
Router4(config-router)# version 2
Router4(config-router)# no auto-summary
Router4(config-router)# network 10.0.3.0
Router4(config-router)# exit

Router4# write memory
```

---

### Router 5 Configuration (Core Router)

Router5 is the main router.
It connects to all other routers and to the Server Room.

```
Router> enable
Router# configure terminal
Router(config)# hostname Router5

! Interface to Router1
Router5(config)# interface FastEthernet0/0
Router5(config-if)# ip address 10.0.0.1 255.255.255.252
Router5(config-if)# no shutdown
Router5(config-if)# exit

! Interface to Router2
Router5(config)# interface FastEthernet0/1
Router5(config-if)# ip address 10.0.1.1 255.255.255.252
Router5(config-if)# no shutdown
Router5(config-if)# exit

! Interface to Router3
Router5(config)# interface FastEthernet1/0
Router5(config-if)# ip address 10.0.2.1 255.255.255.252
Router5(config-if)# no shutdown
Router5(config-if)# exit

! Interface to Router4
Router5(config)# interface FastEthernet1/1
Router5(config-if)# ip address 10.0.3.1 255.255.255.252
Router5(config-if)# no shutdown
Router5(config-if)# exit

! Sub-interface for Server Room (VLAN 50)
Router5(config)# interface FastEthernet2/0
Router5(config-if)# no shutdown
Router5(config-if)# exit

Router5(config)# interface FastEthernet2/0.50
Router5(config-subif)# encapsulation dot1Q 50
Router5(config-subif)# ip address 192.168.50.1 255.255.255.0
Router5(config-subif)# exit

! RIP routing — advertise all networks
Router5(config)# router rip
Router5(config-router)# version 2
Router5(config-router)# no auto-summary
Router5(config-router)# network 10.0.0.0
Router5(config-router)# network 10.0.1.0
Router5(config-router)# network 10.0.2.0
Router5(config-router)# network 10.0.3.0
Router5(config-router)# network 192.168.50.0
Router5(config-router)# exit

Router5# write memory
```

---

## 5. Switch Configuration

This section explains how to configure each switch.
We create VLANs, assign access ports, and configure trunk ports.

### Switch 1 — HR Department

**Step 1:** Set the hostname and create VLAN 10.

```
Switch> enable
Switch# configure terminal
Switch(config)# hostname Switch1

! Create VLAN 10 for HR
Switch1(config)# vlan 10
Switch1(config-vlan)# name HR
Switch1(config-vlan)# exit
```

**Step 2:** Assign access ports to HR PCs.

```
! Port for HR-PC1
Switch1(config)# interface FastEthernet0/2
Switch1(config-if)# switchport mode access
Switch1(config-if)# switchport access vlan 10
Switch1(config-if)# exit

! Port for HR-PC2
Switch1(config)# interface FastEthernet0/3
Switch1(config-if)# switchport mode access
Switch1(config-if)# switchport access vlan 10
Switch1(config-if)# exit
```

**Step 3:** Configure the trunk port to Router1.

```
Switch1(config)# interface FastEthernet0/1
Switch1(config-if)# switchport mode trunk
Switch1(config-if)# switchport trunk native vlan 1
Switch1(config-if)# exit

Switch1# write memory
```

---

### Switch 2 — IT Department

```
Switch> enable
Switch# configure terminal
Switch(config)# hostname Switch2

! Create VLAN 20 for IT
Switch2(config)# vlan 20
Switch2(config-vlan)# name IT
Switch2(config-vlan)# exit

! Port for IT-PC1
Switch2(config)# interface FastEthernet0/2
Switch2(config-if)# switchport mode access
Switch2(config-if)# switchport access vlan 20
Switch2(config-if)# exit

! Port for IT-PC2
Switch2(config)# interface FastEthernet0/3
Switch2(config-if)# switchport mode access
Switch2(config-if)# switchport access vlan 20
Switch2(config-if)# exit

! Trunk port to Router1
Switch2(config)# interface FastEthernet0/1
Switch2(config-if)# switchport mode trunk
Switch2(config-if)# switchport trunk native vlan 1
Switch2(config-if)# exit

Switch2# write memory
```

---

### Switch 3 — Finance Department

```
Switch> enable
Switch# configure terminal
Switch(config)# hostname Switch3

! Create VLAN 30 for Finance
Switch3(config)# vlan 30
Switch3(config-vlan)# name Finance
Switch3(config-vlan)# exit

! Port for Finance-PC1
Switch3(config)# interface FastEthernet0/2
Switch3(config-if)# switchport mode access
Switch3(config-if)# switchport access vlan 30
Switch3(config-if)# exit

! Port for Finance-PC2
Switch3(config)# interface FastEthernet0/3
Switch3(config-if)# switchport mode access
Switch3(config-if)# switchport access vlan 30
Switch3(config-if)# exit

! Trunk port to Router2
Switch3(config)# interface FastEthernet0/1
Switch3(config-if)# switchport mode trunk
Switch3(config-if)# switchport trunk native vlan 1
Switch3(config-if)# exit

Switch3# write memory
```

---

### Switch 4 — Sales Department

```
Switch> enable
Switch# configure terminal
Switch(config)# hostname Switch4

! Create VLAN 40 for Sales
Switch4(config)# vlan 40
Switch4(config-vlan)# name Sales
Switch4(config-vlan)# exit

! Port for Sales-PC1
Switch4(config)# interface FastEthernet0/2
Switch4(config-if)# switchport mode access
Switch4(config-if)# switchport access vlan 40
Switch4(config-if)# exit

! Port for Sales-PC2
Switch4(config)# interface FastEthernet0/3
Switch4(config-if)# switchport mode access
Switch4(config-if)# switchport access vlan 40
Switch4(config-if)# exit

! Trunk port to Router2
Switch4(config)# interface FastEthernet0/1
Switch4(config-if)# switchport mode trunk
Switch4(config-if)# switchport trunk native vlan 1
Switch4(config-if)# exit

Switch4# write memory
```

---

### Switch 5 — Server Room

```
Switch> enable
Switch# configure terminal
Switch(config)# hostname Switch5

! Create VLAN 50 for Servers
Switch5(config)# vlan 50
Switch5(config-vlan)# name Servers
Switch5(config-vlan)# exit

! Port for DHCP Server
Switch5(config)# interface FastEthernet0/2
Switch5(config-if)# switchport mode access
Switch5(config-if)# switchport access vlan 50
Switch5(config-if)# exit

! Port for DNS Server
Switch5(config)# interface FastEthernet0/3
Switch5(config-if)# switchport mode access
Switch5(config-if)# switchport access vlan 50
Switch5(config-if)# exit

! Port for Web Server
Switch5(config)# interface FastEthernet0/4
Switch5(config-if)# switchport mode access
Switch5(config-if)# switchport access vlan 50
Switch5(config-if)# exit

! Port for Mail Server
Switch5(config)# interface FastEthernet0/5
Switch5(config-if)# switchport mode access
Switch5(config-if)# switchport access vlan 50
Switch5(config-if)# exit

! Trunk port to Router5
Switch5(config)# interface FastEthernet0/1
Switch5(config-if)# switchport mode trunk
Switch5(config-if)# switchport trunk native vlan 1
Switch5(config-if)# exit

Switch5# write memory
```

---

## 6. DHCP Server Configuration

We use a dedicated DHCP server in the Server Room.
The DHCP server gives IP addresses to all department PCs.

### Step 1: Set Static IP on DHCP Server

In Packet Tracer:
1. Click on the DHCP Server device.
2. Click the **Desktop** tab.
3. Click **IP Configuration**.
4. Enter these values:
   - IP Address: `192.168.50.10`
   - Subnet Mask: `255.255.255.0`
   - Default Gateway: `192.168.50.1`
   - DNS Server: `192.168.50.20`
5. Click **Save**.

### Step 2: Configure DHCP Pools

1. Click on the DHCP Server device.
2. Click the **Services** tab.
3. Click **DHCP** on the left menu.
4. Turn DHCP service **ON**.

Create one pool for each department:

#### Pool for HR (VLAN 10)

| Field            | Value              |
|------------------|--------------------|
| Pool Name        | HR-POOL            |
| Default Gateway  | 192.168.10.1       |
| DNS Server       | 192.168.50.20      |
| Start IP Address | 192.168.10.100     |
| Subnet Mask      | 255.255.255.0      |
| Max Users        | 100                |

Click **Add** to save the pool.

#### Pool for IT (VLAN 20)

| Field            | Value              |
|------------------|--------------------|
| Pool Name        | IT-POOL            |
| Default Gateway  | 192.168.20.1       |
| DNS Server       | 192.168.50.20      |
| Start IP Address | 192.168.20.100     |
| Subnet Mask      | 255.255.255.0      |
| Max Users        | 100                |

Click **Add** to save the pool.

#### Pool for Finance (VLAN 30)

| Field            | Value              |
|------------------|--------------------|
| Pool Name        | FINANCE-POOL       |
| Default Gateway  | 192.168.30.1       |
| DNS Server       | 192.168.50.20      |
| Start IP Address | 192.168.30.100     |
| Subnet Mask      | 255.255.255.0      |
| Max Users        | 100                |

Click **Add** to save the pool.

#### Pool for Sales (VLAN 40)

| Field            | Value              |
|------------------|--------------------|
| Pool Name        | SALES-POOL         |
| Default Gateway  | 192.168.40.1       |
| DNS Server       | 192.168.50.20      |
| Start IP Address | 192.168.40.100     |
| Subnet Mask      | 255.255.255.0      |
| Max Users        | 100                |

Click **Add** to save the pool.

### Step 3: Configure DHCP Helper on Routers

The DHCP server is in VLAN 50.
PCs are in VLANs 10, 20, 30, 40.
The router must forward DHCP requests to the server.
We use the `ip helper-address` command on each sub-interface.

On Router1:

```
Router1(config)# interface FastEthernet0/0.10
Router1(config-subif)# ip helper-address 192.168.50.10
Router1(config-subif)# exit

Router1(config)# interface FastEthernet0/0.20
Router1(config-subif)# ip helper-address 192.168.50.10
Router1(config-subif)# exit
```

On Router2:

```
Router2(config)# interface FastEthernet0/0.30
Router2(config-subif)# ip helper-address 192.168.50.10
Router2(config-subif)# exit

Router2(config)# interface FastEthernet0/0.40
Router2(config-subif)# ip helper-address 192.168.50.10
Router2(config-subif)# exit
```

### Step 4: Set PCs to Use DHCP

On each PC in Packet Tracer:
1. Click the PC.
2. Click the **Desktop** tab.
3. Click **IP Configuration**.
4. Select **DHCP**.
5. Wait a moment. The PC will get an IP address automatically.

---

## 7. DNS Server Configuration

The DNS server translates domain names to IP addresses.
For example: `www.company.com` → `192.168.50.30`

### Step 1: Set Static IP on DNS Server

1. Click on the DNS Server device.
2. Click the **Desktop** tab.
3. Click **IP Configuration**.
4. Enter these values:
   - IP Address: `192.168.50.20`
   - Subnet Mask: `255.255.255.0`
   - Default Gateway: `192.168.50.1`
5. Click **Save**.

### Step 2: Enable DNS Service

1. Click on the DNS Server device.
2. Click the **Services** tab.
3. Click **DNS** on the left menu.
4. Turn DNS service **ON**.

### Step 3: Add DNS Records

Add these A records (Address records):

| Name              | Type | Address         |
|-------------------|------|-----------------|
| www.company.com   | A    | 192.168.50.30   |
| mail.company.com  | A    | 192.168.50.40   |
| dhcp.company.com  | A    | 192.168.50.10   |
| dns.company.com   | A    | 192.168.50.20   |

To add a record:
1. Type the name in the **Name** field (e.g., `www.company.com`).
2. Select **A Record** in the Type field.
3. Type the IP address in the **Address** field.
4. Click **Add**.

Repeat for each record.

---

## 8. Web Server Configuration

The web server hosts the company website.
Users can open it in a browser.

### Step 1: Set Static IP on Web Server

1. Click on the Web Server device.
2. Click the **Desktop** tab.
3. Click **IP Configuration**.
4. Enter these values:
   - IP Address: `192.168.50.30`
   - Subnet Mask: `255.255.255.0`
   - Default Gateway: `192.168.50.1`
   - DNS Server: `192.168.50.20`
5. Click **Save**.

### Step 2: Enable HTTP Service

1. Click on the Web Server device.
2. Click the **Services** tab.
3. Click **HTTP** on the left menu.
4. Make sure HTTP service is **ON**.
5. Make sure HTTPS service is **ON** (optional).

### Step 3: Edit the Web Page

1. Stay in the **HTTP** services panel.
2. Find the file `index.html` in the file list.
3. Click **Edit** next to `index.html`.
4. Replace the content with this simple HTML:

```html
<html>
<head><title>Company Website</title></head>
<body>
<h1>Welcome to Company.com</h1>
<p>This is the official company website.</p>
<p>Departments: HR, IT, Finance, Sales</p>
</body>
</html>
```

5. Click **Save**.

### Step 4: Test the Web Server

1. Click on any PC (e.g., HR-PC1).
2. Click the **Desktop** tab.
3. Click **Web Browser**.
4. Type `http://192.168.50.30` in the address bar.
5. You should see the company website.

You can also type `http://www.company.com` if DNS is configured.

---

## 9. Mail Server Configuration

The mail server allows users to send and receive email.

### Step 1: Set Static IP on Mail Server

1. Click on the Mail Server device.
2. Click the **Desktop** tab.
3. Click **IP Configuration**.
4. Enter these values:
   - IP Address: `192.168.50.40`
   - Subnet Mask: `255.255.255.0`
   - Default Gateway: `192.168.50.1`
   - DNS Server: `192.168.50.20`
5. Click **Save**.

### Step 2: Enable Email Service

1. Click on the Mail Server device.
2. Click the **Services** tab.
3. Click **EMAIL** on the left menu.
4. Turn the email service **ON**.
5. Set the **Domain Name** to: `company.com`
6. Click **Set**.

### Step 3: Create User Accounts

Create four email accounts (one per department):

| Username    | Password | Department |
|-------------|----------|------------|
| hr.user     | Pass123  | HR         |
| it.user     | Pass123  | IT         |
| finance.user| Pass123  | Finance    |
| sales.user  | Pass123  | Sales      |

To add a user:
1. Type the username in the **User** field (e.g., `hr.user`).
2. Type the password in the **Password** field.
3. Click **+** to add the user.

Repeat for each user.

### Step 4: Configure Email Client on a PC

To set up email on HR-PC1:
1. Click on HR-PC1.
2. Click the **Desktop** tab.
3. Click **Email**.
4. Click **Configure Mail**.
5. Enter these settings:
   - Your Name: `HR User`
   - Email Address: `hr.user@company.com`
   - Incoming Mail Server: `192.168.50.40`
   - Outgoing Mail Server: `192.168.50.40`
   - Username: `hr.user`
   - Password: `Pass123`
6. Click **Save**.

Repeat this for each PC using its own account.

### Step 5: Send a Test Email

1. On HR-PC1, click the **Desktop** tab.
2. Click **Email**.
3. Click **Compose**.
4. In **To**, type: `it.user@company.com`
5. In **Subject**, type: `Hello from HR`
6. In **Body**, type: `This is a test email.`
7. Click **Send**.

Then check IT-PC1 for the email:
1. On IT-PC1, open **Email**.
2. Click **Receive**.
3. You should see the email from HR.

---

## 10. Testing and Verification

Use these tests to check that everything works.

### Test 1: Ping Between VLANs

This tests inter-VLAN routing.

1. Click on HR-PC1.
2. Click the **Desktop** tab.
3. Click **Command Prompt**.
4. Type:
   ```
   ping 192.168.20.100
   ```
5. You should get replies from IT-PC1.

If you get no reply:
- Check that the router sub-interfaces are configured correctly.
- Check that the switch trunk ports are working.

### Test 2: Ping from PC to Server

1. On HR-PC1, open **Command Prompt**.
2. Type:
   ```
   ping 192.168.50.30
   ```
3. You should get replies from the Web Server.

### Test 3: Ping Gateway

From any PC, ping its gateway:
```
ping 192.168.10.1
```
(Replace with the correct gateway for your PC's VLAN.)

### Test 4: Open Web Server in Browser

1. On any PC, click **Desktop** → **Web Browser**.
2. Type: `http://192.168.50.30`
3. You should see the company homepage.
4. Also try: `http://www.company.com`

### Test 5: Send and Receive Email

1. On HR-PC1, open **Email** and send an email to `it.user@company.com`.
2. On IT-PC1, open **Email** and click **Receive**.
3. You should see the email from HR.

### Test 6: Verify RIP Routing Table

On any router, type:
```
Router1# show ip route
```

You should see routes learned via RIP (marked with **R**):
```
R    192.168.30.0/24 [120/2] via 10.0.0.1, ...
R    192.168.40.0/24 [120/2] via 10.0.0.1, ...
R    192.168.50.0/24 [120/2] via 10.0.0.1, ...
```

### Test 7: Verify DHCP Leases

On the DHCP Server:
1. Click on the DHCP Server.
2. Click **Services** → **DHCP**.
3. Scroll down to see the list of active leases.
4. You should see IP addresses given to each PC.

### Test 8: Verify VLANs on Switch

On any switch, type:
```
Switch1# show vlan brief
```

You should see:
```
VLAN Name                 Status    Ports
---- -------------------- --------- ---------
1    default              active    ...
10   HR                   active    Fa0/2, Fa0/3
```

### Test 9: Verify Trunk Ports

On any switch, type:
```
Switch1# show interfaces trunk
```

You should see the trunk ports listed with the VLANs they carry.

### Test 10: Verify RIP Neighbors

On any router, type:
```
Router1# show ip rip database
```

This shows all routes learned via RIP.

---

## Summary Table

| Component    | Configuration Summary                                  |
|--------------|--------------------------------------------------------|
| Routers      | 5 routers, router-on-a-stick for VLANs, RIPv2         |
| Switches     | 5 switches, VLANs 10/20/30/40/50, trunk uplinks       |
| DHCP         | Dedicated server, 4 pools, ip helper-address on routers|
| DNS          | Dedicated server, A records for all servers           |
| Web Server   | Static IP, HTTP enabled, custom HTML page             |
| Mail Server  | Static IP, SMTP/POP3, domain company.com, 4 users     |
| VLANs        | HR=10, IT=20, Finance=30, Sales=40, Servers=50        |
| RIP          | Version 2, no auto-summary, all networks advertised   |

---

## Troubleshooting Tips

| Problem                      | Solution                                              |
|------------------------------|-------------------------------------------------------|
| PC cannot get IP via DHCP    | Check ip helper-address on router sub-interface       |
| Cannot ping another VLAN     | Check router sub-interface encapsulation and trunk    |
| Cannot reach server          | Check RIP routes and router interfaces                |
| Web page not loading         | Verify HTTP service is ON on web server               |
| Email not sending            | Check mail server domain and user credentials         |
| RIP routes not appearing     | Check network statements in RIP config                |
| Trunk not working            | Check switchport mode trunk on both ends              |

---

*End of Document*
