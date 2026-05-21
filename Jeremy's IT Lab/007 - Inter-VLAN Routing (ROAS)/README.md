# Inter-VLAN Routing — Router on a Stick Lab

A hands-on lab to practice inter-VLAN routing using the **Router on a Stick** method with one Cisco 2901 router and two 2960-24TT switches in Packet Tracer.

## Network Topology

```
R1 (2901)
 |  G0/0
 |  G0/1
SW1 (2960-24TT) ---G0/2----G0/1--- SW2 (2960-24TT)
 |  F0/1   F0/2                    F0/2    F0/1
PC1        PC2                     PC3     PC4
10.0.0.2/25  10.0.0.130/25    10.0.0.3/25  10.0.0.131/25
```

| Device | VLAN    | IP Address    | Default Gateway |
|--------|---------|---------------|-----------------|
| PC1    | VLAN 13 | 10.0.0.2/25   | 10.0.0.1        |
| PC3    | VLAN 13 | 10.0.0.3/25   | 10.0.0.1        |
| PC2    | VLAN 24 | 10.0.0.130/25 | 10.0.0.129      |
| PC4    | VLAN 24 | 10.0.0.131/25 | 10.0.0.129      |

## What is Router on a Stick?

A method of inter-VLAN routing where a **single physical interface** on a router is split into multiple **sub-interfaces** — one per VLAN. Each sub-interface acts as the default gateway for its VLAN, avoiding the need for a separate router port per VLAN.

```
R1 G0/0
 ├── G0/0.13  →  Gateway for VLAN 13  (10.0.0.1/25)
 └── G0/0.24  →  Gateway for VLAN 24  (10.0.0.129/25)
```

## Lab Tasks

1. Ping between PCs — identify which succeed (same subnet) and which fail
2. Assign PC1 & PC3 to VLAN 13, and PC2 & PC4 to VLAN 24
3. Create a trunk link between SW1 and SW2
4. Configure inter-VLAN routing using sub-interfaces on R1's G0/0
5. Test connectivity — all PCs should now ping each other

## Key Commands

```bash
# --- SW1: Assign ports to VLANs ---
configure terminal
interface fastethernet 0/1
switchport mode access
switchport access vlan 13

interface fastethernet 0/2
switchport mode access
switchport access vlan 24

# --- SW1: Trunk to SW2 ---
interface gigabitethernet 0/2
switchport mode trunk

# --- SW1: Trunk to R1 (see note below) ---
interface gigabitethernet 0/1
switchport mode trunk

# --- SW2: Assign ports to VLANs ---
interface fastethernet 0/2
switchport mode access
switchport access vlan 13

interface fastethernet 0/1
switchport mode access
switchport access vlan 24

# --- SW2: Trunk to SW1 ---
interface gigabitethernet 0/1
switchport mode trunk

# --- R1: Enable physical interface ---
configure terminal
interface gigabitethernet 0/0
no shutdown

# --- R1: Sub-interface for VLAN 13 ---
interface gigabitethernet 0/0.13
encapsulation dot1q 13
ip address 10.0.0.1 255.255.255.128

# --- R1: Sub-interface for VLAN 24 ---
interface gigabitethernet 0/0.24
encapsulation dot1q 24
ip address 10.0.0.129 255.255.255.128
```

## ⚠️ Easy-to-Miss Step — Trunk Between SW1 and R1

The SW1 port connected to R1 (G0/1) defaults to an **access port in VLAN1**.
VLAN 13 and VLAN 24 traffic cannot reach R1 until this port is also set as a trunk.

```bash
# On SW1
interface gigabitethernet 0/1
switchport mode trunk
```

Without this, inter-VLAN routing will not work even if R1 sub-interfaces are correctly configured.

## Expected Ping Results After Full Configuration

| Source | Destination | Result |
|--------|-------------|--------|
| PC1    | PC3         | ✅ Same VLAN13 |
| PC2    | PC4         | ✅ Same VLAN24 |
| PC1    | PC2         | ✅ Inter-VLAN via R1 |
| PC1    | PC4         | ✅ Inter-VLAN via R1 |

## Tools Required

- Cisco Packet Tracer
