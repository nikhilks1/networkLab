# Cisco VLAN & Dot1Q Trunk Configuration Lab (Multi-Layer Switch)

A hands-on lab to practice VLAN assignment and trunk configuration with **dot1q encapsulation** on Cisco 3560-24PS multi-layer switches in Packet Tracer.

## Network Topology

```
PC1 (10.0.0.1/24) --F0/2--|                    |--F0/2-- PC3 (10.0.0.3/24)
                          SW1 --F0/1----F0/1-- SW2
PC2 (10.0.0.2/24) --F0/3--|
```

| Device | VLAN       |
|--------|------------|
| PC1    | VLAN1 (native/default) |
| PC2    | VLAN2      |
| PC3    | VLAN2      |

## What is a Multi-Layer Switch?

These switches (3560-24PS) operate at both **Layer 2 and Layer 3** of the OSI model — unlike the 2960 used in the previous lab which is Layer 2 only. In this lab we only use their Layer 2 features, but they are chosen specifically because they support **multiple trunk encapsulation types**, which introduces an extra configuration step.

## Lab Tasks

1. Ping between all PCs to confirm initial connectivity
2. Assign PC2 (SW1 F0/3) and PC3 (SW2 F0/2) to VLAN2
3. Create a dot1q trunk between SW1 and SW2
4. Ping again — verify VLAN isolation

## Key Commands

```bash
# Assign port to VLAN2 (on SW1 for PC2)
configure terminal
interface fastethernet 0/3
switchport mode access
switchport access vlan 2

# Assign port to VLAN2 (on SW2 for PC3)
interface fastethernet 0/2
switchport mode access
switchport access vlan 2

# Configure trunk with dot1q encapsulation (on BOTH SW1 and SW2)
interface fastethernet 0/1
switchport trunk encapsulation dot1q   ← required on 3560, not needed on 2960
switchport mode trunk
```

## ⚠️ Key Difference from Previous Lab — Trunk Encapsulation

On the **2960 switch** (previous lab), only one encapsulation type is supported so `switchport mode trunk` works directly.

On the **3560 switch** (this lab), two types are supported:
| Type | Description |
|------|-------------|
| **ISL** | Cisco proprietary (older, rare) |
| **dot1Q** | Industry standard (IEEE 802.1Q) — most common |

Without setting encapsulation first, you'll get this error:
```
Command rejected: An interface whose trunk encapsulation is "Auto" cannot be configured to "trunk" mode.
```
**Fix:** Always run `switchport trunk encapsulation dot1q` before `switchport mode trunk` on 3560 switches.

## Expected Ping Results After Configuration

| Source | Destination | Result |
|--------|-------------|--------|
| PC2    | PC3         | ✅ Success (both VLAN2, trunk active) |
| PC1    | PC2         | ❌ Fail (VLAN1 vs VLAN2) |
| PC1    | PC3         | ❌ Fail (VLAN1 vs VLAN2) |

## Tools Required

- Cisco Packet Tracer
