# VLSM Practical Worksheet - Solutions & Analysis

## Problem Set 1: Basic VLSM Implementation

### Scenario A: Corporate Network Design

**Given Information:**
- Network Address: 192.168.100.0/24
- Requirements:
  - Sales Department: 60 hosts
  - Engineering Department: 30 hosts  
  - Marketing Department: 14 hosts
  - Management: 6 hosts
  - 3 WAN links (point-to-point)

**Solution Process:**

#### Step 1: Analyze Requirements
Sort departments by host count (largest to smallest):
1. Sales: 60 hosts → Need 64 addresses → /26 (255.255.255.192)
2. Engineering: 30 hosts → Need 32 addresses → /27 (255.255.255.224)
3. Marketing: 14 hosts → Need 16 addresses → /28 (255.255.255.240)
4. Management: 6 hosts → Need 8 addresses → /29 (255.255.255.248)
5. WAN Links: 2 hosts each → Need 4 addresses each → /30 (255.255.255.252)

#### Step 2: Subnet Allocation

| Department | Network Address | Subnet Mask | Host Range | Broadcast | Available/Needed |
|------------|----------------|-------------|------------|-----------|------------------|
| Sales | 192.168.100.0/26 | 255.255.255.192 | 192.168.100.1 - 192.168.100.62 | 192.168.100.63 | 62/60 |
| Engineering | 192.168.100.64/27 | 255.255.255.224 | 192.168.100.65 - 192.168.100.94 | 192.168.100.95 | 30/30 |
| Marketing | 192.168.100.96/28 | 255.255.255.240 | 192.168.100.97 - 192.168.100.110 | 192.168.100.111 | 14/14 |
| Management | 192.168.100.112/29 | 255.255.255.248 | 192.168.100.113 - 192.168.100.118 | 192.168.100.119 | 6/6 |
| WAN Link 1 | 192.168.100.120/30 | 255.255.255.252 | 192.168.100.121 - 192.168.100.122 | 192.168.100.123 | 2/2 |
| WAN Link 2 | 192.168.100.124/30 | 255.255.255.252 | 192.168.100.125 - 192.168.100.126 | 192.168.100.127 | 2/2 |
| WAN Link 3 | 192.168.100.128/30 | 255.255.255.252 | 192.168.100.129 - 192.168.100.130 | 192.168.100.131 | 2/2 |

#### Step 3: Binary Analysis (Sales Department Example)

```
Original Network: 192.168.100.0/24
Binary: 11000000.10101000.01100100.00000000

Sales Subnet: 192.168.100.0/26
Binary: 11000000.10101000.01100100.00000000
Mask:   11111111.11111111.11111111.11000000 (/26)

Network portion: 192.168.100.0
Host portion: 0-63 (64 addresses total, 62 usable)
```

#### Step 4: Verification
- Total original addresses: 256
- Total allocated addresses: 132
- Remaining addresses: 124
- Address space efficiency: 51.6%

---

## Problem Set 2: Advanced VLSM Scenario

### Scenario B: Multi-Location Enterprise

**Given Information:**
- Network Address: 10.50.0.0/16
- Locations and Requirements:

#### Main Campus:
- Faculty Network: 800 hosts
- Student Network: 1200 hosts  
- Administration: 150 hosts
- IT Department: 80 hosts
- Guest Network: 300 hosts

#### Branch Campus:
- Faculty Network: 200 hosts
- Student Network: 400 hosts
- Administration: 50 hosts

#### WAN Infrastructure:
- 5 point-to-point links between locations
- 2 backup links

**Solution Process:**

#### Step 1: Hierarchical Address Allocation

**Main Campus Block: 10.50.0.0/19** (8,192 addresses)

| Network | Address Block | Prefix | Hosts Available | Hosts Needed |
|---------|---------------|--------|-----------------|--------------|
| Student Network | 10.50.0.0/21 | /21 | 2,046 | 1,200 |
| Faculty Network | 10.50.8.0/22 | /22 | 1,022 | 800 |
| Guest Network | 10.50.12.0/23 | /23 | 510 | 300 |
| Administration | 10.50.14.0/24 | /24 | 254 | 150 |
| IT Department | 10.50.15.0/25 | /25 | 126 | 80 |

**Branch Campus Block: 10.50.32.0/22** (1,024 addresses)

| Network | Address Block | Prefix | Hosts Available | Hosts Needed |
|---------|---------------|--------|-----------------|--------------|
| Student Network | 10.50.32.0/23 | /23 | 510 | 400 |
| Faculty Network | 10.50.34.0/24 | /24 | 254 | 200 |
| Administration | 10.50.35.0/26 | /26 | 62 | 50 |

**WAN Links Block: 10.50.63.0/24**

