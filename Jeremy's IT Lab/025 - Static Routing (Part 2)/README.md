# CCNA Lab – Static Routing (Multi-Router)

A Packet Tracer lab extending static routing concepts across a four-router topology. All IP addresses are pre-configured — the task is to add static routes on every router for full end-to-end connectivity.

## Topology

```
PC1/PC11 — SW1 — R1 (F0/0) — 10.0.1.0/24
                  R1 (S3/0) — 192.168.12.0/24 — R2 (S3/0)
                  R1 (S2/0) — 192.168.13.0/24 — R3 (S2/0)
                                                  R2 (S2/0) — 192.168.24.0/24 — R4 (S2/0)
                                                  R3 (S3/0) — 192.168.34.0/24 — R4 (S3/0)
PC2/PC22 — SW2 — R2 (F0/0) — 10.0.2.0/24
PC3/PC33 — SW3 — R3 (F0/0) — 10.0.3.0/24
PC4/PC44 — SW4 — R4 (F0/0) — 10.0.4.0/24
```

| Network          | Connects          |
|------------------|-------------------|
| 10.0.1.0/24      | R1 ↔ SW1          |
| 10.0.2.0/24      | R2 ↔ SW2          |
| 10.0.3.0/24      | R3 ↔ SW3          |
| 10.0.4.0/24      | R4 ↔ SW4          |
| 192.168.12.0/24  | R1 ↔ R2           |
| 192.168.13.0/24  | R1 ↔ R3           |
| 192.168.24.0/24  | R2 ↔ R4           |
| 192.168.34.0/24  | R3 ↔ R4           |

## Lab Task

All IP addresses are pre-configured. Configure static routes on R1, R2, R3, and R4 so that every PC can ping every other point in the network (5 routes per router = 20 total).

## Static Routes

```bash
! R1 — add routes to non-directly-connected networks
ip route 10.0.2.0 255.255.255.0 192.168.12.2   ! via R2
ip route 10.0.3.0 255.255.255.0 192.168.13.3   ! via R3
ip route 10.0.4.0 255.255.255.0 192.168.12.2   ! via R2 (or R3)
ip route 192.168.24.0 255.255.255.0 192.168.12.2
ip route 192.168.34.0 255.255.255.0 192.168.13.3

! R2
ip route 10.0.1.0 255.255.255.0 192.168.12.1   ! via R1
ip route 10.0.3.0 255.255.255.0 192.168.12.1   ! via R1 (or R4)
ip route 10.0.4.0 255.255.255.0 192.168.24.4   ! via R4
ip route 192.168.13.0 255.255.255.0 192.168.12.1
ip route 192.168.34.0 255.255.255.0 192.168.24.4

! R3
ip route 10.0.1.0 255.255.255.0 192.168.13.1   ! via R1
ip route 10.0.2.0 255.255.255.0 192.168.13.1   ! via R1 (or R4)
ip route 10.0.4.0 255.255.255.0 192.168.34.4   ! via R4
ip route 192.168.12.0 255.255.255.0 192.168.13.1
ip route 192.168.24.0 255.255.255.0 192.168.34.4

! R4
ip route 10.0.1.0 255.255.255.0 192.168.24.2   ! via R2 (or R3)
ip route 10.0.2.0 255.255.255.0 192.168.24.2   ! via R2
ip route 10.0.3.0 255.255.255.0 192.168.34.3   ! via R3
ip route 192.168.12.0 255.255.255.0 192.168.24.2
ip route 192.168.13.0 255.255.255.0 192.168.34.3

! Verification
show ip route
```

## Key Concepts

- Each router only needs routes to **non-directly-connected** networks
- Where two equal-cost paths exist (e.g. R1→R4 via R2 or R3), either next-hop works — pick one consistently
- This lab demonstrates why static routing doesn't scale: 4 routers → 20 manual routes; dynamic routing protocols like OSPF handle this automatically
