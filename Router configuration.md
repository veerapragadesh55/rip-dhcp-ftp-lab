# RIP v2 Routing Configuration Guide

Complete CLI configuration for RIP v2 dynamic routing in Cisco Packet Tracer across 3-router network topology.

---

## Router 1 (R1) - Headquarters Router

### Overview
- **Role**: Primary HQ Router
- **Interfaces**: Fa0/0 (LAN A), Fa0/1 (WAN to R2)
- **Routing**: RIP v2 on 2 networks

### Interface Configuration

```cisco
configure terminal

! Configure LAN Interface (Fa0/0)
interface FastEthernet0/0
 ip address 192.168.1.1 255.255.255.0
 no shutdown

! Configure WAN Interface to R2 (Fa0/1)
interface FastEthernet0/1
 ip address 10.0.0.1 255.255.255.252
 no shutdown

exit
```

### RIP v2 Configuration

```cisco
configure terminal

router rip
 version 2
 network 192.168.1.0
 network 10.0.0.0
 no auto-summary

exit
```

### Summary

```
Router: R1 (Headquarters)
├─ Fa0/0: 192.168.1.1/24 (LAN A)
├─ Fa0/1: 10.0.0.1/30 (to R2)
└─ RIP v2: Networks 192.168.1.0, 10.0.0.0
```

---

## Router 2 (R2) - Core Router

### Overview
- **Role**: Central Core Router
- **Interfaces**: Fa0/0 (to R1), Fa1/0 (LAN B), Fa0/2 (to R3)
- **Routing**: RIP v2 on 3 networks (main redistribution point)

### Interface Configuration

```cisco
configure terminal

! Configure WAN Interface to R1 (Fa0/0)
interface FastEthernet0/0
 ip address 10.0.1.1 255.255.255.252
 no shutdown

! Configure LAN Interface (Fa1/0)
interface FastEthernet1/0
 ip address 192.168.2.1 255.255.255.0
 no shutdown

! Configure WAN Interface to R3 (Fa0/2)
interface FastEthernet0/2
 ip address 11.0.1.1 255.255.255.252
 no shutdown

exit
```

### RIP v2 Configuration

```cisco
configure terminal

router rip
 version 2
 network 10.0.1.0
 network 192.168.2.0
 network 11.0.1.0
 no auto-summary

exit
```

### Summary

```
Router: R2 (Core Router)
├─ Fa0/0: 10.0.1.1/30 (to R1)
├─ Fa1/0: 192.168.2.1/24 (LAN B)
├─ Fa0/2: 11.0.1.1/30 (to R3)
└─ RIP v2: Networks 10.0.1.0, 192.168.2.0, 11.0.1.0
```

---

## Router 3 (R3) - Branch Router

### Overview
- **Role**: Branch/Remote Router
- **Interfaces**: Fa0/0 (to R2), Fa0/1 (LAN C)
- **Routing**: RIP v2 on 2 networks

### Interface Configuration

```cisco
configure terminal

! Configure WAN Interface to R2 (Fa0/0)
interface FastEthernet0/0
 ip address 12.0.1.1 255.255.255.252
 no shutdown

! Configure LAN Interface (Fa0/1)
interface FastEthernet0/1
 ip address 192.168.3.1 255.255.255.0
 no shutdown

exit
```

### RIP v2 Configuration

```cisco
configure terminal

router rip
 version 2
 network 12.0.1.0
 network 192.168.3.0
 no auto-summary

exit
```

### Summary

```
Router: R3 (Branch Router)
├─ Fa0/0: 12.0.1.1/30 (to R2)
├─ Fa0/1: 192.168.3.1/24 (LAN C)
└─ RIP v2: Networks 12.0.1.0, 192.168.3.0
```

---

## Complete Configuration Scripts (Copy-Paste Ready)

### Router1 - Full Configuration

