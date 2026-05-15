# Cisco Router Basic Configuration Lab

A simple hands-on lab to practice basic router configuration using two Cisco 1941 routers in Packet Tracer.

## Network Topology

```
R1 (1941) ----GigabitEthernet0/0---- R2 (1941)
```

## Lab Tasks

1. Connect R1 and R2 via their GigabitEthernet0/0 interfaces
2. Set hostnames to `R1` and `R2` as shown in the diagram
3. Set the enable password to `cisco` on each router
4. View the running configuration — check if the password is encrypted
5. Enable password encryption on each router
6. View the running configuration again — check if the password is now encrypted
7. Disable password encryption on each router
8. View the running configuration once more — check the encryption status

## Key Commands

```bash
# Set hostname
hostname R1

# Set enable password
enable password cisco

# Enable password encryption
service password-encryption

# Disable password encryption
no service password-encryption

# View running config
show running-config
```

## Tools Required

- Cisco Packet Tracer
