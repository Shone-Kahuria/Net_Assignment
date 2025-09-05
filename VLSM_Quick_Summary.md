# VLSM Assignment - Quick Configuration Reference

## Network Topology Summary
**Base Network**: 172.31.0.0/16

| LAN | Hosts Needed | Subnet Address | Prefix | Subnet Mask | Usable Range | Gateway |
|-----|--------------|----------------|--------|-------------|--------------|---------|
| LAN 3 | 250 | 172.31.0.0/24 | /24 | 255.255.255.0 | 172.31.0.1 - 172.31.0.254 | 172.31.0.1 |
| LAN 4 | 100 | 172.31.1.0/25 | /25 | 255.255.255.128 | 172.31.1.1 - 172.31.1.126 | 172.31.1.1 |
| LAN 1 | 60 | 172.31.1.128/26 | /26 | 255.255.255.192 | 172.31.1.129 - 172.31.1.190 | 172.31.1.129 |
| LAN 2 | 10 | 172.31.1.192/28 | /28 | 255.255.255.240 | 172.31.1.193 - 172.31.1.206 | 172.31.1.193 |

## Part (a) - Fixed Subnetting Results (First 5 Subnets)

| Subnet | Network | Mask | Usable Range | Broadcast | Assignment |
|--------|---------|------|-------------|-----------|------------|
| 1 | 172.31.0.0/24 | 255.255.255.0 | 172.31.0.1 - 172.31.0.254 | 172.31.0.255 | LAN 3 |
| 2 | 172.31.1.0/24 | 255.255.255.0 | 172.31.1.1 - 172.31.1.254 | 172.31.1.255 | LAN 4 |
| 3 | 172.31.2.0/24 | 255.255.255.0 | 172.31.2.1 - 172.31.2.254 | 172.31.2.255 | LAN 1 |
| 4 | 172.31.3.0/24 | 255.255.255.0 | 172.31.3.1 - 172.31.3.254 | 172.31.3.255 | LAN 2 |
| 5 | 172.31.4.0/24 | 255.255.255.0 | 172.31.4.1 - 172.31.4.254 | 172.31.4.255 | Future |

## Part (b) - VLSM Results (First 5 Subnets)

| Subnet | Network | Prefix | Mask | Usable Range | Broadcast | Assignment |
|--------|---------|--------|------|-------------|-----------|------------|
| 1 | 172.31.0.0/24 | /24 | 255.255.255.0 | 172.31.0.1 - 172.31.0.254 | 172.31.0.255 | LAN 3 |
| 2 | 172.31.1.0/25 | /25 | 255.255.255.128 | 172.31.1.1 - 172.31.1.126 | 172.31.1.127 | LAN 4 |
| 3 | 172.31.1.128/26 | /26 | 255.255.255.192 | 172.31.1.129 - 172.31.1.190 | 172.31.1.191 | LAN 1 |
| 4 | 172.31.1.192/28 | /28 | 255.255.255.240 | 172.31.1.193 - 172.31.1.206 | 172.31.1.207 | LAN 2 |
| 5 | 172.31.1.208/28 | /28 | 255.255.255.240 | 172.31.1.209 - 172.31.1.222 | 172.31.1.223 | Future |

## Part (d) - Packet Tracer Device Configuration

### Router Configuration
```
interface GigabitEthernet0/0
ip address 172.31.1.129 255.255.255.192
description LAN1-60Hosts
no shutdown

interface GigabitEthernet0/1  
ip address 172.31.1.193 255.255.255.240
description LAN2-10Hosts
no shutdown
```

### LAN 1 Device IPs (172.31.1.128/26)
- **Router Gateway**: 172.31.1.129
- **PC1**: 172.31.1.131 / 255.255.255.192 / GW: 172.31.1.129
- **PC2**: 172.31.1.132 / 255.255.255.192 / GW: 172.31.1.129

### LAN 2 Device IPs (172.31.1.192/28)
- **Router Gateway**: 172.31.1.193  
- **PC1**: 172.31.1.195 / 255.255.255.240 / GW: 172.31.1.193
- **PC2**: 172.31.1.196 / 255.255.255.240 / GW: 172.31.1.193

## Efficiency Comparison

| Method | Total Allocated | Actually Needed | Wasted | Efficiency |
|--------|----------------|-----------------|---------|------------|
| **Fixed (/24)** | 1,016 addresses | 420 addresses | 596 | 41.3% |
| **VLSM** | 456 addresses | 420 addresses | 36 | 92.1% |
| **Improvement** | 55.1% reduction | Same | 93.9% reduction | 50.8% better |

## Team Information Template
```
Team Member 1: [Name] - [Admission Number]
Team Member 2: [Name] - [Admission Number]  
Date: September 5, 2025
Assignment: Advanced Subnetting VLSM Exercise
```
