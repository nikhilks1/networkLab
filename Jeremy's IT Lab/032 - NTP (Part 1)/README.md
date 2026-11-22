# Cisco NTP Basic Configuration Lab

A simple hands-on lab to practice system clock and NTP configuration using three Cisco routers in Packet Tracer.

## Network Topology

```
R1 ----G0/0 (.1)---- 192.168.12.0/24 ----(.2) G0/0---- R2 ----G0/1 (.2)---- 192.168.23.0/24 ----(.3) G0/0---- R3
```

| Link           | Router Interfaces                    |
|----------------|--------------------------------------|
| 192.168.12.0/24 | R1 G0/0 (.1) - R2 G0/0 (.2)         |
| 192.168.23.0/24 | R2 G0/1 (.2) - R3 G0/0 (.3)         |

## Lab Tasks

1. Configure the timezone, time, and date on R1 to match your local time
2. Configure R1 as an NTP server with the default stratum level
3. Configure R2 to synchronize its time to R1
4. Configure R3 to synchronize its time to R2

## Key Commands

R1 (clock and NTP server)

```
# Set the timezone (example: JST, 9 hours ahead of UTC)
clock timezone JST 9

# Set the time (in privileged exec mode, entered in UTC)
clock set 12:00:00 21 August 2019

# Make R1 an NTP server with the default stratum
ntp master
```

R2 (syncs to R1)

```
clock timezone JST 9
ntp server 192.168.12.1
```

R3 (syncs to R2)

```
clock timezone JST 9
ntp server 192.168.23.2
```

Verify

```
show clock detail
show ntp associations
show ntp status
```

## Notes

* `clock set` uses UTC. The timezone only changes how the time is displayed
* NTP does not sync the timezone, so set it on each router
* NTP can take a few minutes to synchronize. The time source in `show clock detail` changes from "hardware calendar" to "NTP" when it is done

## Tools Required

* Cisco Packet Tracer
