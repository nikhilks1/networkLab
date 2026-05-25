# Cisco Local User Secrets & Login Banners Lab

A short lab to practice creating encrypted local user accounts (using `secret` instead of `password`) and configuring MOTD and login banners on a Cisco 1941 router.

## Network Topology

```
PC1 (PC-PT) ----Console Cable---- R1 (1941)
```

## Lab Tasks

1. Connect to R1 from PC1 via console (Desktop → Terminal)
2. Create two local users using `secret` (encrypted):
   - Username: `ccna` / Secret: `Cisco`
   - Username: `ccnp` / Secret: `Cisco`
3. Set the console port to authenticate using the local user database
4. Set a **Message of the Day (MOTD)** banner: `Welcome to Packet Tracer`
5. Set a **Login** banner: `Authorized users only!`
6. Logout and log back in — verify both banners display correctly

## Key Commands

```bash
# Create local users with encrypted secrets
configure terminal
username ccna secret Cisco
username ccnp secret Cisco

# Verify secrets are encrypted
do show running-config

# Set console to use local database
line console 0
login local

# Set MOTD banner (displays first, before login prompt)
exit
banner motd *Welcome to Packet Tracer*

# Set Login banner (displays after MOTD, before login prompt)
banner login *Authorized users only!*

# Logout to test banners
logout
```

## `password` vs `secret`

| Command | Encrypted by default? |
|---------|-----------------------|
| `username X password Y` | ❌ Plaintext in running-config |
| `username X secret Y`   | ✅ MD5 encrypted automatically |

Always prefer `secret` over `password` for both user accounts and enable access.

## Banner Display Order

When a user connects to the console, banners appear in this order:

```
1. MOTD banner     → "Welcome to Packet Tracer"
2. Login banner    → "Authorized users only!"
3. Username/Password prompt
```

## Note on Delimiter Character

The `*` (asterisk) used in `banner motd *message*` is the **delimiter** — it marks the start and end of the banner text. You can use any character as the delimiter, but it must not appear inside the message itself.

## Tools Required

- Cisco Packet Tracer
