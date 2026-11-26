# Cisco DNS Basic Configuration Lab

A simple hands-on lab to practice DNS with DHCP using one router, three switches, two PCs and three servers in Packet Tracer.

## Network Topology

```
                 DNS1 (.100)
                     |
                    SW3 (VLAN1 20.0.0.2)
                     |
                 G0/3 (.1)  20.0.0.0/24
                     |
SW1 ---- G0/0 (.1) -- R1 -- G0/1 (.1) ---- SW2 ---- SRV1 (.101), SRV2 (.102)
  |     192.168.1.0/24                  10.0.0.0/24
PC1, PC2
```

| Network        | Devices                                                |
|----------------|--------------------------------------------------------|
| 192.168.1.0/24 | PC1, PC2, SW1 (VLAN1 192.168.1.2), gateway R1 G0/0 (.1) |
| 10.0.0.0/24    | SRV1 (.101), SRV2 (.102), SW2 (VLAN1 10.0.0.2), R1 G0/1 (.1) |
| 20.0.0.0/24    | DNS1 (.100), SW3 (VLAN1 20.0.0.2), R1 G0/3 (.1)        |

## Lab Tasks

1. Configure the following DHCP pool on R1
   * Pool name: 1pool
   * Network: 192.168.1.0/24
   * Default gateway: 192.168.1.1
   * Excluded address range: 192.168.1.1 - 192.168.1.10
2. From PC1, attempt to ping SRV1 by IP address, then by name. Does either fail?
3. Add 20.0.0.100 as a DNS server to the 1pool DHCP pool
4. From PC1, ping SRV1 and SRV2 by name. Do the pings succeed?
5. On SW1, attempt to ping SRV1 by IP address and by name
6. On SW1, manually specify R1 as the default gateway and DNS1 as the DNS server, then attempt the pings again

## Key Commands

R1 (DHCP server)

```
ip dhcp excluded-address 192.168.1.1 192.168.1.10

ip dhcp pool 1pool
 network 192.168.1.0 255.255.255.0
 default-router 192.168.1.1

# Later, add the DNS server
ip dhcp pool 1pool
 dns-server 20.0.0.100
```

SW1 (manual gateway and DNS)

```
ip default-gateway 192.168.1.1
ip name-server 20.0.0.100
```

Verify

```
# On PC1
ipconfig /release
ipconfig /renew
ping 10.0.0.101
ping SRV1
ping SRV2

# On SW1
ping 10.0.0.101
ping SRV1
```

## Notes

* Pinging by IP works before DNS is set, pinging by name fails until PC1 learns the DNS server
* Release and renew on PC1 after changing the pool to pick up the DNS server
* A switch does not get gateway or DNS information from DHCP here, so it must be set manually
* The first ping by name can be slow while the name is being resolved

## Tools Required

* Cisco Packet Tracer
