# CCNA Lab – Floating Static Route

A Packet Tracer lab demonstrating how to configure a floating static route as a backup path when a primary RIP-learned route fails.

## Topology

```
PC1/PC2/PC3 — SW1 — R1 (F0/0) — 10.0.0.0/24
                     R1 (S3/0) — 192.168.12.0/24 — R2 (primary path to 10.0.0.0/24 via RIP)
                     R1 (S2/0) — 192.168.13.0/24 — R3 (backup path, no RIP)
                                  R2 (S2/0) — 192.168.23.0/24 — R3
```

- RIP is configured on all interfaces **except** between R1 and R3 (S2/0 links)
- R1 learns the route to `10.0.0.0/24` via RIP through R2 (S3/0) — AD 120, metric 2
- Goal: configure a floating static route via R3 (S2/0) that activates only if the R1–R2 link fails

## Lab Task

Configure a single floating static route on R1 to reach `10.0.0.0/24` via R3, with an administrative distance higher than RIP (120), so it only appears in the routing table when the RIP route is gone.

## Key Command

```bash
! Floating static route on R1 (AD = 121, higher than RIP's 120)
ip route 10.0.0.0 255.255.255.0 192.168.13.3 121

! Verify — floating route should NOT appear while RIP route is active
show ip route

! Simulate failure — shut R1's S3/0
interface s3/0
 shutdown

! Verify again — floating static route should now appear
show ip route

! Trace path (should go directly via R3)
traceroute 10.0.0.11

! Restore interface
interface s3/0
 no shutdown
```

## Key Concepts

- **Administrative Distance (AD)** — measures route trustworthiness; lower AD wins. Static = 1, RIP = 120
- **Floating static route** — a static route with a manually set AD *higher* than the primary route's AD, so it stays hidden until the primary route disappears
- **Metric** — used to compare routes of the *same* AD (e.g. RIP uses hop count). AD is always compared first
- A normal static route (AD 1) would override the RIP route — setting AD to 121 keeps it as a true backup
- When the primary interface recovers, RIP re-advertises the route and the floating static is automatically removed from the routing table