| Link | Address Block | Purpose |
|------|---------------|---------|
| Link 1 | 10.50.63.0/30 | Main-Branch Primary |
| Link 2 | 10.50.63.4/30 | Main-Branch Backup |
| Link 3 | 10.50.63.8/30 | Main-ISP Primary |
| Link 4 | 10.50.63.12/30 | Main-ISP Backup |
| Link 5 | 10.50.63.16/30 | Branch-ISP |
| Link 6 | 10.50.63.20/30 | Reserved |
| Link 7 | 10.50.63.24/30 | Reserved |

#### Step 2: Detailed Subnet Breakdown

**Main Campus - Student Network (10.50.0.0/21)**
```
Binary Analysis:
Network: 10.50.0.0/21
Binary:  00001010.00110010.00000000.00000000
Mask:    11111111.11111111.11111000.00000000

Range: 10.50.0.1 - 10.50.7.254
Broadcast: 10.50.7.255
```

**Main Campus - Faculty Network (10.50.8.0/22)**
```
Binary Analysis:
Network: 10.50.8.0/22  
Binary:  00001010.00110010.00001000.00000000
Mask:    11111111.11111111.11111100.00000000

Range: 10.50.8.1 - 10.50.11.254
Broadcast: 10.50.11.255
```

#### Step 3: Route Summarization

**Main Campus Summary:**
- Summary Route: 10.50.0.0/19
- Covers all main campus networks
- Advertised to branch and external networks

**Branch Campus Summary:**
- Summary Route: 10.50.32.0/22
- Covers all branch campus networks
- Advertised to main campus and external networks

---

## Problem Set 3: Complex VLSM with Growth Planning

### Scenario C: ISP Customer Allocation

**Given Information:**
- ISP Allocated Block: 203.45.128.0/19
- Customer Requirements:

#### Large Enterprise Customers:
1. TechCorp: 2000 hosts (current), 50% growth expected
2. ManufacturingInc: 1500 hosts (current), 30% growth expected
3. HealthSystem: 800 hosts (current), 40% growth expected

#### Medium Business Customers:
4. LocalBank: 300 hosts (current), 25% growth expected
5. RetailChain: 250 hosts (current), 35% growth expected
6. ConsultingFirm: 150 hosts (current), 20% growth expected

#### Small Business Customers:
7-16. Various small businesses: 20-50 hosts each

#### Point-to-Point Links:
- 20 customer connection links
- 10 backbone interconnection links

**Solution with Growth Planning:**

#### Step 1: Calculate Growth Requirements

| Customer | Current | Growth % | Future Need | Allocated Size | Prefix |
|----------|---------|----------|-------------|----------------|--------|
| TechCorp | 2000 | 50% | 3000 | 4094 | /20 |
| ManufacturingInc | 1500 | 30% | 1950 | 2046 | /21 |
| HealthSystem | 800 | 40% | 1120 | 2046 | /21 |
| LocalBank | 300 | 25% | 375 | 510 | /23 |
| RetailChain | 250 | 35% | 338 | 510 | /23 |
| ConsultingFirm | 150 | 20% | 180 | 254 | /24 |

#### Step 2: Hierarchical Allocation

**Large Enterprise Block: 203.45.128.0/20** (4,096 addresses)
- TechCorp: 203.45.128.0/20 (entire block)

**Large Enterprise Block: 203.45.144.0/21** (2,048 addresses)  
- ManufacturingInc: 203.45.144.0/21 (entire block)

**Large Enterprise Block: 203.45.152.0/21** (2,048 addresses)
- HealthSystem: 203.45.152.0/21 (entire block)

**Medium Business Block: 203.45.160.0/22** (1,024 addresses)
- LocalBank: 203.45.160.0/23 (512 addresses)
- RetailChain: 203.45.162.0/23 (512 addresses)

**Small Business Block: 203.45.164.0/22** (1,024 addresses)
- ConsultingFirm: 203.45.164.0/24 (256 addresses)
- Small Business 1: 203.45.165.0/26 (64 addresses)
- Small Business 2: 203.45.165.64/26 (64 addresses)
- [Continue allocation pattern...]

#### Step 3: Infrastructure Allocation

**Point-to-Point Links: 203.45.191.0/24**
```
Customer Links (203.45.191.0/26):
- TechCorp Link: 203.45.191.0/30
- ManufacturingInc Link: 203.45.191.4/30  
- HealthSystem Link: 203.45.191.8/30
[Continue for all 20 customers: 203.45.191.0/30 through 203.45.191.76/30]

Backbone Links (203.45.191.128/26):
- Backbone Link 1: 203.45.191.128/30
- Backbone Link 2: 203.45.191.132/30
[Continue for all 10 backbone links: through 203.45.191.164/30]
```

---

## Problem Set 4: VLSM Troubleshooting

### Scenario D: Identifying VLSM Issues

**Problem Statement:**
A network administrator has implemented the following VLSM scheme but is experiencing connectivity issues:

```
Base Network: 172.20.0.0/16

Current Allocation:
Department A: 172.20.1.0/24 (200 hosts needed)
Department B: 172.20.2.0/26 (50 hosts needed)  
Department C: 172.20.2.0/25 (100 hosts needed)
WAN Link 1: 172.20.3.0/30
WAN Link 2: 172.20.3.8/30
```

