# NTP Authentication Lab

## Topology

| Router | Interface | IP address |
|---|---|---|
| R1 | G0/0 | 192.168.12.1/24 |
| R1 | G0/1 | 192.168.13.1/24 |
| R2 | G0/0 | 192.168.12.2/24 |
| R3 | G0/0 | 192.168.13.3/24 |

R1 is the NTP server. R2 and R3 synchronize their clocks with R1.

## R1 Configuration

Set the clock to the correct UTC time, then set the local time zone. The example below follows the lab's IST (UTC+5:30) setting.

```text
enable
clock set 08:00:00 22 Aug 2019
configure terminal
clock timezone IST 5 30

ntp master
ntp authenticate
ntp authentication-key 1 md5 cisco1
ntp authentication-key 2 md5 cisco2
ntp trusted-key 1
ntp trusted-key 2
end
```

## R2 Configuration

```text
enable
configure terminal
clock timezone IST 5 30
ntp authenticate
ntp authentication-key 1 md5 cisco1
ntp trusted-key 1
ntp server 192.168.12.1 key 1
end
```

## R3 Configuration

```text
enable
configure terminal
clock timezone IST 5 30
ntp authenticate
ntp authentication-key 2 md5 cisco2
ntp trusted-key 2
ntp server 192.168.13.1 key 2
end
```

## Verification

Run these commands on R2 and R3 after allowing time for synchronization:

```text
show clock detail
show ntp associations
```

The clock source should show NTP. In the NTP associations output, `*` indicates the selected synchronization peer and `~` indicates a configured association.

## Notes

- NTP authentication ensures that each client uses the configured trusted key.
- R2 uses key 1 (`cisco1`); R3 uses key 2 (`cisco2`).
- Configure the time zone on each router separately. NTP synchronizes the time, not the displayed time zone.
- The `ntp master` command uses the default stratum level of 8.
