# Cisco VLAN Naming & Trunk Configuration Lab

A VLAN review lab introducing two new skills — manually creating VLANs with custom names and saving the running configuration — on two Cisco 2960-24TT switches.

## Network Topology

```
PC1 (192.168.0.1/24) --F0/2--|                    |--F0/2-- PC3 (192.168.0.3/24)
                             SW1 --F0/1----F0/1-- SW2
PC2 (192.168.0.2/24) --F0/3--|                    |--F0/3-- PC4 (192.168.0.4/24)
```

| Device | VLAN         | VLAN Name  |
|--------|-------------|------------|
| PC1    | VLAN 13     | Management |
| PC3    | VLAN 13     | Management |
| PC2    | VLAN 24     | Engineering|
| PC4    | VLAN 24     | Engineering|

## Lab Tasks

1. Set hostnames to `SW1` and `SW2`
2. Manually create VLAN 13 (Management) and VLAN 24 (Engineering) on each switch
3. Assign PC1 & PC3 to VLAN 13, and PC2 & PC4 to VLAN 24
4. Configure a trunk link between SW1 and SW2
5. Save the running configuration
6. Test — PCs in the same VLAN should be able to ping each other

## Key Commands

```bash
# Set hostname
configure terminal
hostname SW1

# Manually create VLANs and assign names
vlan 13
name Management

vlan 24
name Engineering

# Verify VLANs
do show vlan brief

# Assign ports to VLANs (switchport mode access is default, optional)
interface fastethernet 0/2
switchport access vlan 13

interface fastethernet 0/3
switchport access vlan 24

# Configure trunk between switches
interface fastethernet 0/1
switchport mode trunk

# Verify trunk
show interfaces trunk

# Save configuration
write
```

## New Commands in This Lab

| Command | Purpose |
|---------|---------|
| `vlan <id>` | Creates a VLAN and enters VLAN config mode |
| `name <name>` | Assigns a name to the VLAN |
| `show vlan brief` | Summarised view of VLANs and their assigned ports |
| `show interfaces trunk` | Shows trunk ports, encapsulation, native VLAN, and allowed VLANs |
| `write` | Saves running-config to startup-config (same as `copy run start`) |

## Default VLANs on Cisco Switches

| VLAN | Purpose |
|------|---------|
| VLAN 1 | Default — all ports assigned here by default |
| VLAN 1002–1005 | Reserved for FDDI and Token Ring — not used in CCNA |

## Note on `switchport mode access`

This command is **optional** when assigning a port to a VLAN because switch ports are already in access mode by default. However it is best practice to configure it explicitly for clarity.

## Tools Required

- Cisco Packet Tracer
