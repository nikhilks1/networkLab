# Cisco Serial Connection Configuration Lab

A hands-on lab to practice configuring a serial WAN connection between two Cisco 819HGW routers using CDP, DCE/DTE identification, clock rate, and IP addressing in Packet Tracer.

## Network Topology

```
SW1 (2960-24TT)               SW2 (2960-24TT)
     |  Fa0/1                       Fa0/1  |
     |  Fa0                           Fa0  |
     R1 (819HGW) ---Se0-----Se0--- R2 (819HGW)
                      DCE     DTE
```

## Lab Tasks

1. Use CDP to discover which interfaces connect the routers and switches
2. Identify which end of the serial link is DCE and which is DTE
3. Set the clock rate on the DCE side (R1) to 64,000 bps
4. Assign IP addresses to the serial interfaces of R1 and R2
5. Test connectivity between the routers with a ping

## Key Commands

```bash
# Discover neighbors and interface mappings
show cdp neighbors

# Check interface status
show ip interface brief

# Bring up a shutdown interface
configure terminal
interface serial 0
no shutdown

# Identify DCE / DTE
show controllers serial 0

# Set clock rate on DCE side (R1 only)
interface serial 0
clock rate 64000

# Assign IP addresses
# On R1
interface serial 0
ip address 192.168.1.1 255.255.255.0

# On R2
interface serial 0
ip address 192.168.1.2 255.255.255.0

# Test connectivity
ping 192.168.1.1
```

## Notes

- Serial interfaces are **shutdown by default** — always run `no shutdown` on both ends.
- Only the **DCE side** needs a clock rate configured; the DTE side inherits it.
- Use `show controllers serial 0` to confirm DCE/DTE — look for the keyword `DCE` or `DTE` in the output.
- CDP(Cisco Discovery Protocol) only works between directly connected Cisco devices and operates at Layer 2.

## Tools Required

- Cisco Packet Tracer
