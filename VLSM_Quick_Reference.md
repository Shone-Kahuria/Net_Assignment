# VLSM Quick Reference Guide & Calculation Methods

## Essential VLSM Formulas

### 1. Basic Subnet Calculations

#### Number of Host Addresses
```
Total Addresses = 2^(32-n)
Usable Host Addresses = 2^(32-n) - 2
```
Where `n` = prefix length (subnet mask bits)

#### Number of Subnets  
```
Number of Subnets = 2^(borrowed bits)
```
Where borrowed bits = new prefix length - original prefix length

#### Subnet Increment
```
Subnet Increment = 256 - subnet mask value
```
For the interesting octet (where subnetting occurs)

### 2. Prefix Length to Subnet Mask Conversion

| Prefix | Subnet Mask | Wildcard Mask | Hosts | Subnets (from /24) |
|--------|-------------|---------------|-------|-------------------|
| /24 | 255.255.255.0 | 0.0.0.255 | 254 | 1 |
| /25 | 255.255.255.128 | 0.0.0.127 | 126 | 2 |
| /26 | 255.255.255.192 | 0.0.0.63 | 62 | 4 |
| /27 | 255.255.255.224 | 0.0.0.31 | 30 | 8 |
| /28 | 255.255.255.240 | 0.0.0.15 | 14 | 16 |
| /29 | 255.255.255.248 | 0.0.0.7 | 6 | 32 |
| /30 | 255.255.255.252 | 0.0.0.3 | 2 | 64 |
| /31 | 255.255.255.254 | 0.0.0.1 | 2* | 128 |
| /32 | 255.255.255.255 | 0.0.0.0 | 1 | 256 |

*Note: /31 networks are special case for point-to-point links (RFC 3021)

### 3. VLSM Allocation Strategy

#### Step-by-Step Process
1. **List all subnet requirements** (departments, links, etc.)
2. **Sort by host count** (largest to smallest)
3. **Calculate required subnet size** for each requirement
4. **Determine prefix length** for each subnet
5. **Allocate address space** starting with largest subnets
6. **Verify no overlaps** exist
7. **Document the allocation**

#### Subnet Size Determination
```
Required Hosts → Minimum Subnet Size → Prefix Length
1-2 hosts → 4 addresses → /30
3-6 hosts → 8 addresses → /29  
7-14 hosts → 16 addresses → /28
15-30 hosts → 32 addresses → /27
31-62 hosts → 64 addresses → /26
63-126 hosts → 128 addresses → /25
127-254 hosts → 256 addresses → /24
255-510 hosts → 512 addresses → /23
511-1022 hosts → 1024 addresses → /22
```

## Quick Calculation Examples

### Example 1: Basic VLSM Allocation
**Given:** 192.168.1.0/24
**Requirements:** 100 hosts, 50 hosts, 20 hosts, 2 point-to-point links

**Solution:**
1. Sort: 100, 50, 20, 2, 2
2. Subnet sizes needed: 128(/25), 64(/26), 32(/27), 4(/30), 4(/30)
3. Allocation:
   - 192.168.1.0/25 (100 hosts) → 192.168.1.1-126
   - 192.168.1.128/26 (50 hosts) → 192.168.1.129-190  
   - 192.168.1.192/27 (20 hosts) → 192.168.1.193-222
   - 192.168.1.224/30 (link 1) → 192.168.1.225-226
   - 192.168.1.228/30 (link 2) → 192.168.1.229-230

### Example 2: Binary Verification Method
**Subnet:** 172.16.10.0/26

**Binary Analysis:**
```
IP Address: 172.16.10.0
Binary:     10101100.00010000.00001010.00000000

Subnet Mask: /26 = 255.255.255.192  
Binary:      11111111.11111111.11111111.11000000

Network:     10101100.00010000.00001010.00000000 = 172.16.10.0
First Host:  10101100.00010000.00001010.00000001 = 172.16.10.1
Last Host:   10101100.00010000.00001010.00111110 = 172.16.10.62
Broadcast:   10101100.00010000.00001010.00111111 = 172.16.10.63
Next Network:10101100.00010000.00001010.01000000 = 172.16.10.64
```

## Common VLSM Scenarios & Solutions

### Scenario 1: Corporate Departments
**Problem:** Design subnets for departments with 200, 100, 50, 25 hosts
**Base Network:** 10.1.0.0/16

**Solution Process:**
```
Department A: 200 hosts → need 256 addresses → /24
Department B: 100 hosts → need 128 addresses → /25  
Department C: 50 hosts → need 64 addresses → /26
Department D: 25 hosts → need 32 addresses → /27

Allocation:
10.1.1.0/24   - Department A (254 usable hosts)
10.1.2.0/25   - Department B (126 usable hosts)
10.1.2.128/26 - Department C (62 usable hosts)
10.1.2.192/27 - Department D (30 usable hosts)
```

