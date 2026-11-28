# Cisco Dynamic NAT Lab

A simple hands-on lab to practice dynamic NAT using two routers, two switches, three PCs and a server in Packet Tracer.

## Network Topology

```
PC1 (.11) --\
PC2 (.12) ---- SW1 ---- G0/0 R1 S0/3/0 ---- INTERNET ---- S0/3/0 R2 G0/0 ---- SW2 ---- SRV1 (.100)
PC3 (.13) --/    192.168.1.0/24 (.1)  (.1)   1.2.3.0/24   (.2)    (.1)    1.1.1.0/24
```

| Network        | Devices                                       |
|----------------|-----------------------------------------------|
| 192.168.1.0/24 | PC1 (.11), PC2 (.12), PC3 (.13), R1 G0/0 (.1) |
| 1.2.3.0/24     | R1 S0/3/0 (.1) - R2 S0/3/0 (.2)               |
| 1.1.1.0/24     | SRV1 (.100), R2 G0/0 (.1)                     |

## Lab Tasks

1. RIP has been configured so R1 and R2 can reach their inside networks. Why can't PC1, PC2 and PC3 ping SRV1?
   (Hint: the serial connection between R1 and R2 simulates the Internet with ACLs)
2. Configure dynamic NAT on R1 to translate the 192.168.1.0/24 subnet to the address range 1.2.3.10 - 1.2.3.20
3. Ping from each PC to SRV1, then use a show command on R1 to check the translations

## Key Commands

R1

```
# Mark the inside and outside interfaces
interface g0/0
 ip nat inside
interface s0/3/0
 ip nat outside
exit

# ACL that identifies the addresses to translate
access-list 1 permit 192.168.1.0 0.0.0.255

# Pool of outside addresses
ip nat pool pool1 1.2.3.10 1.2.3.20 netmask 255.255.255.0

# Link the ACL to the pool
ip nat inside source list 1 pool pool1
```

Verify

```
# On each PC
ping 1.1.1.100

# On R1
show ip nat translations

# Clear the dynamic translations
clear ip nat translation *
```

## Notes

* Unlike static NAT (one-to-one), dynamic NAT assigns outside addresses from a pool as needed
* The ACL only picks which traffic gets translated. Traffic it denies is not blocked, just not translated
* In `show ip nat translations`, "inside local" is the original private address and "inside global" is the translated public address

## Tools Required

* Cisco Packet Tracer
