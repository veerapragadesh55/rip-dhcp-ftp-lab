# RIP Routing with DHCP & FTP Server Lab

A Cisco Packet Tracer network simulation demonstrating **RIP v2 (Routing Information Protocol)** dynamic routing with **DHCP server** and **FTP server** configuration across multiple subnets.

## 📋 Project Overview

This lab simulates a small enterprise network with:
- **3 Routers** configured with RIP v2 for dynamic routing
- **DHCP Server** (192.168.1.10) providing automatic IP assignment to client devices
- **FTP Server** (192.168.1.10) for file transfer services
- **Multiple subnets** connected via RIP-enabled routers
- **Network redundancy** with dynamic route updates

## 🏗️ Network Topology

```
                    R1 ══════════ R2 ══════════ R3
              192.168.1.1    10.0.0.1 & 11.0.1.1    12.0.1.1 & 192.168.3.1
                                  192.168.2.1

┌──────────────────────────────────────────────────────────────────────────┐
│                         NETWORK TOPOLOGY                                 │
├──────────────────────────────────────────────────────────────────────────┤
│                                                                          │
│  Router1 (HQ Router)                                                     │
│  ├─ FastEthernet0/0: 192.168.1.1/24 (LAN A)                            │
│  └─ FastEthernet0/1: 10.0.0.1/30 (Link to Router2)                     │
│     Password: Router1-123                                               │
│                                                                          │
│  Router2 (Core Router)                                                   │
│  ├─ FastEthernet0/0: 10.0.1.1/30 (Link to Router1)                     │
│  ├─ FastEthernet0/1: 11.0.1.1/30 (Link to Router3)                     │
│  └─ FastEthernet1/0: 192.168.2.1/24 (LAN B)                            │
│     Password: Router2-000                                               │
│                                                                          │
│  Router3 (Branch Router)                                                 │
│  ├─ FastEthernet0/0: 12.0.1.1/30 (Link to Router2)                     │
│  └─ FastEthernet0/1: 192.168.3.1/24 (LAN C)                            │
│     Password: Router3-555                                               │
│                                                                          │
│  ⚙️  SERVERS (on LAN A - 192.168.1.0/24)                                │
│  ├─ DHCP Server: 192.168.1.10                                          │
│  └─ FTP Server: 192.168.1.10  (User: User-123 | Pass: HR-000)         │
│                                                                          │
│  🖥️  CLIENT DEVICES: Configured as DHCP clients across all LANs        │
│                                                                          │
└──────────────────────────────────────────────────────────────────────────┘
```

## 🔧 Configuration Details

### Routing Protocol
- **Protocol**: RIPv2 (Routing Information Protocol Version 2)
- **Metric**: Hop count (max 15)
- **Update Interval**: 30 seconds
- **Convergence**: ~3 minutes on topology change

### DHCP Configuration
- **DHCP Server Location**: LAN A (192.168.1.100)
- **Pools**:
  - `POOL_A`: 192.168.1.50 - 192.168.1.150 (gateway 192.168.1.1)
  - `POOL_B`: 192.168.2.50 - 192.168.2.150 (gateway 192.168.2.1)
  - `POOL_C`: 192.168.3.50 - 192.168.3.150 (gateway 192.168.3.1)
- **Lease Time**: 86400 seconds (24 hours)

### FTP Server Configuration
- **FTP Server Location**: LAN A (192.168.1.10)
- **Username**: `User/HR`
- **Password**: `123/000`
- **Port**: 21 (default)
- **Service**: File Transfer Protocol for document/config backup

### Subnets
| Network | Router Interface | Purpose |
|---------|------------------|---------|
| 192.168.1.0/24 | Router1 Fa0/0 | **Headquarters LAN** (DHCP + FTP Server) |
| 192.168.2.0/24 | Router2 Fa1/0 | **Branch LAN** (Remote users) |
| 192.168.3.0/24 | Router3 Fa0/1 | **Regional LAN** (Branch office) |
| 10.0.0.0/30 | R1-R2 WAN Link | **Router1-Router2 Connection** (10.0.0.1 - 10.0.1.1) |
| 11.0.1.0/30 | R2-R3 WAN Link | **Router2-Router3 Connection** (11.0.1.1 - 12.0.1.1) |

