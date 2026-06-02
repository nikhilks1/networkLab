# CCNA Lab – Port Security (Manual MAC Address Configuration)

A Packet Tracer lab demonstrating how to manually configure secure MAC addresses on switch ports and observe port security violations.

## Topology

```
PC1 (192.168.1.11/24) — SW1 (F0/2) — (F0/1) SW1 — SW2 (F0/1) — (F0/2) SW2 — PC2 (192.168.1.12/24)
```

## Lab Steps

1. Ping from PC1 to PC2 to generate traffic.
2. View SW1's MAC address table and note PC1's MAC address.
3. Enable port security on SW1's F0/2 and manually set PC1's MAC as a secure address.
4. Repeat on SW2's F0/2 for PC2's MAC address.
5. Ping PC1 → PC2 to verify connectivity.
6. Swap cables: connect PC1 to SW2's F0/2 and PC2 to SW1's F0/2.
7. Ping PC1 → PC2 — a port security violation triggers on SW2's F0/2, shutting it down (err-disabled).
8. Reconnect cables to the original topology.
9. Ping PC1 → PC2 — fails because SW2's F0/2 is still err-disabled.
10. Manually recover the interface with `shutdown` → `no shutdown`, then ping to confirm recovery.

## Key Commands

```bash
# Enable port security with a manual MAC address
interface f0/2
 switchport mode access
 switchport port-security
 switchport port-security mac-address <MAC>

# Recover an err-disabled interface
interface f0/2
 shutdown
 no shutdown

# Verify
show mac address-table
show interfaces f0/2
```

## Key Concepts

- **Default max secure MACs:** 1 per port
- **Default violation action:** `shutdown` (err-disabled state)
- **err-disabled recovery:** must be done manually with `shutdown` / `no shutdown` unless auto-recovery is configured
