# CCNA Lab – Troubleshooting (VLANs, Trunking, Port Security, Router-on-a-Stick)

A Packet Tracer troubleshooting lab using the same topology as Lab 20. Find and fix all misconfigurations to restore full connectivity between PC1, PC2, PC3, and PC4.

## Topology

```
                        R1 (G0/0)
                           |
                        SW1 (G0/1)
                F0/2 /          \ F0/3        F0/1 (trunk) → SW2
              PC1                PC2         F0/2 → PC3 | F0/3 → PC4

PC1: 10.0.0.11/24 (VLAN 13)   PC2: 10.0.1.12/24 (VLAN 24)
PC3: 10.0.0.13/24 (VLAN 13)   PC4: 10.0.1.14/24 (VLAN 24)
```

## Misconfigurations to Find & Fix

### SW1 – Port Security
- **Bug:** F0/2 had a static secure MAC address configured instead of sticky learning
- **Fix:**
```bash
interface f0/2
 no switchport port-security mac-address <wrong-mac>
 switchport port-security mac-address sticky
```

### R1 – Sub-interface Errors
- **Bug 1:** G0/0.13 had wrong IP address (10.0.0.2 instead of 10.0.0.1)
- **Bug 2:** G0/0.24 had wrong VLAN tag (dot1q 2 instead of dot1q 24)
- **Fix:**
```bash
interface g0/0.13
 ip address 10.0.0.1 255.255.255.0

interface g0/0.24
 encapsulation dot1q 24
```

### SW2 – Trunk & VLAN
- **Bug 1:** F0/1 was configured as an access port instead of a trunk
- **Bug 2:** F0/2 was in VLAN 23 instead of VLAN 13
- **Fix:**
```bash
interface f0/1
 switchport mode trunk

interface f0/2
 switchport access vlan 13
```

## Verification Commands

```bash
show port-security
show port-security interface f0/2
show interfaces trunk
show interfaces f0/1 switchport
show vlan brief
show run
```

## Key Concepts

- Always verify port security violation counters — a non-zero count points to the problem interface
- Use `show interfaces <int> switchport` to check if a port is access or trunk mode
- Sub-interface VLAN tags and IP addresses must exactly match what's configured as the default gateway on end hosts
- After fixing trunk/VLAN configs, allow a few seconds for Spanning Tree to reconverge before testing pings
