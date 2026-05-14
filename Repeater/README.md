# Hub Network — Cisco Packet Tracer

A single-segment LAN where 6 PCs communicate through two switches connected via a repeater.

## Topology

![Network Topology](topologyImg.png)

```
              Repeater0
             /         \
          Switch1      Switch2
         /  |  \       /  |  \
       PC0 PC1 PC2   PC3 PC4 PC5
```

## Devices

| Device | Model | Role |
|---|---|---|
| Repeater0 | Repeater-PT | Extends signal between the two switches |
| Switch1 | 2960-24TT | Connects PC0, PC1, PC2 |
| Switch2 | 2960-24TT | Connects PC3, PC4, PC5 |

## IP Addresses

| Device | IP Address |
|---|---|
| PC0 | 10.10.10.1 |
| PC1 | 10.10.10.2 |
| PC2 | 10.10.10.3 |
| PC3 | 10.10.10.4 |
| PC4 | 10.10.10.5 |
| PC5 | 10.10.10.6 |

## How It Works

- All 6 PCs are on the same network: **10.10.10.0/24**
- **Switch1** and **Switch2** handle local switching within each group
- The **Repeater** connects the two switches, regenerating and forwarding the signal across the segment
- All PCs can ping each other since they share the same subnet

## Concepts Demonstrated

- Layer 1 repeater usage for signal extension
- Layer 2 switching with Cisco 2960
- Single-subnet flat LAN design
