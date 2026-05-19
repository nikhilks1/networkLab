# Cisco VLAN Configuration Lab

A hands-on lab to practice configuring VLANs and trunk interfaces on two Cisco 2960-24TT switches in Packet Tracer.

## Network Topology

```
PC1 (10.0.0.1/24) --F0/2--|              |--F0/2-- PC3 (10.0.0.3/24)
                          SW1 --F0/1--F0/1-- SW2
PC2 (10.0.0.2/24) --F0/3--|              |--F0/3-- PC4 (10.0.0.4/24)

VLAN1 → PC1, PC3
VLAN2 → PC2, PC4
```

## OSI Layer

> VLANs operate at **Layer 2 (Data Link Layer)** of the OSI model.  
> They isolate broadcast domains at the switch level, even if devices share the same Layer 3 (IP) network.

## Lab Tasks

1. Ping between all computers to confirm initial connectivity
2. Assign PC1 & PC3 to VLAN1, and PC2 & PC4 to VLAN2
3. Try pinging PC1↔PC3 and PC2↔PC4 — observe what works and what doesn't
4. Configure the inter-switch link (F0/1) on both switches as a trunk port
5. Ping again — verify VLAN isolation is working correctly

## Key Commands

```bash
# Assign a port to a VLAN (access mode)
configure terminal
interface fastethernet 0/2
switchport mode access
switchport access vlan 1

interface fastethernet 0/3
switchport mode access
switchport access vlan 2

# Configure trunk port between switches (on both SW1 and SW2)
interface fastethernet 0/1
switchport mode trunk

# Verify configuration
show running-config
show vlan brief
```

## ⚠️ Issue at Step 3 — PC2 & PC4 Couldn't Ping Each Other (Same VLAN2)

**Problem:** PC1 and PC3 (VLAN1) could ping each other, but PC2 and PC4 (VLAN2) could **not** ping each other — even though they were in the same VLAN.

**Root Cause:** The link between SW1 and SW2 (F0/1 on both switches) was still an **access port**, defaulting to the **native VLAN (VLAN1)**. This meant only VLAN1 traffic could pass between the two switches. VLAN2 traffic from PC2/PC4 was being **dropped** at the inter-switch link.

**Fix (Step 4):** Configure F0/1 on both switches as a **trunk port** using `switchport mode trunk`. Trunk ports carry traffic from **multiple VLANs** simultaneously, allowing VLAN2 traffic to pass through.

## Native VLAN — What It Is

- The **native VLAN** is the VLAN whose traffic is sent **untagged** across a trunk link.
- On Cisco switches, **VLAN1 is the native VLAN by default**.
- All switch ports belong to VLAN1 by default, which is why explicitly setting a port to VLAN1 doesn't show in the running config.
- For security, the native VLAN is often changed in production environments (not covered in this lab).

## Expected Ping Results After Step 5

| Source | Destination | Result |
|--------|-------------|--------|
| PC1    | PC3         | ✅ Success (both VLAN1) |
| PC2    | PC4         | ✅ Success (both VLAN2) |
| PC1    | PC2         | ❌ Fail (different VLANs) |
| PC1    | PC4         | ❌ Fail (different VLANs) |

## Tools Required

- Cisco Packet Tracer
