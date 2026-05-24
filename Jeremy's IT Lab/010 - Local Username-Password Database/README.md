# Cisco Local User Authentication & Case Sensitivity Lab

A short lab to practice creating local user accounts on a Cisco 1941 router and to understand how case sensitivity works for usernames and passwords.

## Network Topology

```
PC1 (PC-PT) ----Console Cable---- R1 (1941)
```

## Lab Tasks

1. Access R1 from PC1 via console (Desktop → Terminal)
2. Create two local users on R1:
   - Username: `ccna` / Password: `cisco`
   - Username: `ccnp` / Password: `CISCO`
3. Configure the console line to authenticate using the local user database
4. Logout and log back in with each account — test case sensitivity
5. Create a third user: Username: `CCNA` / Password: `router`
6. Login with the `CCNA` account — check what password is needed, verify with `show run`
7. Review: Are usernames case-sensitive? Are passwords case-sensitive?

## Key Commands

```bash
# Create local users
configure terminal
username ccna password cisco
username ccnp password CISCO

# Configure console to use local user database
line console 0
login local              # 'login local' instead of just 'login'

# Logout
end
logout

# Verify users in config
show running-config
```

## Key Findings — Case Sensitivity

| Item     | Case-Sensitive? | Example |
|----------|-----------------|---------|
| Username | ❌ No  | `ccna`, `CCNA`, `CcNa` all work |
| Password | ✅ Yes | `cisco` ≠ `CISCO` ≠ `Cisco` |

## Bonus Observation — Duplicate Username Behaviour

When a new user `CCNA` (uppercase) was created while `ccna` (lowercase) already existed:
- The router treated them as the **same user** (usernames are not case-sensitive)
- It **did not create a second account** — it updated the existing one
- The username stayed stored as `ccna` in the running config
- But the password was updated to `router`

> Always be careful when updating user passwords — if you type the username differently in case, the router will silently update the existing account instead of creating a new one.

## Tools Required

- Cisco Packet Tracer
