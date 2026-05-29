# Cisco System Configuration Dialog Lab

A lab to practice using the **System Configuration Dialog (Setup Wizard)** to perform basic configuration on a Cisco router and switch, then verify with a ping.

## Network Topology

```
SW1 --G0/1----G0/0-- R1
VLAN1: 192.168.1.2/24    192.168.1.1/24
```

## Lab Tasks

1. Use the System Configuration Dialog to configure R1 and SW1 (save to NVRAM)
2. Ping from SW1 to R1 to verify connectivity

## Configuration Values

| Setting | R1 | SW1 |
|---------|-----|-----|
| Hostname | R1 | SW1 |
| Enable secret | Cisco | Cisco |
| Enable password | CCNA | CCNA |
| VTY password | CCENT | CCENT |
| SNMP | No | No |
| VLAN1 interface | No | 192.168.1.2 / 255.255.255.0 |
| GigabitEthernet0/0 | 192.168.1.1 / 255.255.255.0 | — |
| Cluster command switch | — | No |

## How to Enter the Setup Dialog

```bash
# If first time on CLI, it prompts automatically.
# Otherwise, trigger it manually:
enable
setup
# Answer 'yes' to enter configuration dialog
# Choose 'no' for basic management setup (use extended setup)
```

## Dialog Flow

```
Would you like to enter the initial configuration dialog? → yes
Would you like to enter basic management setup? → no
Would you like to see the current interface summary? → Enter (yes)
Hostname: → R1 / SW1
Enable secret: → Cisco
Enable password: → CCNA
Virtual terminal password: → CCENT
SNMP network management: → Enter (no)
Configure interface?: → yes (only for Gi0/0 on R1, VLAN1 on SW1)
[0] Cancel  [1] Redo  [2] Save → Enter (save)
```

## Key Notes

- **Physical switch ports cannot have IP addresses** — only a VLAN interface (like VLAN1) can, used for management access.
- **VTY (Virtual Teletype)** — the virtual terminal lines used for remote access (Telnet/SSH). Password set here protects that access.
- The enable secret is **automatically encrypted**; the enable password is **not**.
- The setup dialog saves directly to **NVRAM (startup-config)** when you choose option 2.
- Verify saved config with `show startup-config`.

## Tools Required

- Cisco Packet Tracer
