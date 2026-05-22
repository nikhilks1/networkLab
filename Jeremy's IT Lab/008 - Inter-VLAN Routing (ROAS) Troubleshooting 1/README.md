# Inter-VLAN Routing Troubleshooting Lab

A troubleshooting lab using the same topology as the Router on a Stick lab. Inter-VLAN routing has been pre-configured but contains **one misconfiguration** — find it and fix it.

## Network Topology

```
R1 (2901)
 |  G0/0
 |  G0/1
SW1 (2960-24TT) ---G0/2----G0/1--- SW2 (2960-24TT)
 |  F0/1   F0/2                    F0/2    F0/1
PC1        PC2                     PC4     PC3
10.0.0.2/25  10.0.0.130/25    10.0.0.131/25  10.0.0.3/25
```

| Device | VLAN    | IP Address    | Default Gateway |
|--------|---------|---------------|-----------------|
| PC1    | VLAN 13 | 10.0.0.2/25   | 10.0.0.1        |
| PC3    | VLAN 13 | 10.0.0.3/25   | 10.0.0.1        |
| PC2    | VLAN 24 | 10.0.0.130/25 | 10.0.0.129      |
| PC4    | VLAN 24 | 10.0.0.131/25 | 10.0.0.129      |

## Scenario

Inter-VLAN routing was configured but PCs in **different VLANs cannot ping each other**.  
PCs in the **same VLAN can** ping each other.  
There is exactly **one misconfiguration** — troubleshoot and fix it without changing VLAN membership.

## Troubleshooting Steps Taken

```bash
# Step 1 — Confirm the problem (ping tests from PC1 & PC2)
# Same-VLAN pings work, cross-VLAN pings fail → inter-VLAN routing is broken

# Step 2 — Check SW1 link to R1
show ip interface brief          # G0/1 is up/up ✅

# Step 3 — Check trunk interfaces on SW1
show interfaces trunk            # Both trunks up, all VLANs allowed ✅

# Step 4 — Check R1 sub-interfaces
show ip interface brief          # IPs look correct ✅

# Step 5 — Check PC default gateways
ipconfig /all                    # All PCs have correct gateways ✅

# Step 6 — Inspect sub-interface encapsulation on R1
show interface g0/0.13           # VLAN ID 13 ✅
show interface g0/0.24           # VLAN ID 14 ❌  ← BUG FOUND
```

## Root Cause

Sub-interface `G0/0.24` on R1 had encapsulation set to **VLAN 14** instead of **VLAN 24** — a simple typo. This caused all VLAN 24 traffic to be mismatched, so R1 couldn't route between VLANs.

## Fix

```bash
configure terminal
interface gigabitethernet 0/0.24
encapsulation dot1q 24           # corrected from 14 → 24
```

## Useful Troubleshooting Commands

| Command | Purpose |
|---------|---------|
| `show ip interface brief` | Check interface status and IP addresses |
| `show interfaces trunk` | Verify trunk ports and allowed VLANs |
| `show interface g0/0.X` | Check VLAN ID set on a sub-interface |
| `show running-config` | Full config review |
| `ipconfig /all` | Verify PC IP and default gateway |

## Key Lesson

> Always double-check the VLAN ID in `encapsulation dot1q <vlan-id>` on sub-interfaces.  
> A single digit typo here will silently break inter-VLAN routing with no obvious error message.

## Tools Required

- Cisco Packet Tracer
