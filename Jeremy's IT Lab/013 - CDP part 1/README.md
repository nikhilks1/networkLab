# Cisco Discovery Protocol (CDP) Lab

A hands-on lab to explore CDP — discovering neighbors, understanding timers, enabling/disabling CDP globally and per-interface, and using interface range configuration.

## Network Topology

```
PC1, PC2 --F0/3, F0/4--|                              |--F0/3, F0/4-- PC3, PC0
                       SW1 --F0/1-- R1 --Se2/0----Se2/0-- R2 --Fa0/1-- SW2
                             192.168.1.0/24  10.0.0.0/24   192.168.2.0/24
```

## Lab Tasks

1. Use CDP to identify which interfaces connect all routers and switches
2. Determine DCE/DTE on the R1–R2 serial link — set clock rate of 64 KB/s on DCE side
3. Find the default CDP send and hold timers using a show command
4. Disable CDP globally on R1 — attempt to view CDP neighbors
5. Re-enable CDP on R1 — check if neighbors appear instantly
6. Disable CDP on switch interfaces connected to PCs (security best practice)

## Key Commands

```bash
# View directly connected CDP neighbors
show cdp neighbors

# View detailed CDP neighbor info (IP, IOS version, etc.)
show cdp neighbors detail

# Check DCE/DTE on serial interface
show controllers serial 2/0

# Set clock rate on DCE side (R1)
configure terminal
interface serial 2/0
clock rate 64000

# Check CDP timers on all interfaces
show cdp interface

# Disable CDP globally
configure terminal
no cdp run

# Re-enable CDP globally
cdp run

# Disable CDP on specific interfaces (per-interface)
interface fastethernet 0/3
no cdp enable

# Disable CDP on multiple interfaces using interface range
interface range fastethernet 0/3 - 4
no cdp enable
```

## CDP Timers (Default)

| Timer     | Default | Meaning |
|-----------|---------|---------|
| Send time | 60 sec  | CDP advertisement sent every 60 seconds |
| Hold time | 180 sec | Neighbor removed if no advertisement received in 180 seconds |

> After re-enabling CDP, neighbors **do not appear instantly** — you must wait up to 60 seconds for the next advertisement cycle.

## CDP Security — Why Disable on PC-Facing Ports?

CDP shares device information (model, IOS version, interfaces) with directly connected devices. PCs do not need this information, so disabling CDP on those ports reduces the risk of information leakage.

```bash
# Best practice on both switches — disable CDP toward PCs
interface range fastethernet 0/3 - 4
no cdp enable
```

## CDP vs LLDP

| Feature | CDP | LLDP |
|---------|-----|------|
| Vendor  | Cisco proprietary | IEEE 802.1AB (vendor-neutral) |
| Default | Enabled on Cisco devices | Disabled by default on Cisco |
| Use case | Cisco-only networks | Multi-vendor networks |

## Interface Range — Useful Tip

Instead of configuring each interface one by one, use `interface range` to apply the same command to multiple interfaces at once:

```bash
interface range fastethernet 0/3 - 4
no cdp enable
```

## Tools Required

- Cisco Packet Tracer
