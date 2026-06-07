# CCNA Lab – Telnet Configuration

A Packet Tracer lab focused on configuring Telnet access to Cisco devices using VTY lines and local user authentication.

## Topology

```
PC1 (192.168.1.11/24) — F0/1 SW1 (VLAN1: 192.168.1.2/24) G0/1 — G0/0 R1 (192.168.1.1/24)
```

## Lab Steps

1. Configure IP addresses — VLAN1 interface on SW1 and G0/0 on R1.
2. Create a local user account (`cisco` / `CCNA`) on both SW1 and R1.
3. Configure VTY lines 0–15 on both devices to use local login and allow only Telnet.
4. Telnet to SW1 and R1 from PC1 to verify access.

## Key Commands

```bash
# SW1 – Management IP on VLAN1
interface vlan 1
 ip address 192.168.1.2 255.255.255.0
 no shutdown

# R1 – Interface IP
interface g0/0
 ip address 192.168.1.1 255.255.255.0
 no shutdown

# Local user (both devices)
username cisco password CCNA

# VTY lines (both devices)
line vty 0 15
 login local
 transport input telnet

# Telnet from PC1
telnet 192.168.1.2   ! connects to SW1
telnet 192.168.1.1   ! connects to R1

# Verification
show run | section vty
show users
```

## Key Concepts

- Switches use a **VLAN interface** (SVI) for management IP — physical ports cannot have IPs
- **VTY lines** (0–15) handle remote connections; `login local` enforces local username/password auth
- **`transport input telnet`** restricts the line to Telnet only; use `ssh` or `all` for other options
- Telnet sends all data in **plaintext** — use SSH in production environments
- Router interfaces are **administratively down** by default; always add `no shutdown`
