# CCNA Lab – Multi-Topic Review (VLANs, Trunking, Port Security, Router-on-a-Stick)

A Packet Tracer review lab covering hostnames, enable secret, VLANs, trunking, port security, and inter-VLAN routing in a single topology.

## Topology

```
                        R1 (G0/0)
                           |
                        SW1 (G0/1)
                F0/2 /          \ F0/3        F0/1 (trunk) → SW2
              PC1                PC2         F0/2 → PC3 | F0/3 → PC4

PC1: 10.0.0.11/24   PC2: 10.0.1.12/24
PC3: 10.0.0.13/24   PC4: 10.0.1.14/24
```

| VLAN | PCs        | Gateway   |
|------|------------|-----------|
| 13   | PC1, PC3   | 10.0.0.1  |
| 24   | PC2, PC4   | 10.0.1.1  |

## Lab Steps

1. Configure hostnames on R1, SW1, and SW2.
2. Set enable secret to `CCNA` on all three devices.
3. Configure PC-facing switch ports as access ports in their respective VLANs (VLAN 13: PC1, PC3 — VLAN 24: PC2, PC4).
4. Use `show cdp neighbors` to identify the inter-switch link, then configure it as a trunk.
5. Enable port security on all PC-facing ports with sticky MAC learning and violation action `restrict`.
6. Configure Router-on-a-Stick on R1's G0/0 with sub-interfaces for each VLAN.
7. Ping between all PCs to confirm full connectivity across VLANs.

## Key Commands

```bash
# Hostnames & enable secret
hostname SW1
enable secret CCNA

# Access ports & VLANs
interface f0/2
 switchport mode access
 switchport access vlan 13

# Trunk
interface f0/1
 switchport mode trunk

# Port security (sticky + restrict)
interface range f0/2 - 3
 switchport port-security
 switchport port-security mac-address sticky
 switchport port-security violation restrict

# Router-on-a-Stick (R1)
interface g0/0
 no shutdown

interface g0/0.13
 encapsulation dot1q 13
 ip address 10.0.0.1 255.255.255.0

interface g0/0.24
 encapsulation dot1q 24
 ip address 10.0.1.1 255.255.255.0

# Verification
show interfaces trunk
show cdp neighbors
show port-security interface f0/2
```

## Key Concepts

- **Enable secret** is always preferred over `enable password` — it uses stronger MD5 encryption
- **Trunk links** are required between switches to carry multiple VLANs
- **Port security `restrict`** drops violating frames silently without err-disabling the port (unlike the default `shutdown`)
- **Router-on-a-Stick** uses sub-interfaces (one per VLAN) on a single physical link to perform inter-VLAN routing
- Sub-interface numbers don't have to match VLAN IDs, but it's best practice
