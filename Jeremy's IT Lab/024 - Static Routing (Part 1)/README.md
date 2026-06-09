# CCNA Lab – Static Routing

A Packet Tracer lab focused on configuring static routes on two routers to allow PC1 and PC2 to communicate across three subnets.

## Topology

```
PC1 (.11) — SW1 — R1 (G0/1: .1) — (G0/0: .1) — R2 (G0/0: .2) — (G0/1: .1) — SW2 — PC2 (.12)

192.168.1.0/24       10.0.0.0/24        192.168.2.0/24
```

| Device | Interface | IP Address       |
|--------|-----------|------------------|
| R1     | G0/1      | 192.168.1.1/24   |
| R1     | G0/0      | 10.0.0.1/24      |
| R2     | G0/0      | 10.0.0.2/24      |
| R2     | G0/1      | 192.168.2.1/24   |
| PC1    | —         | 192.168.1.11/24  |
| PC2    | —         | 192.168.2.12/24  |

## Lab Steps

1. Configure and enable G0/0 and G0/1 on R1 and R2 with the addresses above.
2. From PC1, ping progressively toward PC2 — observe which pings succeed and which fail before static routes are added.
3. Repeat from PC2 toward PC1.
4. Add static routes on R1 and R2 to enable full end-to-end connectivity, then verify with ping or tracert.

## Ping Test Results (Before Static Routes)

| From | To              | Result |
|------|-----------------|--------|
| PC1  | R1 G0/1 (192.168.1.1) | ✅ |
| PC1  | R1 G0/0 (10.0.0.1)    | ✅ |
| PC1  | R2 G0/0 (10.0.0.2)    | ❌ (R2 has no return route) |
| PC2  | R2 G0/1 (192.168.2.1) | ✅ |
| PC2  | R2 G0/0 (10.0.0.2)    | ✅ |
| PC2  | R1 G0/0 (10.0.0.1)    | ❌ (R1 has no return route) |

## Key Commands

```bash
# Interface configuration (R1)
interface g0/1
 ip address 192.168.1.1 255.255.255.0
 no shutdown

interface g0/0
 ip address 10.0.0.1 255.255.255.0
 no shutdown

# Static route on R1 (next-hop method)
ip route 192.168.2.0 255.255.255.0 10.0.0.2

# Static route on R2 (exit-interface method)
ip route 192.168.1.0 255.255.255.0 g0/0

# Verification
show ip route
```

## Key Concepts

- For a ping to succeed, **both directions** need a valid route — the destination must know how to reply
- Static routes use either a **next-hop IP** or an **exit interface** as the forwarding method
- Route types in `show ip route`: **C** = connected network, **L** = local (router's own IP), **S** = static
- Routes should point to the **subnet**, not individual host IPs (e.g. `/24` not `/32`)
