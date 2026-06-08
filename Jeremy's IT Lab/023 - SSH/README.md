# CCNA Lab – SSH Configuration

A Packet Tracer lab focused on configuring SSH access to Cisco devices — a more secure alternative to Telnet, as all traffic is encrypted.

## Topology

```
PC1 (192.168.1.11/24) — F0/1 SW1 (VLAN1: 192.168.1.2/24) G0/1 — G0/0 R1 (192.168.1.1/24)
```

## Lab Steps

1. Configure hostnames on SW1 and R1 (required for SSH).
2. Configure IP addresses — VLAN1 interface on SW1 and G0/0 on R1.
3. Create a local user account (`cisco` / `CCNA`) on both devices.
4. Configure DNS domain name `cisco.com` on both devices (required for SSH).
5. Generate RSA keys with modulus size 1024 on both devices.
6. Configure VTY lines 0–15: local login, SSH only, 5-minute inactivity timeout.
7. Enable SSH version 2 on both devices.
8. SSH from PC1 to SW1 and R1 to verify access.

## Key Commands

```bash
# Hostname (required for SSH)
hostname SW1

# IP addresses
interface vlan 1          ! SW1
 ip address 192.168.1.2 255.255.255.0
 no shutdown

interface g0/0            ! R1
 ip address 192.168.1.1 255.255.255.0
 no shutdown

# Local user
username cisco password CCNA

# DNS domain name (required for SSH)
ip domain-name cisco.com

# Generate RSA keys
crypto key generate rsa
 ! enter modulus: 1024

# VTY lines
line vty 0 15
 login local
 transport input ssh
 exec-timeout 5

# Enable SSH v2
ip ssh version 2

# Connect from PC1
ssh -l cisco 192.168.1.2   ! to SW1
ssh -l cisco 192.168.1.1   ! to R1

# Verification
show ip ssh
show ssh
show run | section vty
```

## Key Concepts

- SSH requires **hostname + domain name + RSA keys** before it can be enabled
- **`transport input ssh`** blocks Telnet — only SSH connections are accepted on VTY lines
- **`exec-timeout 5`** disconnects idle sessions after 5 minutes — important security practice
- Always use **SSH version 2** — it fixes known weaknesses in version 1
- SSH encrypts all traffic; Telnet sends everything in plaintext
