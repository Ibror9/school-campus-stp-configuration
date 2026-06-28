# School Campus STP Configuration

## Cisco Packet Tracer Network Project

## Project Overview

This project demonstrates the design and configuration of a redundant school campus network using Cisco Packet Tracer.

The main goal of this project is to implement **Spanning Tree Protocol (STP)** to prevent Layer 2 switching loops while maintaining network availability through redundant links.

The network includes a distribution layer, access layer, redundant connections, and router redundancy using HSRP.

---

# Network Topology

The campus network consists of:

* 2 Distribution switches
* 2 Access switches
* 2 Routers
* Multiple redundant Layer 2 paths
* End-user devices

Topology:

```
              SP1 -------- SP2
               |            |
               |            |
              R1 ----------- R2
               |            |
              CD1 ========= CD2
              /              \
           Acc3              Acc4
             |                |
            PC1              PC2
```

---

# Network Devices

| Device | Role                          |
| ------ | ----------------------------- |
| SP1    | Service Provider Router       |
| SP2    | Service Provider Router       |
| R1     | Primary Router                |
| R2     | Backup Router                 |
| CD1    | Primary Distribution Switch   |
| CD2    | Secondary Distribution Switch |
| Acc3   | Access Switch                 |
| Acc4   | Access Switch                 |

---

# IP Addressing Plan

## Campus LAN

Network:

```
10.10.10.0/24
```

HSRP Virtual IP:

```
10.10.10.1
```

Router interfaces:

```
R1:
10.10.10.2/24

R2:
10.10.10.3/24
```

PC addressing:

```
PC1:
10.10.10.10

PC2:
10.10.10.11
```

---

# Project Objectives

The network was designed to achieve:

* Layer 2 loop prevention
* High availability
* Redundant paths
* Automatic failover
* Efficient traffic forwarding
* Campus network reliability

---

# VLAN Design

The campus network uses VLAN segmentation.

| VLAN | Name       | Purpose            |
| ---- | ---------- | ------------------ |
| 10   | STUDENTS   | Student devices    |
| 20   | TEACHERS   | Staff devices      |
| 30   | ADMIN      | Administration     |
| 99   | MANAGEMENT | Network management |

---

# VLAN Configuration

Example:

```cisco
vlan 10
name STUDENTS

vlan 20
name TEACHERS

vlan 30
name ADMIN

vlan 99
name MANAGEMENT
```

---

# Spanning Tree Configuration

## STP Mode

Rapid PVST+ was used:

```cisco
spanning-tree mode rapid-pvst
```

Rapid PVST+ provides:

* Faster convergence
* VLAN-based STP
* Improved failover

---

# Root Bridge Configuration

## CD1 - Primary Root Bridge

CD1 was configured as the root bridge.

```cisco
spanning-tree vlan 1 priority 4096
```

Role:

* Main forwarding switch
* Best path selection point

---

## CD2 - Secondary Root Bridge

CD2 provides redundancy.

```cisco
spanning-tree vlan 1 priority 8192
```

Role:

* Backup root bridge
* Takes over if CD1 fails

---

# Access Switch STP Configuration

Acc3 and Acc4:

```cisco
spanning-tree mode rapid-pvst

spanning-tree portfast default

spanning-tree bpduguard default
```

Purpose:

* Faster client connection
* Protection against accidental loops

---

# Trunk Configuration

Distribution links use trunking.

Example:

```cisco
interface g0/2

switchport mode trunk

switchport trunk allowed vlan 10,20,30,99
```

---

# EtherChannel Configuration

Redundant links were bundled to increase bandwidth and reliability.

Example:

```cisco
interface range g0/1-2

channel-group 1 mode active


interface port-channel 1

switchport mode trunk
```

Benefits:

* Increased bandwidth
* Link redundancy
* Loop prevention

---

# HSRP Configuration

R1:

```cisco
interface g0/1

standby 1 ip 10.10.10.1

standby 1 priority 110

standby 1 preempt
```

R2:

```cisco
interface g0/1

standby 1 ip 10.10.10.1

standby 1 priority 100

standby 1 preempt
```

Purpose:

* Router redundancy
* Default gateway failover

---

# STP Verification

Commands used:

## Check Root Bridge

```cisco
show spanning-tree
```

Expected:

```
Root ID
Priority 4096
CD1
```

---

## Check VLANs

```cisco
show vlan brief
```

---

## Check Trunks

```cisco
show interfaces trunk
```

---

## Check MAC Learning

```cisco
show mac address-table
```

---

# Testing and Troubleshooting

## Test 1: Connectivity

Command:

```bash
ping 10.10.10.11
```

Result:

Successful communication between hosts.

---

## Test 2: STP Failover

Disabled primary path:

```cisco
interface g0/2

shutdown
```

Result:

STP recalculated the topology and traffic moved through the backup path.

---

## Test 3: Router Failover

Shutdown R1 interface:

Result:

R2 became active gateway through HSRP.

---

# Skills Demonstrated

* Cisco IOS configuration
* Switching
* VLAN implementation
* Trunking
* Rapid-PVST+
* Root bridge election
* STP troubleshooting
* EtherChannel
* HSRP
* Network documentation
* Campus network design

---

# Tools Used

* Cisco Packet Tracer
* Cisco IOS CLI
* GitHub

---

# Author

Your Name BARRY

CCNA Networking Practice Project

