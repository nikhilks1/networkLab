# Cisco LLDP Basic Configuration Lab

A simple hands-on lab to practice LLDP (Link Layer Discovery Protocol) using two switches, two routers and a PC in Packet Tracer.

## Network Topology

```
SW1 ---- R1 ---- R2 ---- SW2 ---- PC1
```

## Lab Tasks

1. Disable CDP and enable LLDP on each networking device
2. Use a show command to find the default timer values for LLDP
3. Use LLDP to identify which interfaces are used to connect the routers and switches
4. Use LLDP to identify the IOS version of neighboring devices
5. Prevent SW2's F0/1 interface from sending or receiving LLDP updates

## Key Commands

```
# Disable CDP and enable LLDP (on every device)
no cdp run
lldp run

# View LLDP status and default timers
show lldp

# View connected neighbors and interfaces
show lldp neighbors

# View neighbor details including IOS version
show lldp neighbors detail

# Stop LLDP on an interface (SW2 F0/1)
interface f0/1
 no lldp receive
 no lldp transmit
```

## Notes

* Default timers: advertisement 30 seconds, hold time 120 seconds, reinitialization delay 2 seconds
* CDP is Cisco proprietary, LLDP is an open standard and works with other vendors
* CDP uses one command (`cdp enable`) for both directions, LLDP uses two separate commands (`lldp receive` and `lldp transmit`)
* In Packet Tracer, turn off 'Show Device Model Labels' and 'Always Show Port Labels in Logical Workspace' under Options > Preferences

## Tools Required

* Cisco Packet Tracer
