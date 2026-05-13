# Inter-Network (Router + 2 Switches) — Cisco Packet Tracer

2 LANs connected through a Router using two switches.

## Topology

```
              [ Router0 - 2811 ]
             Fa0/0          Fa0/1
              |                |
          [ Switch0 ]      [ Switch1 ]
         /    |    \        /   |   \
       PC0   PC1   PC2   PC3  PC4  PC5
```

## LAN 1 — Switch0

| Device | IP Address | Subnet Mask | Gateway   |
|--------|------------|-------------|-----------|
| PC0    | 10.0.0.1   | 255.0.0.0   | 10.0.0.4 |
| PC1    | 10.0.0.2   | 255.0.0.0   | 10.0.0.4 |
| PC2    | 10.0.0.3   | 255.0.0.0   | 10.0.0.4 |

## LAN 2 — Switch1

| Device | IP Address    | Subnet Mask     | Gateway        |
|--------|---------------|-----------------|----------------|
| PC3    | 192.168.1.1   | 255.255.255.0   | 192.168.1.4  |
| PC4    | 192.168.1.2   | 255.255.255.0   | 192.168.1.4  |
| PC5    | 192.168.1.3   | 255.255.255.0   | 192.168.1.4  |

## Router Interface Config

```
Router> en
Router# conf t

Router(config)# int fa0/0
Router(config-if)# ip address 10.0.0.254 255.0.0.0
Router(config-if)# no shutdown

Router(config)# int fa0/1
Router(config-if)# ip address 192.168.1.254 255.255.255.0
Router(config-if)# no shutdown
```

## PC CLI Setup

**Each PC → Desktop → Command Prompt:**
```
ipconfig 10.0.0.X 255.0.0.0          (LAN1 PCs)
ipconfig 192.168.1.X 255.255.255.0   (LAN2 PCs)
```

> Set the **Default Gateway** on each PC to the router's interface IP of that LAN.

## Test

```
ping 192.168.1.1    (from PC0 to PC3 — cross-network)
ping 10.0.0.1       (from PC3 to PC0 — cross-network)
```

## About the Router

- Works at **Layer 3 — Network Layer** of the OSI model
- Routes traffic **between different networks** using IP addresses
- Each interface connects to a separate LAN
- Acts as the **Default Gateway** for all PCs

## Author
**Nikhil KS** — [@nikhilks1](https://github.com/nikhilks1)
