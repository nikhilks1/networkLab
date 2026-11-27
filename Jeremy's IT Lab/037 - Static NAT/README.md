# Cisco Static NAT Lab

A simple hands-on lab to practice static NAT using two routers, two switches, three PCs and a server in Packet Tracer.

## Network Topology

```
PC1 (.11) --\
PC2 (.12) ---- SW1 ---- G0/0 R1 S0/3/0 ---- INTERNET ---- S0/3/0 R2 G0/0 ---- SW2 ---- SRV1 (.100)
PC3 (.13) --/    192.168.1.0/24 (.1)  (.1)   1.2.3.0/24   (.2)    (.1)    1.1.1.0/24
```

| Network        | Devices                                  |
|----------------|------------------------------------------|
| 192.168.1.0/24 | PC1 (.11), PC2 (.12), PC3 (.13), R1 G0/0 (.1) |
| 1.2.3.0/24     | R1 S0/3/0 (.1) - R2 S0/3/0 (.2)          |
| 1.1.1.0/24     | SRV1 (.100), R2 G0/0 (.1)                |

## Lab Tasks

1. RIP has been configured so R1 and R2 can reach their inside networks. Why can't PC1, PC2 and PC3 ping SRV1?
   (Hint: the serial connection between R1 and R2 simulates the Internet with ACLs)
2. Configure static NAT on R1 to translate PC1, PC2 and PC3 to 1.2.3.11, 1.2.3.12 and 1.2.3.13
3. Attempt to ping SRV1 from each PC again. Do the pings succeed?

## Key Commands

R1

```
# Mark the inside and outside interfaces
interface g0/0
 ip nat inside
interface s0/3/0
 ip nat outside
exit

# Static translations (inside private address, then outside public address)
ip nat inside source static 192.168.1.11 1.2.3.11
ip nat inside source static 192.168.1.12 1.2.3.12
ip nat inside source static 192.168.1.13 1.2.3.13
```

Verify

```
# On each PC
ping 1.1.1.100

# On R1
show ip nat translations
```

## Notes

* The pings fail at first because private addresses (192.168.1.0/24) cannot be routed over the Internet, so the reply from SRV1 is dropped
* Static NAT is a one-to-one mapping between an inside address and an outside address
* R1 translates the source address on the way out and the destination address on the way back

## Tools Required

* Cisco Packet Tracer
