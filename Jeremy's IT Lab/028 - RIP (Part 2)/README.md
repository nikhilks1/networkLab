# CCNA Lab – RIPv2 (Multi-Router)

A Packet Tracer lab configuring RIPv2 across four routers for full network connectivity — demonstrating how much simpler dynamic routing is compared to static routes.

## Topology

```
PC1 — SW1 — R1 (G0/2) — 10.0.0.0/24
             R1 (G0/0) — 12.0.0.0/24 — R2
             R1 (G0/1) — 13.0.0.0/24 — R3

PC2 — SW2 — R2 (G0/2) — 20.0.0.0/24
             R2 (G0/1) — 24.0.0.0/24 — R4

PC3 — SW3 — R3 (G0/2) — 30.0.0.0/24
             R3 (G0/0) — 34.0.0.0/24 — R4

PC4 — SW4 — R4 (G0/2) — 40.0.0.0/24
```

| Network     | Connects  |
|-------------|-----------|
| 10.0.0.0/24 | R1 ↔ SW1  |
| 20.0.0.0/24 | R2 ↔ SW2  |
| 30.0.0.0/24 | R3 ↔ SW3  |
| 40.0.0.0/24 | R4 ↔ SW4  |
| 12.0.0.0/24 | R1 ↔ R2   |
| 13.0.0.0/24 | R1 ↔ R3   |
| 24.0.0.0/24 | R2 ↔ R4   |
| 34.0.0.0/24 | R3 ↔ R4   |

## Lab Task

All IP addresses are pre-configured. Enable RIPv2 on all four routers and disable routing updates on switch-facing interfaces (passive interface).

## Key Commands

```bash
! Example: R1 (repeat pattern on R2, R3, R4 with their networks)
router rip
 version 2
 no auto-summary
 network 10.0.0.0
 network 12.0.0.0
 network 13.0.0.0
 passive-interface g0/2   ! disable updates toward switch/PCs

! Verification
show ip route
show ip protocols
```

## RIP Timers (from `show ip protocols`)

| Timer     | Default  | Purpose                                      |
|-----------|----------|----------------------------------------------|
| Update    | 30 sec   | How often RIP sends routing updates          |
| Invalid   | 180 sec  | Time before a route is marked invalid        |
| Hold-down | 180 sec  | Prevents accepting worse routes after change |
| Flush     | 240 sec  | Time before route is removed from table      |

## Key Concepts

- **`passive-interface`** stops RIP updates from being sent out toward switches/PCs while still advertising those networks to other routers
- **`no auto-summary`** ensures specific subnets are advertised instead of classful summaries
- **Convergence** — the process of all routers learning and agreeing on the full network topology after RIP updates propagate
- RIPv2 vs static routing: 4 routers needed only 4 simple `router rip` blocks vs 20 manually configured static routes