```cisco
enable
configure terminal

! Hostname & Enable Password
hostname Router1
enable password Router1-123

! Interface Fa0/0 (LAN A)
interface FastEthernet0/0
 ip address 192.168.1.1 255.255.255.0
 no shutdown

! Interface Fa0/1 (WAN to R2)
interface FastEthernet0/1
 ip address 10.0.0.1 255.255.255.252
 no shutdown

! RIP v2 Configuration
router rip
 version 2
 network 192.168.1.0
 network 10.0.0.0
 no auto-summary

end
write memory
```

### Router2 - Full Configuration

```cisco
enable
configure terminal

! Hostname & Enable Password
hostname Router2
enable password Router2-000

! Interface Fa0/0 (WAN to R1)
interface FastEthernet0/0
 ip address 10.0.1.1 255.255.255.252
 no shutdown

! Interface Fa1/0 (LAN B)
interface FastEthernet1/0
 ip address 192.168.2.1 255.255.255.0
 no shutdown

! Interface Fa0/2 (WAN to R3)
interface FastEthernet0/2
 ip address 11.0.1.1 255.255.255.252
 no shutdown

! RIP v2 Configuration
router rip
 version 2
 network 10.0.1.0
 network 192.168.2.0
 network 11.0.1.0
 no auto-summary

end
write memory
```

### Router3 - Full Configuration

```cisco
enable
configure terminal

! Hostname & Enable Password
hostname Router3
enable password Router3-555

! Interface Fa0/0 (WAN to R2)
interface FastEthernet0/0
 ip address 12.0.1.1 255.255.255.252
 no shutdown

! Interface Fa0/1 (LAN C)
interface FastEthernet0/1
 ip address 192.168.3.1 255.255.255.0
 no shutdown

! RIP v2 Configuration
router rip
 version 2
 network 12.0.1.0
 network 192.168.3.0
 no auto-summary

end
write memory
```

---

## Verification Commands

Run these on each router to verify RIP configuration and routing:

### On All Routers

```cisco
! Check interfaces and IP addresses
show ip interface brief

! Verify RIP is running
show ip protocols

! View RIP configuration
show run | section router rip

! Check RIP routing table
show ip route
show ip route rip

! Detailed RIP database
show ip rip database

! View active RIP neighbors
debug ip rip

! Check RIP version
show ip protocols | include RIP
```

### Verify Connectivity Between Routers

```cisco
! Ping Router2 LAN from Router1
ping 192.168.2.1

! Ping Router3 LAN from Router1
ping 192.168.3.1

! Check if RIP routes appear
show ip route rip
```

---

## Troubleshooting RIP Issues

### Issue: RIP Routes Not Appearing

**Symptoms**: `show ip route` doesn't show routes from other networks

**Solution**:
```cisco
! 1. Verify RIP is running
show ip protocols

! 2. Check RIP configuration
show run | section router rip

! 3. Verify interfaces are UP
show ip interface brief

! 4. Check network statements
show run | section router rip
! Make sure all networks are listed

! 5. Enable RIP debug
debug ip rip
! Watch for RIP packets being sent/received
```

### Issue: Cannot Ping Between Subnets

**Symptoms**: Ping fails to remote LANs

**Solution**:
```cisco
! 1. Check routing table
show ip route

! 2. Verify RIP learned the routes
show ip route rip

! 3. Test connectivity to gateway
ping 192.168.2.1  (if pinging from R1 to R2 LAN)

! 4. Check interface status
show ip interface fa0/0
show ip interface fa0/1

! 5. Verify no access lists blocking
show access-lists
show ip access-lists
```

### Issue: Routes Not Converging

**Symptoms**: Routing table takes too long to update after change

**Potential causes**:
- RIP timers still running (wait 180+ seconds)
- Interface down not properly detected
- RIP not enabled on all interfaces

**Solution**:
```cisco
! Check RIP timers
show ip protocols

! Manually trigger update
clear ip route *

! Restart RIP
router rip
 no network 192.168.1.0
 network 192.168.1.0
 
! Clear all routes and let RIP rebuild
clear ip route *
```

---

## RIP Configuration Parameters Reference

