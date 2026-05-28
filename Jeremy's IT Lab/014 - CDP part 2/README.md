# Cisco CDP — Detailed Neighbor Information Lab

A focused CDP lab introducing `show cdp neighbors detail`, `show cdp entry`, and `show version` to discover hardware models and IOS versions of neighboring devices.

## Network Topology

```
SW1 --Gi0/1----Gi0/1-- R1 --Gi0/0----Gi0/0-- R2 --Gi0/1----Gi0/1-- SW2
(2960)         (C1900)                (C2900)                (3560 multilayer)
```

## Lab Tasks

1. Use CDP to identify which interfaces connect all routers and switches
2. Use CDP to identify the hardware model of each neighboring device
3. Use CDP to identify the IOS version running on each neighboring device

## Key Commands

```bash
# Basic neighbor view (model visible under 'Platform' column)
show cdp neighbors

# Detailed view for ALL neighbors (IOS version, IP, capabilities)
show cdp neighbors detail

# Detailed view for ONE specific neighbor
show cdp entry R1

# Verify IOS version directly on the device itself
show version
```

## Device Models Discovered via CDP

| Device | Model       | IOS Version  |
|--------|-------------|--------------|
| R1     | C1900       | 15.1(4)M4    |
| R2     | C2900       | 15.1(4)M4    |
| SW1    | 2960 series | 12.2(25)FX   |
| SW2    | 3560 series | 12.2(1)      |

> SW2's sunburst icon in Packet Tracer indicates it is a **multi-layer switch** (Layer 2 + Layer 3).

## `show cdp neighbors` vs `show cdp neighbors detail`

| Command | Info Shown |
|---------|-----------|
| `show cdp neighbors` | Device name, local/remote interface, platform, capability |
| `show cdp neighbors detail` | All of the above + IP address, IOS version, VTP domain |
| `show cdp entry <name>` | Same as detail but for one specific neighbor only |

> Use `show cdp entry <name>` when a device has many neighbors and you only need info about one — it avoids information overload.

## Tools Required

- Cisco Packet Tracer
