# Switch Network — Cisco Packet Tracer

5 PCs connected to a single Switch using straight-through cables.

## Topology

```
              [ Switch0 ]
   /    /    /    \    \
 PC0  PC1  PC2  PC3  PC4
```

## IP Configuration

| Device | IP Address  | Subnet Mask   | Port  |
|--------|-------------|---------------|-------|
| PC0    | 10.10.10.1  | 255.255.255.0 | Fa0/1 |
| PC1    | 10.10.10.2  | 255.255.255.0 | Fa0/2 |
| PC2    | 10.10.10.3  | 255.255.255.0 | Fa0/3 |
| PC3    | 10.10.10.4  | 255.255.255.0 | Fa0/4 |
| PC4    | 10.10.10.5  | 255.255.255.0 | Fa0/5 |

## CLI Setup

**Each PC → Desktop → Command Prompt:**
```
ipconfig 10.10.10.X 255.255.255.0
```

## Test

```
ping 10.10.10.2
```

## View MAC Address Table

```
Switch> sh mac-address-table
```

**Sample Output:**

```
Vlan    Mac Address       Type      Ports
----    -----------       --------  -----
  1     0001.420a.7578    DYNAMIC   Fa0/3
  1     000a.f308.c821    DYNAMIC   Fa0/1
  1     000a.f367.b917    DYNAMIC   Fa0/4
```

## About the Switch

- **Layer:** Works at **Layer 2 — Data Link Layer** of the OSI model
- **Memory:** Has a **MAC Address Table (CAM Table)** stored in memory — learns and remembers which device is on which port
- **Unicast:** Sends frames directly to the destination port only — no unnecessary traffic
- **Multicast:** Forwards frames only to ports that are part of the multicast group
- **Broadcast:** Sends to all ports only when needed (e.g., ARP requests)
- **Full Duplex:** Each port can **send and receive simultaneously** — no collisions, no waiting
- **Efficient:** Each port is its own **collision domain** — far more efficient than a hub

> A switch is smarter, faster, and more efficient than a hub because it learns MAC addresses, avoids unnecessary traffic, and supports full duplex communication.

## Author
**Nikhil KS** — [@nikhilks1](https://github.com/nikhilks1)

## Security Risks

- **MAC Flooding:** Attacker fills the CAM table with fake MACs — switch acts like a hub and broadcasts all traffic
- **MAC Spoofing:** Attacker fakes a MAC address to impersonate another device
- **VLAN Hopping:** Attacker gains access to traffic from other VLANs
- **Broadcast Storms:** Too many broadcasts can overwhelm the network

> Use **Port Security** on the switch to limit MAC addresses per port and reduce these risks.
```
Switch(config-if)# switchport port-security maximum 1
Switch(config-if)# switchport port-security violation shutdown
```
