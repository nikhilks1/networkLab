# Cisco Extended ACL Lab

A simple hands-on lab to practice extended ACLs using two routers and three LANs in Packet Tracer.

## Network Topology

```
PC1 (.11) --\
             SW1 -- F0/0 R1 S2/0 ---- 12.0.0.0/24 ---- S2/0 R2 F0/0 -- SW3 -- SRV1 (.100)
PC2 (.12) --/        |                                                  |
                    F1/0                                                -- SRV2 (.101)
                     |
                    SW2 -- PC3 (.13), PC4 (.14)
```

| Network        | Devices                  | Gateway       |
|----------------|--------------------------|---------------|
| 192.168.1.0/24 | PC1 (.11), PC2 (.12)     | R1 F0/0 (.1)  |
| 192.168.2.0/24 | PC3 (.13), PC4 (.14)     | R1 F1/0 (.1)  |
| 12.0.0.0/24    | R1 S2/0 (.1) - R2 S2/0 (.2) | -          |
| 192.168.3.0/24 | SRV1 (.100), SRV2 (.101) | R2 F0/0 (.1)  |

## Lab Tasks

Configure extended ACLs to achieve the following:

1. Only PC1 can access SRV1
2. Only hosts on the 192.168.2.0/24 network can access SRV2
3. Verify with ping from each PC
4. Check the ACL hit counts

## Key Commands

Routing (needed first so every network can reach the others)

```
# R1
ip route 192.168.3.0 255.255.255.0 12.0.0.2

# R2
ip route 192.168.1.0 255.255.255.0 12.0.0.1
ip route 192.168.2.0 255.255.255.0 12.0.0.1
```

ACL on R2 (placed close to the destination)

```
access-list 100 permit ip host 192.168.1.11 host 192.168.3.100
access-list 100 permit ip 192.168.2.0 0.0.0.255 host 192.168.3.101
access-list 100 deny ip any host 192.168.3.100
access-list 100 deny ip any host 192.168.3.101
access-list 100 permit ip any any

interface f0/0
 ip access-group 100 out
```

Verify

```
show access-lists
show running-config
```

## Expected Result

| Source        | SRV1 (.100) | SRV2 (.101) |
|---------------|-------------|-------------|
| PC1           | Allowed     | Denied      |
| PC2           | Denied      | Denied      |
| PC3 / PC4     | Denied      | Allowed     |

## Tools Required

* Cisco Packet Tracer
