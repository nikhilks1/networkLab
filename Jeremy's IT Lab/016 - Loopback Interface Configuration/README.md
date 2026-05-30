# Cisco Loopback Interface Lab

A simple introductory lab to understand, configure, and remove loopback interfaces on two Cisco routers in Packet Tracer.

## Network Topology

```
R1 --G0/0----G0/0-- R2
192.168.1.1/24    192.168.1.2/24

Loopback0 on R1: 1.1.1.1/32
Loopback0 on R2: 2.2.2.2/32
```

## What is a Loopback Interface?

- A **logical (virtual) interface** — it does not physically exist on the router
- **Never goes down** unless manually shut with `shutdown`
- Used for testing, router identification, and as a stable reference IP in routing protocols
- Can be **deleted** via CLI (unlike physical interfaces)
- Uses `/32` subnet mask since it represents a single host address

## Lab Tasks

1. Set IP addresses on the physical G0/0 interfaces and enable them
2. Create a loopback interface on each router (1.1.1.1/32 on R1, 2.2.2.2/32 on R2)
3. Ping both the local loopback and the remote router's loopback from each router
4. Remove the loopback interfaces from both routers

## Key Commands

```bash
# Configure physical interface
configure terminal
interface gigabitethernet 0/0
ip address 192.168.1.1 255.255.255.0
no shutdown

# Create loopback interface (up by default, no 'no shutdown' needed)
interface loopback 0          # or: interface l0
ip address 1.1.1.1 255.255.255.255

# Verify interfaces
show ip interface brief

# Check routing table
show ip route

# Delete loopback interface
no interface loopback 0

# Attempt to delete physical interface (will fail)
no interface gigabitethernet 0/0
# Output: "Removal of physical interfaces is not permitted"
```

## Loopback vs Physical Interface

| Feature | Physical Interface | Loopback Interface |
|---------|-------------------|-------------------|
| Exists physically | ✅ Yes | ❌ No (logical) |
| Can go down | ✅ Yes (cable/failure) | ❌ Never (unless shutdown) |
| Can be deleted via CLI | ❌ No | ✅ Yes |
| Typical subnet mask | Any | /32 (single host) |

## Note on Pinging Remote Loopback

Routers cannot ping a remote loopback by default — they need a **route** to that network. In this lab, static routes were pre-configured to allow cross-router loopback pings. Routing will be covered in later labs.

## Tools Required

- Cisco Packet Tracer