### Scenario 2: WAN Link Optimization
**Problem:** Allocate 10 point-to-point WAN links efficiently
**Available Block:** 192.168.255.0/24

**Solution:**
```
Use /30 subnets (4 addresses each, 2 usable)
Increment: 4 addresses per subnet

Link 1: 192.168.255.0/30   (.1-.2)
Link 2: 192.168.255.4/30   (.5-.6)
Link 3: 192.168.255.8/30   (.9-.10)
Link 4: 192.168.255.12/30  (.13-.14)
Link 5: 192.168.255.16/30  (.17-.18)
Link 6: 192.168.255.20/30  (.21-.22)
Link 7: 192.168.255.24/30  (.25-.26)
Link 8: 192.168.255.28/30  (.29-.30)
Link 9: 192.168.255.32/30  (.33-.34)
Link 10: 192.168.255.36/30 (.37-.38)

Summary Route: 192.168.255.0/26 (covers first 16 /30 subnets)
```

## Advanced VLSM Techniques

### 1. Route Summarization Planning
When designing VLSM, plan for route summarization:

**Example Hierarchy:**
```
Regional Office: 10.1.0.0/20
├── Building A: 10.1.0.0/22
│   ├── Floor 1: 10.1.0.0/24
│   ├── Floor 2: 10.1.1.0/24  
│   └── Floor 3: 10.1.2.0/24
└── Building B: 10.1.4.0/22
    ├── Floor 1: 10.1.4.0/24
    └── Floor 2: 10.1.5.0/24

Summary to other regions: 10.1.0.0/20
```

### 2. Growth Planning Integration
**Formula for Growth Planning:**
```
Future Requirement = Current Hosts × (1 + Growth Rate)
Allocated Size = Next Power of 2 ≥ Future Requirement + 10% buffer
```

**Example:**
```
Current: 80 hosts
Growth: 25% over 3 years  
Future: 80 × 1.25 = 100 hosts
Buffer: 100 × 1.10 = 110 hosts
Allocation: 128 addresses (/25)
```

### 3. Verification Checklist
- [ ] No overlapping address ranges
- [ ] Adequate addresses for each requirement  
- [ ] Contiguous allocation for summarization
- [ ] Reserved space for future growth
- [ ] Proper documentation completed
- [ ] Routing protocol compatibility verified

## Troubleshooting Common VLSM Issues

### Issue 1: Address Overlap
**Symptoms:** 
- Duplicate IP addresses on network
- Routing problems between subnets
- Connectivity issues

**Diagnosis:**
Use binary analysis to identify overlapping ranges

**Resolution:**
Reallocate subnets to eliminate overlaps

### Issue 2: Insufficient Address Space
**Symptoms:**
- Cannot assign IPs to all required hosts
- DHCP pool exhaustion

**Diagnosis:**
Calculate actual vs. allocated addresses

**Resolution:**
Increase subnet size or implement secondary addressing

### Issue 3: Inefficient Allocation
**Symptoms:**
- High percentage of unused addresses
- Fragmented address space

**Diagnosis:**
Analyze utilization percentages

**Resolution:**
Redesign allocation using proper VLSM methodology

## VLSM Calculation Tools & Methods

### Manual Calculation Method
1. Convert requirements to binary
2. Determine host bits needed  
3. Calculate network bits
4. Verify boundaries
5. Document allocation

### Online Calculator Verification
Use online VLSM calculators to verify manual calculations:
- Input base network and requirements
- Compare results with manual calculations
- Verify no overlaps exist

### Spreadsheet Template Structure
```
Column A: Subnet Name
Column B: Host Requirement  
Column C: Subnet Size
Column D: Prefix Length
Column E: Network Address
Column F: First Host
Column G: Last Host  
Column H: Broadcast Address
Column I: Next Network
```

## IPv6 VLSM Considerations

### Key Differences from IPv4
- Standard subnet size: /64 (18 quintillion addresses)
- Point-to-point links: /127 (2 addresses)
- Loopback: /128 (1 address)
- No broadcast addresses (uses multicast)

### IPv6 Allocation Example
```
Base: 2001:db8::/32
Organization: 2001:db8:1000::/48  
Site: 2001:db8:1000:1::/64
Point-to-Point: 2001:db8:1000:ff00::/127
```

## Summary of Key Points

1. **VLSM enables efficient address utilization** through variable subnet sizes
2. **Proper planning prevents address waste** and future renumbering
3. **Hierarchical design supports route summarization** and scalability
4. **Binary analysis ensures accurate subnet boundaries** and overlap prevention
5. **Documentation is critical** for ongoing network management
6. **Growth planning should be integrated** into initial design
7. **Verification methods prevent implementation errors** and connectivity issues

This quick reference guide provides the essential formulas, methods, and examples needed for successful VLSM implementation and troubleshooting.
