# CCNA Lab – RIP (Routing Information Protocol)

A Packet Tracer lab introducing RIP as a dynamic routing protocol, comparing RIPv1 (classful) vs RIPv2 (classless) behavior.

## Topology

```
PC1/PC2 — SW1 — R2 (F0/0: 10.0.1.1/24) — SW2 — PC3/PC4
                 R2 (F1/0: 10.0.2.1/24)
                 R2 (S2/0: 192.168.1.2/24)
R1 (S2/0: 192.168.1.1/24) ————————————————————^
```

| Network        | Connects         |
|----------------|------------------|
| 192.168.1.0/24 | R1 ↔ R2 (S2/0)   |
| 10.0.1.0/24    | R2 F0/0 ↔ SW1    |
| 10.0.2.0/24    | R2 F1/0 ↔ SW2    |

## Lab Steps

1. Configure RIP (v1) on R1 and R2 — advertise all connected networks.
2. Check R1's routing table — observe what route R2 has advertised.
3. Enable RIP version 2 and disable auto-summary on both routers.
4. Check R1's routing table again — observe the difference.

## Key Commands

```bash
! Enable RIP and advertise networks (both routers)
router rip
 network 192.168.1.0   ! R1

router rip
 network 192.168.1.0   ! R2
 network 10.0.0.0      ! activates RIP on all 10.x interfaces (classful)

! Upgrade to RIPv2 with classless routing
router rip
 version 2
 no auto-summary

! Verification
show ip route
```

## RIPv1 vs RIPv2 — Routing Table Comparison

| Version | Route learned by R1 for R2's networks |
|---------|---------------------------------------|
| RIPv1   | `10.0.0.0/8` — auto-summarized to classful Class A |
| RIPv2   | `10.0.1.0/24` and `10.0.2.0/24` — exact subnets |

## Key Concepts

- **RIP** is a distance-vector protocol using **hop count** as its metric (max 15 hops)
- **RIPv1** is classful — automatically summarizes subnets to their classful boundary (Class A/B/C)
- **RIPv2** is classless — supports VLSM and sends subnet masks in updates; always disable `auto-summary`
- The `network` command activates RIP on interfaces whose IPs fall within that classful network
- RIP AD = **120**; you won't see it used in production (OSPF/EIGRP preferred), but it's exam-relevant
