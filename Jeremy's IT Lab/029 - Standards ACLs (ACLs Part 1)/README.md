# CCNA Lab – Standard Access Control Lists (ACLs)

A Packet Tracer lab introducing numbered standard ACLs to restrict traffic based on source address.

## Topology

```
PC1/PC2 — SW1 — R1 (F0/0) — 192.168.1.0/24
                 R1 (F1/0) — 192.168.2.0/24 — SW2 — PC3/PC4
                 R1 (S2/0) — 12.0.0.0/24 — R2 (S2/0)
                                            R2 (F0/0) — 192.168.3.0/24 — SW3 — SRV1 (.100)
```

## Requirements

1. Only the `192.168.1.0/24` network can access SRV1.
2. PC4 cannot communicate with the `192.168.1.0/24` network.

## ACL 1 — Restrict access to SRV1 (configured on R2)

Standard ACLs should be applied as close to the **destination** as possible.

```bash
access-list 1 permit 192.168.1.0 0.0.0.255

interface f0/0
 ip access-group 1 out
```

- Implicit `deny any` at the end blocks all other traffic to SRV1.

## ACL 2 — Block PC4 from reaching 192.168.1.0/24 (configured on R1)

```bash
access-list 1 deny host 192.168.2.14
access-list 1 permit any

interface f0/0
 ip access-group 1 out
```

- `deny host` blocks only PC4; `permit any` allows everything else through.
- Applied **outbound on F0/0** (closest to destination network 192.168.1.0/24).

## Verification

```bash
show access-lists
show ip interface f0/0   ! confirm ACL applied
```

| From | To              | Result |
|------|-----------------|--------|
| PC1  | SRV1            | ✅ |
| PC3  | SRV1            | ❌ (Destination unreachable) |
| PC3  | PC1             | ✅ |
| PC4  | PC1             | ❌ (Destination unreachable) |

## Key Concepts

- **Standard ACLs** — numbered 1–99, filter based on **source address only**
- **Extended ACLs** — numbered 100–199, filter on source, destination, ports, etc. (covered in future labs)
- **Wildcard mask** — inverse of subnet mask (e.g. `/24` → `0.0.0.255`)
- **Placement rule** — standard ACLs go as close to the **destination** as possible to avoid unintended blocking
- ACLs are processed **top to bottom**; first match wins
- Every ACL has an **implicit deny any** at the end
- `ip access-group <num> out` — applies the ACL to traffic leaving the interface
