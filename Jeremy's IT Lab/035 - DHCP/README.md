# Cisco DHCP Basic Configuration Lab

A simple hands-on lab to practice DHCP server, client and relay agent configuration using two routers in Packet Tracer.

## Network Topology

```
PC1, PC2 -- SW1 -- G0/1 R1 G0/0 ---- 192.168.12.0/24 ---- G0/0 R2 G0/1 -- SW2 -- PC3, PC4
              10.0.0.0/24 (.1)                                   (.1) 20.0.0.0/24
```

| Network         | Devices                                |
|-----------------|----------------------------------------|
| 10.0.0.0/24     | PC1, PC2 (gateway R1 G0/1 .1)          |
| 192.168.12.0/24 | R1 G0/0 (.1) - R2 G0/0 (DHCP client)   |
| 20.0.0.0/24     | PC3, PC4 (gateway R2 G0/1 .1)          |

## Lab Tasks

1. Configure three DHCP pools on R1

| Pool   | Range           | Default gateway | DNS server | Excluded addresses      |
|--------|-----------------|-----------------|------------|-------------------------|
| 10pool | 10.0.0.0/24     | 10.0.0.1        | 10.0.0.1   | 10.0.0.1 - 10.0.0.10    |
| 20pool | 20.0.0.0/24     | 20.0.0.1        | 20.0.0.1   | 20.0.0.1 - 20.0.0.10    |
| 12pool | 192.168.12.0/24 | -               | -          | -                       |

2. Configure R2's G0/0 interface as a DHCP client, then enable the interface
3. Configure R2's G0/1 interface as a DHCP relay agent so hosts on 20.0.0.0/24 can get IP addresses

## Key Commands

R1 (DHCP server)

```
ip dhcp pool 10pool
 network 10.0.0.0 255.255.255.0
 default-router 10.0.0.1
 dns-server 10.0.0.1
exit
ip dhcp excluded-address 10.0.0.1 10.0.0.10

ip dhcp pool 20pool
 network 20.0.0.0 255.255.255.0
 default-router 20.0.0.1
 dns-server 20.0.0.1
exit
ip dhcp excluded-address 20.0.0.1 20.0.0.10

ip dhcp pool 12pool
 network 192.168.12.0 255.255.255.0
```

R2 (client and relay agent)

```
interface g0/0
 ip address dhcp
 no shutdown

interface g0/1
 ip helper-address 192.168.12.1
```

Verify on a PC

```
ipconfig /release
ipconfig /renew
```

## Notes

* A PC with no DHCP server gets an APIPA address in the 169.254.0.0/16 range
* Excluded addresses are set in global configuration mode, not inside the pool
* The relay agent goes on the Layer 3 interface closest to the hosts

## Tools Required

* Cisco Packet Tracer
