# Named Standard ACL Lab

Packet Tracer practice lab on configuring named standard ACLs (Jeremy's IT Lab, CCNA).

## Topology

| Network | Connected to | Devices |
|---|---|---|
| 192.168.1.0/24 | R1 F0/0 (.1) via SW1 | PC1 (.11), PC2 (.12) |
| 192.168.2.0/24 | R1 F1/0 (.1) via SW2 | PC3 (.13), PC4 (.14) |
| 12.0.0.0/24 | R1 S2/0 (.1) to R2 S2/0 (.2) | Serial link |
| 192.168.3.0/24 | R2 F0/0 (.1) via SW3 | SRV1 (.100), SRV2 (.101) |

## Requirements

1. Hosts in 192.168.1.0/24 and 192.168.2.0/24 cannot communicate with each other.
2. Hosts in 192.168.2.0/24 cannot access 192.168.3.0/24.

## Notes

- Standard ACLs only filter on the source address.
- Place them as close to the destination as possible.
- Named ACLs are created with `ip access-list standard <name>` and have their own config mode.
- There is an implicit deny at the end, so every ACL needs `permit any`.

I named the ACLs after the direction they block, for example `2-1` blocks traffic from network 2 to network 1.

## Configuration

### R1

```
ip access-list standard 2-1
 deny 192.168.2.0 0.0.0.255
 permit any
exit
ip access-list standard 1-2
 deny 192.168.1.0 0.0.0.255
 permit any
exit

interface f0/0
 ip access-group 2-1 out
interface f1/0
 ip access-group 1-2 out
```

### R2

```
ip access-list standard 2-3
 deny 192.168.2.0 0.0.0.255
 permit any
exit

interface f0/0
 ip access-group 2-3 out
```

## Testing

| From | To | Result |
|---|---|---|
| PC1 | 192.168.2.x | Destination host unreachable |
| PC3 | 192.168.1.x | Destination host unreachable |
| PC3 | SRV1 (192.168.3.100) | Destination host unreachable |
| PC3 | SRV2 (192.168.3.101) | Destination host unreachable |

Check the ACLs with `show access-lists`.
