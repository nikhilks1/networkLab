# Inter-VLAN Routing Troubleshooting Lab — Multiple Devices

An advanced troubleshooting lab using the Router on a Stick topology. This time there is **one misconfiguration per networking device** (R1, SW1, SW2) — find and fix all three.

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

PC1 cannot ping any other PC on the network.  
There is **one misconfiguration on each device** — R1, SW1, and SW2.  
Fix all three without changing VLAN membership.

---

## Bug 1 — R1 : Wrong IP on Sub-interface G0/0.13

**Found with:**
```bash
show ip interface brief
# G0/0.13 showed 10.0.0.11 instead of 10.0.0.1
```

**Fix:**
```bash
configure terminal
interface gigabitethernet 0/0.13
ip address 10.0.0.1 255.255.255.128
```

---

## Bug 2 — SW1 : PC1's Port in Wrong VLAN

**Found with:**
```bash
show vlan brief
# F0/1 was assigned to VLAN 12 instead of VLAN 13
```

**Fix:**
```bash
configure terminal
interface fastethernet 0/1
switchport access vlan 13
```

---

## Bug 3 — SW2 : Inter-Switch Link Not a Trunk

**Found with:**
```bash
show interfaces trunk
# No trunk interfaces appeared — G0/1 was still an access port
```

**Fix:**
```bash
configure terminal
interface gigabitethernet 0/1
switchport mode trunk
```

---

## Useful Troubleshooting Commands

| Command | What to Check |
|---------|---------------|
| `show ip interface brief` | IP addresses on sub-interfaces |
| `show vlan brief` | Which ports belong to which VLAN |
| `show interfaces trunk` | Which ports are trunks |
| `ipconfig /all` | PC default gateway correctness |

## Key Lesson

> In multi-device troubleshooting, check **every device systematically** — R1 → SW1 → SW2.  
> One wrong VLAN ID, one wrong IP, or one missing trunk can each independently break connectivity.

## Tools Required

- Cisco Packet Tracer