| Parameter | Value | Purpose |
|-----------|-------|---------|
| Protocol | RIPv2 | Classless routing (supports CIDR) |
| Metric | Hop count | Max 15 hops |
| Update Interval | 30 seconds | RIP sends updates every 30s |
| Invalid Timer | 180 seconds | Route marked invalid after no updates |
| Hold-down Timer | 180 seconds | Wait before accepting alternate route |
| Flush Timer | 240 seconds | Route removed from table |
| Auto-summary | Disabled | Use classless routing boundaries |

---

## Network Design Summary

### IP Addressing Scheme

| Device | Interface | IP Address | Subnet Mask | Network |
|--------|-----------|-----------|-------------|---------|
| R1 | Fa0/0 | 192.168.1.1 | 255.255.255.0 | 192.168.1.0/24 (LAN A) |
| R1 | Fa0/1 | 10.0.0.1 | 255.255.255.252 | 10.0.0.0/30 (WAN) |
| R2 | Fa0/0 | 10.0.1.1 | 255.255.255.252 | 10.0.1.0/30 (WAN) |
| R2 | Fa1/0 | 192.168.2.1 | 255.255.255.0 | 192.168.2.0/24 (LAN B) |
| R2 | Fa0/2 | 11.0.1.1 | 255.255.255.252 | 11.0.1.0/30 (WAN) |
| R3 | Fa0/0 | 12.0.1.1 | 255.255.255.252 | 12.0.1.0/30 (WAN) |
| R3 | Fa0/1 | 192.168.3.1 | 255.255.255.0 | 192.168.3.0/24 (LAN C) |

### RIP Networks Advertised

| Router | Networks Advertised | Purpose |
|--------|-------------------|---------|
| R1 | 192.168.1.0, 10.0.0.0 | HQ LAN + WAN link to R2 |
| R2 | 10.0.1.0, 192.168.2.0, 11.0.1.0 | Links to R1, R3 + Branch LAN |
| R3 | 12.0.1.0, 192.168.3.0 | WAN link to R2 + Regional LAN |

---

## Expected Routing Table (show ip route output)

### On Router1

```
R    192.168.2.0/24 [120/1] via 10.0.0.2, 00:00:15, FastEthernet0/1
R    192.168.3.0/24 [120/2] via 10.0.0.2, 00:00:15, FastEthernet0/1
R    10.0.1.0/30 [120/1] via 10.0.0.2, 00:00:15, FastEthernet0/1
R    11.0.1.0/30 [120/2] via 10.0.0.2, 00:00:15, FastEthernet0/1
R    12.0.1.0/30 [120/3] via 10.0.0.2, 00:00:15, FastEthernet0/1
```

### On Router2

```
R    192.168.1.0/24 [120/1] via 10.0.1.1, 00:00:15, FastEthernet0/0
R    192.168.3.0/24 [120/1] via 11.0.1.2, 00:00:15, FastEthernet0/2
R    10.0.0.0/30 [120/1] via 10.0.1.1, 00:00:15, FastEthernet0/0
R    12.0.1.0/30 [120/1] via 11.0.1.2, 00:00:15, FastEthernet0/2
```

### On Router3

```
R    192.168.1.0/24 [120/2] via 11.0.1.1, 00:00:15, FastEthernet0/0
R    192.168.2.0/24 [120/1] via 11.0.1.1, 00:00:15, FastEthernet0/0
R    10.0.0.0/30 [120/2] via 11.0.1.1, 00:00:15, FastEthernet0/0
R    10.0.1.0/30 [120/1] via 11.0.1.1, 00:00:15, FastEthernet0/0
```

---

## Important Notes

- ✅ **RIPv2** supports classless routing (CIDR notation)
- ✅ **`no auto-summary`** prevents automatic summarization at class boundaries
- ✅ **Hop count metric** means RIP prefers paths with fewer router hops
- ⚠️ **RIP is slow** - max 15 hops, 30-second update interval
- ⚠️ **RIPv1 is obsolete** - always use RIPv2
- 💡 **For production**, use OSPF or BGP instead (RIP is educational only)

---

**Last Updated**: August 2026  
**Tested on**: Cisco Packet Tracer 7.x+  
**Configuration Type**: RIP v2 Dynamic Routing
