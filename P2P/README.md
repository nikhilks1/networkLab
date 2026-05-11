# Peer to Peer Connection — Cisco Packet Tracer

A simple P2P network connecting two PCs directly using a crossover cable.

## Topology

```
[ PC0 ] --------crossover cable-------- [ PC2 ]
10.10.10.1                              10.10.10.2
255.0.0.0                               255.0.0.0
```

## CLI Setup

**PC0:**
```
ipconfig 10.10.10.1 255.0.0.0
```

**PC2:**
```
ipconfig 10.10.10.2 255.0.0.0
```

## Test

```
ping 10.10.10.2
```

## Author
**Nikhil KS** — [@nikhilks1](https://github.com/nikhilks1)
