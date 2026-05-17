# Cisco Router Console Connection & Password Security Lab

A beginner-friendly lab to practice connecting to a Cisco 1941 router via console and configuring password security in Packet Tracer.

## Network Topology

```
PC1 (PC-PT) ----RS-232 / Console Cable---- R1 (1941)
```

## Lab Tasks

1. Connect PC1's RS-232 port to R1's console port using a console cable
2. Access R1 via PC1 → Desktop → Terminal, and set the hostname to `R1`
3. Set the enable secret to `cisco`
4. Set the console password to `ccna` and require it on login
5. Enable password encryption, verify via running config, then save

## Key Commands

```bash
# Enter privileged & global config mode
enable
configure terminal

# Set hostname
hostname R1

# Set enable secret (encrypted by default)
enable secret cisco

# Configure console password
line console 0
password ccna
login                         # required to enforce the password

# Enable password encryption
service password-encryption

# Verify configuration
show running-config

# Save configuration
copy running-config startup-config
```

## Notes

- **Enable secret** vs **Enable password** — always prefer `enable secret`; it is MD5-encrypted by default.
- `login` under `line console 0` is mandatory — without it the console password is ignored.
- `service password-encryption` encrypts plaintext passwords (like the console password) in the running config.
- After disabling `service password-encryption`, already-encrypted passwords **remain encrypted**; only new plaintext passwords stay unencrypted.

## Tools Required

- Cisco Packet Tracer