## 🔐 Access Credentials

| Device | Type | IP Address | Username | Password |
|--------|------|-----------|----------|----------|
| Router1 | Router | 192.168.1.1 | (console) | Router1-123 |
| Router2 | Router | 192.168.2.1 | (console) | Router2-000 |
| Router3 | Router | 192.168.3.1 | (console) | Router3-555 |
| DHCP Server | Server | 192.168.1.10 | - | - |
| FTP Server | Server | 192.168.1.10 | **User-123** | **HR-000** |

## 📂 Files in This Repository

- `README.md` - Project overview (this file)
- `ROUTER_CONFIGURATION.md` - Detailed router CLI configs
- `rip-dhcp-ftp-lab.pkt` - Cisco Packet Tracer network simulation

## 🚀 How to Use

### Prerequisites
- **Cisco Packet Tracer** (v7.0 or later)
- Basic understanding of RIP routing and DHCP

### Steps

1. **Open the Lab**
   - Open `rip-dhcp-ftp-lab.pkt` in Packet Tracer

2. **Verify Configurations**
   - Refer to `ROUTER_CONFIGURATION.md` for exact CLI commands
   - Compare with running configs on each router

3. **Test DHCP & Connectivity**
   - From LAN A clients, ping devices on LAN B and LAN C
   - Verify DHCP clients receive IP addresses automatically
   - Use `show ip route` to view RIP routing tables

4. **Test FTP Server Access**
   - From any client, connect to FTP Server: `192.168.1.10`
   - Login with credentials: **User-123 / HR-000**
   - Test file upload/download across subnets
   - Verify RIP routes allow FTP access from remote LANs

5. **Test Failover** (Optional)
   - Disable a WAN link and observe route reconvergence
   - Verify FTP and DHCP still accessible via alternate paths
   - Check for alternate routes in routing table

## 📊 Verification Commands

### On Routers (CLI)

```bash
# View RIP configuration
show run | section rip
show ip protocols

# View routing table
show ip route
show ip route rip

# View DHCP pools and leases
show ip dhcp pool
show ip dhcp binding

# Verify interfaces
show ip interface brief
```

### In Simulation

- **Ping** between devices across routers to verify connectivity
- **Traceroute** to verify path taken through RIP network
- Check **Device → Config → IP Configuration** on clients for DHCP-assigned IPs
- **FTP Access**: 
  - Open client → Desktop → Web Browser (or FTP client)
  - Connect to `ftp://192.168.1.10` or `192.168.1.10`
  - Login: **User-123** / **HR-000**
  - Upload/download files to test FTP across all subnets

## 🎯 Learning Objectives

After completing this lab, you should understand:

- ✅ RIPv2 dynamic routing configuration & convergence
- ✅ Multi-router network topology & failover
- ✅ DHCP server setup, pools, and automatic IP assignment
- ✅ FTP server configuration and secure file transfer
- ✅ IP addressing & subnetting across multiple LANs
- ✅ Cross-subnet connectivity & routing verification
- ✅ Troubleshooting routing & service delivery issues
- ✅ Network packet flow and path selection through RIP

## ⚠️ Important Notes

- RIP is **distance-vector** protocol (deprecated in production; OSPF/BGP preferred)
- This lab is for **educational purposes** only
- Packet Tracer doesn't simulate all real-world network behaviors
- Convergence times are accelerated in simulation

## 🔗 Related Concepts

- Routing Information Protocol (RIP/RIPv2)
- Dynamic Routing Protocols & Convergence
- DHCP (Dynamic Host Configuration Protocol)
- FTP (File Transfer Protocol) & File Services
- Classless Inter-Domain Routing (CIDR) & Subnetting
- Network Failover & Route Redundancy
- Cisco IOS Command Line Interface (CLI)
- Multi-site Connectivity & WAN Design

## 📝 Notes for Your GitHub

- Add this README to explain the project purpose
- Include router configurations in a separate markdown file
- Consider adding a `SETUP_GUIDE.md` if you want step-by-step instructions
- Add a `.gitignore` to exclude unnecessary files

---

**Created**: August 2026  
**Tested on**: Cisco Packet Tracer 7.x  
**Author**: Veera - Networking Lab
