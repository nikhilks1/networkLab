# Cisco Port Security — Sticky MAC Address Lab

A lab to understand the difference between dynamically learned secure MAC addresses and sticky secure MAC addresses, and how each behaves after a switch reload.

## Network Topology

```
PC1 (192.168.1.11/24) --F0/2-- SW1 --F0/1----F0/1-- SW2 --F0/2-- PC2 (192.168.1.12/24)
```

## Lab Tasks

1. Enable port security on F0/2 of each switch
2. Ping PC1 → PC2 to generate traffic
3. View secure MAC addresses on SW1 — PC1's MAC should appear
4. Check SW1 F0/2 in running-config — note what is saved
5. Save running-config and reload SW1
6. View secure MAC addresses again — is PC1's MAC still there?
7. Enable sticky MAC on SW1 F0/2, then ping again to generate traffic
8. View secure MACs and running-config — what is different now?
9. Save running-config and reload SW1
10. View secure MACs again — is PC1's MAC still there this time?

## Key Commands

```bash
# Enable port security
configure terminal
interface fastethernet 0/2
switchport mode access
switchport port-security

# Enable sticky MAC address learning
switchport port-security mac-address sticky

# View secure MAC address table
show port-security address

# View running configuration
show running-config

# Save and reload
write
reload
```

## Sticky vs Dynamic Secure MAC — Key Difference

| Behaviour | Dynamic Secure MAC | Sticky Secure MAC |
|-----------|-------------------|-------------------|
| Learned automatically | ✅ | ✅ |
| Saved to running-config | ❌ | ✅ |
| Survives reload (if config saved) | ❌ | ✅ |
| Appears in `show port-security address` | ✅ (until reload) | ✅ (persistent) |

## What the Running-Config Shows

**Without sticky** — only manually entered commands appear:
```
switchport mode access
switchport port-security
```

**With sticky** — learned MAC is automatically added:
```
switchport mode access
switchport port-security
switchport port-security mac-address sticky
switchport port-security mac-address sticky 00XX.XXXX.XXXX
```

## Key Lesson

> Dynamic secure MACs are lost on reload even if you save the config, because they are never written to it. Sticky MACs are written to the running-config automatically and will survive a reload **as long as you save the running-config** before reloading.

## Tools Required

- Cisco Packet Tracer
