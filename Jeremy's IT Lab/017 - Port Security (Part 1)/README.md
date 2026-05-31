# Cisco Port Security Lab

A hands-on lab to understand and configure basic port security on Cisco switches — controlling which MAC addresses are allowed on which switch interfaces.

## Network Topology

```
PC1 (192.168.1.11/24) --F0/2-- SW1 --F0/1----F0/1-- SW2 --F0/2-- PC2 (192.168.1.12/24)
```

## Lab Tasks

1. Find the MAC address of SW2 from SW1's CLI, and vice versa
2. Explain why PC1 and PC2 don't appear in the MAC address tables initially
3. Ping PC1 → PC2, then check MAC address tables again
4. Enable port security on F0/2 of each switch (PC-facing ports)
5. Confirm the default max MAC addresses allowed (1) — explicitly configure it
6. Confirm the default violation action (shutdown) — explicitly configure it
7. Manually configure PC1's MAC as a secure address on SW1 F0/2, and PC2's on SW2 F0/2

## Key Commands

```bash
# View MAC address table
show mac address-table

# Enable port security (port must be explicitly set to access mode first)
configure terminal
interface fastethernet 0/2
switchport mode access
switchport port-security

# Set max allowed MAC addresses (default is 1)
switchport port-security maximum 1

# Set violation action
switchport port-security violation shutdown

# Manually configure a secure MAC address
switchport port-security mac-address <mac-address>

# View port security settings
show port-security

# View secure MAC addresses
show port-security address
```

## Why PC MACs Didn't Appear Initially (Step 2)

Switches learned each other's MAC addresses via **BPDU (Bridge Protocol Data Unit)** packets sent automatically as part of Spanning Tree Protocol. However, PC1 and PC2 had not sent any traffic yet, so the switches had no way to learn their MAC addresses. MAC addresses are only learned when a frame is **received** on a port.

## Port Security Violation Actions

| Action | Drops Traffic | Log/Message | Shuts Port | Violation Counter |
|--------|--------------|-------------|------------|-------------------|
| **Protect** | ✅ | ❌ | ❌ | ❌ |
| **Restrict** | ✅ | ✅ | ❌ | ✅ |
| **Shutdown** *(default)* | ✅ | ✅ | ✅ (err-disabled) | ✅ |

## ⚠️ Port Security Requires Access Mode

Port security **cannot be enabled on a dynamic or trunk port**. Even if the port is currently acting as an access port, it must be **explicitly set** to access mode first:

```bash
switchport mode access       # required before enabling port security
switchport port-security
```

## Tools Required

- Cisco Packet Tracer