**Issue Identification & Resolution:**

#### Problem 1: Overlapping Subnets
**Issue:** Department B (172.20.2.0/26) and Department C (172.20.2.0/25) have overlapping address spaces.

**Analysis:**
- Department B: 172.20.2.0/26 covers 172.20.2.0 - 172.20.2.63
- Department C: 172.20.2.0/25 covers 172.20.2.0 - 172.20.2.127
- Overlap: 172.20.2.0 - 172.20.2.63

**Resolution:**
```
Corrected Allocation:
Department A: 172.20.1.0/24 (254 hosts available, 200 needed) ✓
Department C: 172.20.2.0/25 (126 hosts available, 100 needed) ✓
Department B: 172.20.2.128/26 (62 hosts available, 50 needed) ✓
WAN Link 1: 172.20.3.0/30 ✓
WAN Link 2: 172.20.3.4/30 (corrected from /8 spacing)
```

#### Problem 2: Inefficient WAN Link Allocation
**Issue:** WAN Link 2 uses 172.20.3.8/30, wasting addresses between 172.20.3.4-172.20.3.7.

**Best Practice:** Allocate WAN links consecutively for easier management and potential summarization.

---

## Problem Set 5: IPv6 VLSM Concepts

### Scenario E: IPv6 Subnet Planning

**Given Information:**
- Allocated IPv6 Block: 2001:db8:1000::/48
- Network Requirements:

#### Departments:
- Engineering: 4 VLANs
- Sales: 2 VLANs  
- Marketing: 2 VLANs
- IT Operations: 6 VLANs
- Guest Networks: 3 VLANs

#### Point-to-Point Links:
- 8 WAN connections (/127 networks)

**IPv6 VLSM Solution:**

#### Standard /64 Subnet Allocation:
```
Base: 2001:db8:1000::/48 (65,536 /64 subnets available)

Engineering Department:
- VLAN 10: 2001:db8:1000:10::/64
- VLAN 11: 2001:db8:1000:11::/64  
- VLAN 12: 2001:db8:1000:12::/64
- VLAN 13: 2001:db8:1000:13::/64

Sales Department:  
- VLAN 20: 2001:db8:1000:20::/64
- VLAN 21: 2001:db8:1000:21::/64

Marketing Department:
- VLAN 30: 2001:db8:1000:30::/64
- VLAN 31: 2001:db8:1000:31::/64

IT Operations:
- VLAN 40: 2001:db8:1000:40::/64
- VLAN 41: 2001:db8:1000:41::/64
- VLAN 42: 2001:db8:1000:42::/64
- VLAN 43: 2001:db8:1000:43::/64
- VLAN 44: 2001:db8:1000:44::/64
- VLAN 45: 2001:db8:1000:45::/64

Guest Networks:
- Guest-1: 2001:db8:1000:100::/64
- Guest-2: 2001:db8:1000:101::/64  
- Guest-3: 2001:db8:1000:102::/64
```

#### Point-to-Point Links (/127):
```
WAN Links:
- Link 1: 2001:db8:1000:ff00::/127
- Link 2: 2001:db8:1000:ff00::2/127
- Link 3: 2001:db8:1000:ff00::4/127
- Link 4: 2001:db8:1000:ff00::6/127
- Link 5: 2001:db8:1000:ff00::8/127
- Link 6: 2001:db8:1000:ff00::a/127
- Link 7: 2001:db8:1000:ff00::c/127
- Link 8: 2001:db8:1000:ff00::e/127
```

---

## Summary and Key Learning Points

### 1. VLSM Design Principles
- **Hierarchical allocation** from largest to smallest requirements
- **Address conservation** through precise subnet sizing
- **Growth planning** with appropriate overhead allocation
- **Logical organization** aligned with network topology

### 2. Common Pitfalls to Avoid
- **Overlapping subnets** due to poor planning
- **Inefficient allocation** of address space
- **Inadequate growth planning** leading to renumbering
- **Poor documentation** causing management difficulties

### 3. Best Practices Demonstrated
- **Top-down design** approach for optimal allocation
- **Binary verification** to ensure proper boundaries
- **Route summarization** planning for efficiency
- **Standardized allocation** patterns for consistency

### 4. Real-World Applications
- **Enterprise network design** with multiple departments
- **ISP customer allocation** with diverse requirements  
- **Multi-site connectivity** with WAN link optimization
- **IPv6 transition planning** using modern addressing

### 5. Technical Verification Methods
- **Address overlap checking** through binary analysis
- **Subnet boundary validation** using subnet calculators
- **Route table optimization** through summarization
- **Growth impact assessment** for future planning

This comprehensive worksheet demonstrates the practical application of VLSM principles across various networking scenarios, providing both theoretical understanding and hands-on problem-solving experience essential for network design and administration.
