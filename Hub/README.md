# Hub Network — Cisco Packet Tracer

6 PCs connected to a single Hub using straight-through cables.
Hubs are non inteligent devices and it will send or forwards data that is sent to them to all the other ports and the recivers have to drop the packet if it is not for them also that is the reason why we are moving from hubs to switches and hubs works on layer 1 - physical layer which is used in LAN

## Topology

```
              [ Hub0 ]
   /    /    /    \    \    \
 PC0  PC1  PC2  PC3  PC4  PC5
```

## IP Configuration

| Device | IP Address  | Subnet Mask   |
|--------|-------------|---------------|
| PC0    | 10.10.10.1  | 255.255.255.0 |
| PC1    | 10.10.10.2  | 255.255.255.0 |
| PC2    | 10.10.10.3  | 255.255.255.0 |
| PC3    | 10.10.10.4  | 255.255.255.0 |
| PC4    | 10.10.10.5  | 255.255.255.0 |
| PC5    | 10.10.10.6  | 255.255.255.0 |

## CLI Setup

**Each PC → Desktop → Command Prompt:**
```
ipconfig 10.10.10.X 255.255.255.0
```

## Test

```
ping 10.10.10.2
```

## Author
**Nikhil KS** — [@nikhilks1](https://github.com/nikhilks1)
