# Ka-Chow Net: The Radiator Springs Network — Complete Solution

**Group ID:** 1401  
**Course:** CSE421 — Computer Networks  
**Date:** August 16, 2026

| Student ID | Name |
|-----------|------|
| 23301038 | Luban Latif |
| 22299322 | Farhan Labib |
| 23301591 | MD Rawnak Ahasan |
| 23301634 | Kazi MD Tanvirul Hasan Sadd |

---

## 1. Base Network Address Derivation

- **1st Group Member's Student ID:** 23301038
- **Last four digits:** 1038
- **Split into pairs:** **10** and **38**
- First pair → 1st octet = **10**
- Second pair → 2nd octet = **38**
- **Base Network Address: `10.38.0.0/16`**
- **Subnet Mask:** `255.255.0.0`

---

## 2. VLSM Subnet Allocation

### 2.1 Host Requirements & Subnet Sizing

Subnets are allocated in the specified order: **SS → DC → FVC → LCT → RBA → MTY → WWM → P2P Links**

| # | Unit/Link | Hosts Needed | Next Power of 2 | Block Size | Prefix | Subnet Mask |
|---|-----------|-------------|-----------------|------------|--------|-------------|
| 1 | SS (Sheriff Station) | 260 | 2⁹ = 512 | 512 | /23 | 255.255.254.0 |
| 2 | DC (Doc-Hudson Clinic) | 180 | 2⁸ = 256 | 256 | /24 | 255.255.255.0 |
| 3 | FVC (Flo V8 Café) | 150 | 2⁸ = 256 | 256 | /24 | 255.255.255.0 |
| 4 | LCT (Luigi Casa-Della Tires) | 220 | 2⁸ = 256 | 256 | /24 | 255.255.255.0 |
| 5 | RBA (Ramone Body Art) | 90 | 2⁷ = 128 | 128 | /25 | 255.255.255.128 |
| 6 | MTY (Mater Tow Yard) | 90 | 2⁷ = 128 | 128 | /25 | 255.255.255.128 |
| 7 | WWM (Wheel Well Motel) | 40 | 2⁶ = 64 | 64 | /26 | 255.255.255.192 |
| 8 | Downtown Hub Transit | 5 | 2³ = 8 | 8 | /29 | 255.255.255.248 |
| 9 | SS–FVC Direct Link | 2 | 2² = 4 | 4 | /30 | 255.255.255.252 |
| 10 | SS–RBA Direct Link | 2 | 2² = 4 | 4 | /30 | 255.255.255.252 |
| 11 | RBA–DC Direct Link | 2 | 2² = 4 | 4 | /30 | 255.255.255.252 |
| 12 | FVC–WWM Direct Link | 2 | 2² = 4 | 4 | /30 | 255.255.255.252 |

### 2.2 VLSM Allocation Table

| # | Subnet Name | Network Address | Prefix | Usable Range | Broadcast | Usable Hosts |
|---|------------|----------------|--------|-------------|-----------|-------------|
| 1 | SS LAN | 10.38.0.0 | /23 | 10.38.0.1 – 10.38.1.254 | 10.38.1.255 | 510 |
| 2 | DC LAN | 10.38.2.0 | /24 | 10.38.2.1 – 10.38.2.254 | 10.38.2.255 | 254 |
| 3 | FVC LAN | 10.38.3.0 | /24 | 10.38.3.1 – 10.38.3.254 | 10.38.3.255 | 254 |
| 4 | LCT LAN | 10.38.4.0 | /24 | 10.38.4.1 – 10.38.4.254 | 10.38.4.255 | 254 |
| 5 | RBA LAN | 10.38.5.0 | /25 | 10.38.5.1 – 10.38.5.126 | 10.38.5.127 | 126 |
| 6 | MTY LAN | 10.38.5.128 | /25 | 10.38.5.129 – 10.38.5.254 | 10.38.5.255 | 126 |
| 7 | WWM LAN | 10.38.6.0 | /26 | 10.38.6.1 – 10.38.6.62 | 10.38.6.63 | 62 |
| 8 | Hub Transit | 10.38.6.64 | /29 | 10.38.6.65 – 10.38.6.70 | 10.38.6.71 | 6 |
| 9 | SS–FVC Link | 10.38.6.72 | /30 | 10.38.6.73 – 10.38.6.74 | 10.38.6.75 | 2 |
| 10 | SS–RBA Link | 10.38.6.76 | /30 | 10.38.6.77 – 10.38.6.78 | 10.38.6.79 | 2 |
| 11 | RBA–DC Link | 10.38.6.80 | /30 | 10.38.6.81 – 10.38.6.82 | 10.38.6.83 | 2 |
| 12 | FVC–WWM Link | 10.38.6.84 | /30 | 10.38.6.85 – 10.38.6.86 | 10.38.6.87 | 2 |

**Total addresses used:** 512 + 256 + 256 + 256 + 128 + 128 + 64 + 8 + 4×4 = 1,624 out of 65,536

### 2.3 VLSM Tree

```
10.38.0.0/16
├─ 10.38.0.0/17
│  └─ 10.38.0.0/18
│     └─ 10.38.0.0/19
│        └─ 10.38.0.0/20
│           └─ 10.38.0.0/21
│              ├─ 10.38.0.0/22
│              │  ├─ 10.38.0.0/23 ─────────────── ✅ SS LAN (260 hosts)
│              │  └─ 10.38.2.0/23
│              │     ├─ 10.38.2.0/24 ───────────── ✅ DC LAN (180 hosts)
│              │     └─ 10.38.3.0/24 ───────────── ✅ FVC LAN (150 hosts)
│              └─ 10.38.4.0/22
│                 ├─ 10.38.4.0/23
│                 │  ├─ 10.38.4.0/24 ───────────── ✅ LCT LAN (220 hosts)
│                 │  └─ 10.38.5.0/24
│                 │     ├─ 10.38.5.0/25 ─────────── ✅ RBA LAN (90 hosts)
│                 │     └─ 10.38.5.128/25 ────────── ✅ MTY LAN (90 hosts)
│                 └─ 10.38.6.0/23
│                    └─ 10.38.6.0/24
│                       ├─ 10.38.6.0/25
│                       │  ├─ 10.38.6.0/26 ──────── ✅ WWM LAN (40 hosts)
│                       │  └─ 10.38.6.64/26
│                       │     └─ 10.38.6.64/27
│                       │        ├─ 10.38.6.64/28
│                       │        │  ├─ 10.38.6.64/29 ── ✅ Hub Transit (5 routers)
│                       │        │  └─ 10.38.6.72/29
│                       │        │     ├─ 10.38.6.72/30 ─ ✅ SS–FVC Link
│                       │        │     └─ 10.38.6.76/30 ─ ✅ SS–RBA Link
│                       │        └─ 10.38.6.80/28
│                       │           └─ 10.38.6.80/29
│                       │              ├─ 10.38.6.80/30 ─ ✅ RBA–DC Link
│                       │              └─ 10.38.6.84/30 ─ ✅ FVC–WWM Link
│                       └─ 10.38.7.0/24 (unused)
└─ 10.38.128.0/17 (unused)
```

---

## 3. Complete IP Address Table

### 3.1 Sheriff Station (SS) — 10.38.0.0/23

| Device | Interface | IP Address | Subnet Mask | Default Gateway | Type |
|--------|-----------|-----------|-------------|-----------------|------|
| R-SS | Gig0/0 (LAN) | 10.38.0.1 | 255.255.254.0 | — | Static |
| R-SS | Gig0/1 (Hub) | 10.38.6.65 | 255.255.255.248 | — | Static |
| R-SS | Se0/1/0 (→FVC) | 10.38.6.73 | 255.255.255.252 | — | Static |
| R-SS | Se0/1/1 (→RBA) | 10.38.6.77 | 255.255.255.252 | — | Static |
| Mail-SS | Fa0 | 10.38.0.2 | 255.255.254.0 | 10.38.0.1 | Static |
| DNS-SS | Fa0 | 10.38.0.3 | 255.255.254.0 | 10.38.0.1 | Static |
| Web-SS | Fa0 | 10.38.0.4 | 255.255.254.0 | 10.38.0.1 | Static |
| Printer-SS | Fa0 | 10.38.0.5 | 255.255.254.0 | 10.38.0.1 | Static |
| PC-SS-1 | Fa0 | DHCP | — | 10.38.0.1 | DHCP |
| PC-SS-2 | Fa0 | DHCP | — | 10.38.0.1 | DHCP |

### 3.2 Doc-Hudson Clinic (DC) — 10.38.2.0/24

| Device | Interface | IP Address | Subnet Mask | Default Gateway | Type |
|--------|-----------|-----------|-------------|-----------------|------|
| R-DC | Gig0/0 (LAN) | 10.38.2.1 | 255.255.255.0 | — | Static |
| R-DC | Gig0/1 (Hub) | 10.38.6.67 | 255.255.255.248 | — | Static |
| R-DC | Se0/1/0 (→RBA) | 10.38.6.82 | 255.255.255.252 | — | Static |
| Mail-DC | Fa0 | 10.38.2.2 | 255.255.255.0 | 10.38.2.1 | Static |
| Printer-DC | Fa0 | 10.38.2.3 | 255.255.255.0 | 10.38.2.1 | Static |
| PC-DC-1 | Fa0 | DHCP | — | 10.38.2.1 | DHCP |
| PC-DC-2 | Fa0 | DHCP | — | 10.38.2.1 | DHCP |

### 3.3 Flo V8 Café (FVC) — 10.38.3.0/24

| Device | Interface | IP Address | Subnet Mask | Default Gateway | Type |
|--------|-----------|-----------|-------------|-----------------|------|
| R-FVC | Gig0/0 (LAN) | 10.38.3.1 | 255.255.255.0 | — | Static |
| R-FVC | Gig0/1 (Hub) | 10.38.6.68 | 255.255.255.248 | — | Static |
| R-FVC | Se0/1/0 (→SS) | 10.38.6.74 | 255.255.255.252 | — | Static |
| R-FVC | Se0/1/1 (→WWM) | 10.38.6.85 | 255.255.255.252 | — | Static |
| Mail-FVC | Fa0 | 10.38.3.2 | 255.255.255.0 | 10.38.3.1 | Static |
| Web-FVC | Fa0 | 10.38.3.3 | 255.255.255.0 | 10.38.3.1 | Static |
| DHCP-FVC | Fa0 | 10.38.3.4 | 255.255.255.0 | 10.38.3.1 | Static |
| Printer-FVC | Fa0 | 10.38.3.5 | 255.255.255.0 | 10.38.3.1 | Static |
| PC-FVC-1 | Fa0 | DHCP | — | 10.38.3.1 | DHCP |
| PC-FVC-2 | Fa0 | DHCP | — | 10.38.3.1 | DHCP |

### 3.4 Luigi Casa-Della Tires (LCT) — 10.38.4.0/24

| Device | Interface | IP Address | Subnet Mask | Default Gateway | Type |
|--------|-----------|-----------|-------------|-----------------|------|
| R-LCT | Gig0/0 (LAN) | 10.38.4.1 | 255.255.255.0 | — | Static |
| R-LCT | Gig0/1 (Hub) | 10.38.6.66 | 255.255.255.248 | — | Static |
| Mail-LCT | Fa0 | 10.38.4.2 | 255.255.255.0 | 10.38.4.1 | Static |
| DHCP-LCT | Fa0 | 10.38.4.3 | 255.255.255.0 | 10.38.4.1 | Static |
| Printer-LCT | Fa0 | 10.38.4.4 | 255.255.255.0 | 10.38.4.1 | Static |
| PC-LCT-1 | Fa0 | DHCP | — | 10.38.4.1 | DHCP |
| PC-LCT-2 | Fa0 | DHCP | — | 10.38.4.1 | DHCP |

### 3.5 Ramone Body Art (RBA) — 10.38.5.0/25

| Device | Interface | IP Address | Subnet Mask | Default Gateway | Type |
|--------|-----------|-----------|-------------|-----------------|------|
| R-RBA | Gig0/0 (LAN) | 10.38.5.1 | 255.255.255.128 | — | Static |
| R-RBA | Se0/1/0 (→SS) | 10.38.6.78 | 255.255.255.252 | — | Static |
| R-RBA | Se0/1/1 (→DC) | 10.38.6.81 | 255.255.255.252 | — | Static |
| Mail-RBA | Fa0 | 10.38.5.2 | 255.255.255.128 | 10.38.5.1 | Static |
| Printer-RBA | Fa0 | 10.38.5.3 | 255.255.255.128 | 10.38.5.1 | Static |
| PC-RBA-1 | Fa0 | DHCP | — | 10.38.5.1 | DHCP |
| PC-RBA-2 | Fa0 | DHCP | — | 10.38.5.1 | DHCP |

### 3.6 Mater Tow Yard (MTY) — 10.38.5.128/25

| Device | Interface | IP Address | Subnet Mask | Default Gateway | Type |
|--------|-----------|-----------|-------------|-----------------|------|
| R-MTY | Gig0/0 (LAN) | 10.38.5.129 | 255.255.255.128 | — | Static |
| R-MTY | Gig0/1 (Hub) | 10.38.6.69 | 255.255.255.248 | — | Static |
| Mail-MTY | Fa0 | 10.38.5.130 | 255.255.255.128 | 10.38.5.129 | Static |
| Printer-MTY | Fa0 | 10.38.5.131 | 255.255.255.128 | 10.38.5.129 | Static |
| PC-MTY-1 | Fa0 | DHCP | — | 10.38.5.129 | DHCP |
| PC-MTY-2 | Fa0 | DHCP | — | 10.38.5.129 | DHCP |

### 3.7 Wheel Well Motel (WWM) — 10.38.6.0/26

| Device | Interface | IP Address | Subnet Mask | Default Gateway | Type |
|--------|-----------|-----------|-------------|-----------------|------|
| R-WWM | Gig0/0 (LAN) | 10.38.6.1 | 255.255.255.192 | — | Static |
| R-WWM | Se0/1/0 (→FVC) | 10.38.6.86 | 255.255.255.252 | — | Static |
| Mail-WWM | Fa0 | 10.38.6.2 | 255.255.255.192 | 10.38.6.1 | Static |
| Printer-WWM | Fa0 | 10.38.6.3 | 255.255.255.192 | 10.38.6.1 | Static |
| PC-WWM-1 | Fa0 | DHCP | — | 10.38.6.1 | DHCP |
| PC-WWM-2 | Fa0 | DHCP | — | 10.38.6.1 | DHCP |

### 3.8 Inter-Router Links Summary

| Link | Subnet | Router A | IP A | Router B | IP B |
|------|--------|----------|------|----------|------|
| Hub Transit | 10.38.6.64/29 | R-SS Gig0/1 | .65 | R-LCT Gig0/1 | .66 |
| | | R-DC Gig0/1 | .67 | R-FVC Gig0/1 | .68 |
| | | R-MTY Gig0/1 | .69 | | |
| SS–FVC Serial | 10.38.6.72/30 | R-SS Se0/1/0 | .73 | R-FVC Se0/1/0 | .74 |
| SS–RBA Serial | 10.38.6.76/30 | R-SS Se0/1/1 | .77 | R-RBA Se0/1/0 | .78 |
| RBA–DC Serial | 10.38.6.80/30 | R-RBA Se0/1/1 | .81 | R-DC Se0/1/0 | .82 |
| FVC–WWM Serial | 10.38.6.84/30 | R-FVC Se0/1/1 | .85 | R-WWM Se0/1/0 | .86 |

---

## 4. Router Configuration Commands

> **Note:** All routers are Cisco 2911. Routers requiring serial interfaces have a HWIC-2T module installed in slot 0/1. The router must be powered off in Packet Tracer before adding the module.

### 4.1 R-SS (Sheriff Station Router)

```
enable
configure terminal
hostname R-SS
no ip domain-lookup

! ===== INTERFACE CONFIGURATION =====

interface GigabitEthernet0/0
 description SS LAN
 ip address 10.38.0.1 255.255.254.0
 no shutdown

interface GigabitEthernet0/1
 description Downtown Hub Transit
 ip address 10.38.6.65 255.255.255.248
 no shutdown

interface Serial0/1/0
 description SS-FVC Direct Link (DCE)
 ip address 10.38.6.73 255.255.255.252
 clock rate 64000
 no shutdown

interface Serial0/1/1
 description SS-RBA Direct Link (DCE)
 ip address 10.38.6.77 255.255.255.252
 clock rate 64000
 no shutdown

! ===== DHCP CONFIGURATION (Server for SS, RBA, DC) =====

ip dhcp excluded-address 10.38.0.1 10.38.0.10
ip dhcp excluded-address 10.38.5.1 10.38.5.10
ip dhcp excluded-address 10.38.2.1 10.38.2.10

ip dhcp pool SS_POOL
 network 10.38.0.0 255.255.254.0
 default-router 10.38.0.1
 dns-server 10.38.0.3

ip dhcp pool RBA_POOL
 network 10.38.5.0 255.255.255.128
 default-router 10.38.5.1
 dns-server 10.38.0.3

ip dhcp pool DC_POOL
 network 10.38.2.0 255.255.255.0
 default-router 10.38.2.1
 dns-server 10.38.0.3

! ===== STATIC ROUTES =====

ip route 10.38.4.0 255.255.255.0 10.38.6.66
ip route 10.38.3.0 255.255.255.0 10.38.6.68
ip route 10.38.5.128 255.255.255.128 10.38.6.69
ip route 10.38.6.0 255.255.255.192 10.38.6.68
ip route 10.38.6.84 255.255.255.252 10.38.6.68

! ===== RIPv2 CONFIGURATION =====

router rip
 version 2
 network 10.0.0.0
 passive-interface GigabitEthernet0/0
 passive-interface Serial0/1/0
 no auto-summary
 redistribute static metric 1

end
write memory
```

### 4.2 R-DC (Doc-Hudson Clinic Router)

```
enable
configure terminal
hostname R-DC
no ip domain-lookup

! ===== INTERFACE CONFIGURATION =====

interface GigabitEthernet0/0
 description DC LAN
 ip address 10.38.2.1 255.255.255.0
 no shutdown

interface GigabitEthernet0/1
 description Downtown Hub Transit
 ip address 10.38.6.67 255.255.255.248
 no shutdown

interface Serial0/1/0
 description RBA-DC Direct Link (DTE)
 ip address 10.38.6.82 255.255.255.252
 no shutdown

! ===== DHCP RELAY (to SS Router) =====

interface GigabitEthernet0/0
 ip helper-address 10.38.6.65

! ===== RIPv2 CONFIGURATION =====

router rip
 version 2
 network 10.0.0.0
 passive-interface GigabitEthernet0/0
 no auto-summary

end
write memory
```

### 4.3 R-FVC (Flo V8 Café Router)

```
enable
configure terminal
hostname R-FVC
no ip domain-lookup

! ===== INTERFACE CONFIGURATION =====

interface GigabitEthernet0/0
 description FVC LAN
 ip address 10.38.3.1 255.255.255.0
 no shutdown

interface GigabitEthernet0/1
 description Downtown Hub Transit
 ip address 10.38.6.68 255.255.255.248
 no shutdown

interface Serial0/1/0
 description SS-FVC Direct Link (DTE)
 ip address 10.38.6.74 255.255.255.252
 no shutdown

interface Serial0/1/1
 description FVC-WWM Link (DCE)
 ip address 10.38.6.85 255.255.255.252
 clock rate 64000
 no shutdown

! ===== EXIT-INTERFACE STATIC ROUTES =====

! Primary route to SS LAN via direct serial link
ip route 10.38.0.0 255.255.254.0 Serial0/1/0

! DC LAN via hub
ip route 10.38.2.0 255.255.255.0 GigabitEthernet0/1

! LCT LAN via hub
ip route 10.38.4.0 255.255.255.0 GigabitEthernet0/1

! RBA LAN via hub
ip route 10.38.5.0 255.255.255.128 GigabitEthernet0/1

! MTY LAN via hub
ip route 10.38.5.128 255.255.255.128 GigabitEthernet0/1

! WWM LAN via FVC-WWM serial
ip route 10.38.6.0 255.255.255.192 Serial0/1/1

! SS-RBA Link via hub
ip route 10.38.6.76 255.255.255.252 GigabitEthernet0/1

! RBA-DC Link via hub
ip route 10.38.6.80 255.255.255.252 GigabitEthernet0/1

! ===== RECURSIVE BACKUP ROUTE TO SS LAN =====
! If Se0/1/0 (direct SS-FVC link) fails, reach SS via LCT
! Next-hop 10.38.4.1 is NOT directly connected → recursive lookup
! AD 10 makes it a floating route (only active when primary is removed)

ip route 10.38.0.0 255.255.254.0 10.38.4.1 10

end
write memory
```

### 4.4 R-LCT (Luigi Casa-Della Tires Router)

```
enable
configure terminal
hostname R-LCT
no ip domain-lookup

! ===== INTERFACE CONFIGURATION =====

interface GigabitEthernet0/0
 description LCT LAN
 ip address 10.38.4.1 255.255.255.0
 no shutdown

interface GigabitEthernet0/1
 description Downtown Hub Transit
 ip address 10.38.6.66 255.255.255.248
 no shutdown

! ===== STATIC ROUTES (no default route permitted) =====

! SS LAN — PRIMARY via SS hub IP
ip route 10.38.0.0 255.255.254.0 10.38.6.65

! DC LAN via DC hub IP
ip route 10.38.2.0 255.255.255.0 10.38.6.67

! FVC LAN via FVC hub IP
ip route 10.38.3.0 255.255.255.0 10.38.6.68

! RBA LAN via SS hub IP (SS->SS-RBA->RBA)
ip route 10.38.5.0 255.255.255.128 10.38.6.65

! MTY LAN via MTY hub IP
ip route 10.38.5.128 255.255.255.128 10.38.6.69

! WWM LAN via FVC hub IP (FVC->FVC-WWM->WWM)
ip route 10.38.6.0 255.255.255.192 10.38.6.68

! SS-FVC Link via SS hub IP
ip route 10.38.6.72 255.255.255.252 10.38.6.65

! SS-RBA Link via SS hub IP
ip route 10.38.6.76 255.255.255.252 10.38.6.65

! RBA-DC Link via DC hub IP
ip route 10.38.6.80 255.255.255.252 10.38.6.67

! FVC-WWM Link via FVC hub IP
ip route 10.38.6.84 255.255.255.252 10.38.6.68

! ===== FLOATING STATIC BACKUP TO SS LAN VIA DC =====
! AD 5 -> only active when primary route (AD 1) to SS LAN is removed
! Path: LCT -> Hub -> DC -> RBA-DC -> RBA -> SS-RBA -> SS

ip route 10.38.0.0 255.255.254.0 10.38.6.67 5

end
write memory
```

### 4.5 R-RBA (Ramone Body Art Router)

```
enable
configure terminal
hostname R-RBA
no ip domain-lookup

! ===== INTERFACE CONFIGURATION =====

interface GigabitEthernet0/0
 description RBA LAN
 ip address 10.38.5.1 255.255.255.128
 no shutdown

interface Serial0/1/0
 description SS-RBA Direct Link (DTE)
 ip address 10.38.6.78 255.255.255.252
 no shutdown

interface Serial0/1/1
 description RBA-DC Direct Link (DCE)
 ip address 10.38.6.81 255.255.255.252
 clock rate 64000
 no shutdown

! ===== DHCP RELAY (to SS Router) =====

interface GigabitEthernet0/0
 ip helper-address 10.38.6.77

! ===== RIPv2 CONFIGURATION =====
! No static routes needed — all learned via RIPv2

router rip
 version 2
 network 10.0.0.0
 passive-interface GigabitEthernet0/0
 no auto-summary

end
write memory
```

### 4.6 R-MTY (Mater Tow Yard Router)

```
enable
configure terminal
hostname R-MTY
no ip domain-lookup

! ===== INTERFACE CONFIGURATION =====

interface GigabitEthernet0/0
 description MTY LAN
 ip address 10.38.5.129 255.255.255.128
 no shutdown

interface GigabitEthernet0/1
 description Downtown Hub Transit
 ip address 10.38.6.69 255.255.255.248
 no shutdown

! ===== DHCP RELAY (to LCT DHCP Server) =====

interface GigabitEthernet0/0
 ip helper-address 10.38.4.3

! ===== STATIC ROUTES (no default route permitted) =====

! SS LAN via SS hub IP
ip route 10.38.0.0 255.255.254.0 10.38.6.65

! DC LAN via DC hub IP
ip route 10.38.2.0 255.255.255.0 10.38.6.67

! FVC LAN via FVC hub IP
ip route 10.38.3.0 255.255.255.0 10.38.6.68

! LCT LAN via LCT hub IP
ip route 10.38.4.0 255.255.255.0 10.38.6.66

! RBA LAN via SS hub IP (SS->SS-RBA->RBA)
ip route 10.38.5.0 255.255.255.128 10.38.6.65

! WWM LAN via FVC hub IP (FVC->FVC-WWM->WWM)
ip route 10.38.6.0 255.255.255.192 10.38.6.68

! SS-FVC Link via SS hub IP
ip route 10.38.6.72 255.255.255.252 10.38.6.65

! SS-RBA Link via SS hub IP
ip route 10.38.6.76 255.255.255.252 10.38.6.65

! RBA-DC Link via DC hub IP
ip route 10.38.6.80 255.255.255.252 10.38.6.67

! FVC-WWM Link via FVC hub IP
ip route 10.38.6.84 255.255.255.252 10.38.6.68

end
write memory
```

### 4.7 R-WWM (Wheel Well Motel Router)

```
enable
configure terminal
hostname R-WWM
no ip domain-lookup

! ===== INTERFACE CONFIGURATION =====

interface GigabitEthernet0/0
 description WWM LAN
 ip address 10.38.6.1 255.255.255.192
 no shutdown

interface Serial0/1/0
 description FVC-WWM Link (DTE)
 ip address 10.38.6.86 255.255.255.252
 no shutdown

! ===== DHCP RELAY (to FVC DHCP Server) =====

interface GigabitEthernet0/0
 ip helper-address 10.38.3.4

! ===== DEFAULT ROUTE =====
! WWM can only reach the rest of the network through FVC

ip route 0.0.0.0 0.0.0.0 10.38.6.85

end
write memory
```

---

## 5. Server Configurations (Packet Tracer GUI)

### 5.1 DNS Server — DNS-SS (10.38.0.3)

**Location:** Sheriff Station LAN  
**Packet Tracer:** Services > DNS > Toggle **ON**

| Record Type | Domain Name | Address |
|------------|------------|---------|
| A Record | mail.sheriff.rs | 10.38.0.2 |
| A Record | mail.doc.rs | 10.38.2.2 |
| A Record | mail.flo.rs | 10.38.3.2 |
| A Record | mail.luigi.rs | 10.38.4.2 |
| A Record | mail.ramone.rs | 10.38.5.2 |
| A Record | mail.mater.rs | 10.38.5.130 |
| A Record | mail.wheelwell.rs | 10.38.6.2 |
| A Record | www.sheriff.radiatorsprings.nyc | 10.38.0.4 |
| A Record | www.flo.radiatorsprings.nyc | 10.38.3.3 |
| A Record | www.flosv8.radiatorsprings.nyc | 10.38.3.3 |

**Static IP Configuration:**
- IP: 10.38.0.3, Mask: 255.255.254.0, Gateway: 10.38.0.1, DNS: 10.38.0.3

### 5.2 Web Servers

#### Web-SS (10.38.0.4) — www.sheriff.radiatorsprings.nyc
**Packet Tracer:** Services > HTTP > Toggle **ON**  
Edit `index.html` to display: "Welcome to Sheriff Station — Radiator Springs"

**Static IP:** 10.38.0.4, Mask: 255.255.254.0, Gateway: 10.38.0.1, DNS: 10.38.0.3

#### Web-FVC (10.38.3.3) — www.flo.radiatorsprings.nyc
**Packet Tracer:** Services > HTTP > Toggle **ON**  
Edit `index.html` to display: "Welcome to Flo's V8 Cafe — Radiator Springs"

**Static IP:** 10.38.3.3, Mask: 255.255.255.0, Gateway: 10.38.3.1, DNS: 10.38.0.3

### 5.3 Email Servers

#### Mail-SS (10.38.0.2) — sheriff.rs
**Packet Tracer:** Services > EMAIL > Toggle SMTP & POP3 **ON**

| Setting | Value |
|---------|-------|
| Domain Name | sheriff.rs |
| User 1 | sheriff1 / password: cisco |
| User 2 | sheriff2 / password: cisco |

**Static IP:** 10.38.0.2, Mask: 255.255.254.0, Gateway: 10.38.0.1, DNS: 10.38.0.3

#### Mail-FVC (10.38.3.2) — flo.rs
**Packet Tracer:** Services > EMAIL > Toggle SMTP & POP3 **ON**

| Setting | Value |
|---------|-------|
| Domain Name | flo.rs |
| User 1 | flo1 / password: cisco |
| User 2 | flo2 / password: cisco |

**Static IP:** 10.38.3.2, Mask: 255.255.255.0, Gateway: 10.38.3.1, DNS: 10.38.0.3

#### Mail-DC (10.38.2.2) — doc.rs
- Domain: doc.rs | Users: doc1, doc2 (password: cisco)
- Static IP: 10.38.2.2, Mask: 255.255.255.0, GW: 10.38.2.1, DNS: 10.38.0.3

#### Mail-LCT (10.38.4.2) — luigi.rs
- Domain: luigi.rs | Users: luigi1, luigi2 (password: cisco)
- Static IP: 10.38.4.2, Mask: 255.255.255.0, GW: 10.38.4.1, DNS: 10.38.0.3

#### Mail-RBA (10.38.5.2) — ramone.rs
- Domain: ramone.rs | Users: ramone1, ramone2 (password: cisco)
- Static IP: 10.38.5.2, Mask: 255.255.255.128, GW: 10.38.5.1, DNS: 10.38.0.3

#### Mail-MTY (10.38.5.130) — mater.rs
- Domain: mater.rs | Users: mater1, mater2 (password: cisco)
- Static IP: 10.38.5.130, Mask: 255.255.255.128, GW: 10.38.5.129, DNS: 10.38.0.3

#### Mail-WWM (10.38.6.2) — wheelwell.rs
- Domain: wheelwell.rs | Users: wheelwell1, wheelwell2 (password: cisco)
- Static IP: 10.38.6.2, Mask: 255.255.255.192, GW: 10.38.6.1, DNS: 10.38.0.3

### 5.4 DHCP Servers (Dedicated Server Devices)

#### DHCP-LCT (10.38.4.3) — Serves LCT and MTY

**Packet Tracer:** Services > DHCP > Toggle **ON**

**Pool 1 — LCT LAN:**

| Setting | Value |
|---------|-------|
| Pool Name | lct_pool |
| Default Gateway | 10.38.4.1 |
| DNS Server | 10.38.0.3 |
| Start IP Address | 10.38.4.11 |
| Subnet Mask | 255.255.255.0 |
| Max Users | 240 |

**Pool 2 — MTY LAN (click "Add" to create second pool):**

| Setting | Value |
|---------|-------|
| Pool Name | mty_pool |
| Default Gateway | 10.38.5.129 |
| DNS Server | 10.38.0.3 |
| Start IP Address | 10.38.5.139 |
| Subnet Mask | 255.255.255.128 |
| Max Users | 100 |

**Static IP:** 10.38.4.3, Mask: 255.255.255.0, GW: 10.38.4.1, DNS: 10.38.0.3

#### DHCP-FVC (10.38.3.4) — Serves FVC and WWM

**Packet Tracer:** Services > DHCP > Toggle **ON**

**Pool 1 — FVC LAN:**

| Setting | Value |
|---------|-------|
| Pool Name | fvc_pool |
| Default Gateway | 10.38.3.1 |
| DNS Server | 10.38.0.3 |
| Start IP Address | 10.38.3.11 |
| Subnet Mask | 255.255.255.0 |
| Max Users | 240 |

**Pool 2 — WWM LAN (click "Add" to create second pool):**

| Setting | Value |
|---------|-------|
| Pool Name | wwm_pool |
| Default Gateway | 10.38.6.1 |
| DNS Server | 10.38.0.3 |
| Start IP Address | 10.38.6.11 |
| Subnet Mask | 255.255.255.192 |
| Max Users | 50 |

**Static IP:** 10.38.3.4, Mask: 255.255.255.0, GW: 10.38.3.1, DNS: 10.38.0.3

### 5.5 PC Email Client Configuration (for cross-domain testing)

#### PC-SS-1 (Email Client)
Desktop > Email:

| Setting | Value |
|---------|-------|
| Your Name | Sheriff1 |
| Email Address | sheriff1@sheriff.rs |
| Incoming Mail Server | mail.sheriff.rs |
| Outgoing Mail Server | mail.sheriff.rs |
| User Name | sheriff1 |
| Password | cisco |

#### PC-SS-2 (Email Client)
Same as above but: sheriff2@sheriff.rs, User: sheriff2

#### PC-FVC-1 (Email Client)
Desktop > Email:

| Setting | Value |
|---------|-------|
| Your Name | Flo1 |
| Email Address | flo1@flo.rs |
| Incoming Mail Server | mail.flo.rs |
| Outgoing Mail Server | mail.flo.rs |
| User Name | flo1 |
| Password | cisco |

#### PC-FVC-2 (Email Client)
Same as above but: flo2@flo.rs, User: flo2

### 5.6 All Server & Printer Static IPs — Quick Reference

| Server | IP Address | Subnet Mask | Gateway | DNS Server |
|--------|-----------|-------------|---------|-----------|
| DNS-SS | 10.38.0.3 | 255.255.254.0 | 10.38.0.1 | 10.38.0.3 |
| Mail-SS | 10.38.0.2 | 255.255.254.0 | 10.38.0.1 | 10.38.0.3 |
| Web-SS | 10.38.0.4 | 255.255.254.0 | 10.38.0.1 | 10.38.0.3 |
| Printer-SS | 10.38.0.5 | 255.255.254.0 | 10.38.0.1 | 10.38.0.3 |
| Mail-DC | 10.38.2.2 | 255.255.255.0 | 10.38.2.1 | 10.38.0.3 |
| Printer-DC | 10.38.2.3 | 255.255.255.0 | 10.38.2.1 | 10.38.0.3 |
| Mail-FVC | 10.38.3.2 | 255.255.255.0 | 10.38.3.1 | 10.38.0.3 |
| Web-FVC | 10.38.3.3 | 255.255.255.0 | 10.38.3.1 | 10.38.0.3 |
| DHCP-FVC | 10.38.3.4 | 255.255.255.0 | 10.38.3.1 | 10.38.0.3 |
| Printer-FVC | 10.38.3.5 | 255.255.255.0 | 10.38.3.1 | 10.38.0.3 |
| Mail-LCT | 10.38.4.2 | 255.255.255.0 | 10.38.4.1 | 10.38.0.3 |
| DHCP-LCT | 10.38.4.3 | 255.255.255.0 | 10.38.4.1 | 10.38.0.3 |
| Printer-LCT | 10.38.4.4 | 255.255.255.0 | 10.38.4.1 | 10.38.0.3 |
| Mail-RBA | 10.38.5.2 | 255.255.255.128 | 10.38.5.1 | 10.38.0.3 |
| Printer-RBA | 10.38.5.3 | 255.255.255.128 | 10.38.5.1 | 10.38.0.3 |
| Mail-MTY | 10.38.5.130 | 255.255.255.128 | 10.38.5.129 | 10.38.0.3 |
| Printer-MTY | 10.38.5.131 | 255.255.255.128 | 10.38.5.129 | 10.38.0.3 |
| Mail-WWM | 10.38.6.2 | 255.255.255.192 | 10.38.6.1 | 10.38.0.3 |
| Printer-WWM | 10.38.6.3 | 255.255.255.192 | 10.38.6.1 | 10.38.0.3 |

---

## 6. Testing & Verification Procedures

### 6.1 Full Connectivity Test
From any PC, ping all other unit gateways:
```
ping 10.38.0.1     (SS Gateway)
ping 10.38.2.1     (DC Gateway)
ping 10.38.3.1     (FVC Gateway)
ping 10.38.4.1     (LCT Gateway)
ping 10.38.5.1     (RBA Gateway)
ping 10.38.5.129   (MTY Gateway)
ping 10.38.6.1     (WWM Gateway)
```

### 6.2 FVC Recursive Route Failover Test
1. From PC-FVC-1, ping 10.38.0.1 (SS gateway) — **should succeed** via Se0/1/0
2. On R-FVC: `show ip route 10.38.0.0` — verify route via Serial0/1/0
3. On R-FVC: `configure terminal` > `interface Serial0/1/0` > `shutdown`
4. On R-FVC: `show ip route 10.38.0.0` — verify route now shows next-hop 10.38.4.1 (recursive via LCT)
5. From PC-FVC-1, ping 10.38.0.1 — **should succeed** via recursive route
6. Restore: `no shutdown` on Se0/1/0

### 6.3 LCT Floating Static Route Failover Test
1. From PC-LCT-1, ping 10.38.0.1 (SS gateway) — **should succeed** via Hub (10.38.6.65)
2. On R-LCT: `show ip route 10.38.0.0` — verify route via 10.38.6.65 (AD 1)
3. On R-SS: `configure terminal` > `interface GigabitEthernet0/1` > `shutdown`
4. On R-LCT: Remove stale primary route:
   `configure terminal` > `no ip route 10.38.0.0 255.255.254.0 10.38.6.65`
5. On R-LCT: `show ip route 10.38.0.0` — verify floating route via 10.38.6.67 (DC, AD 5) is now active
6. Path: LCT > Hub > DC > RBA-DC > RBA > SS-RBA > SS
7. From PC-LCT-1, ping 10.38.0.1 — **should succeed** via floating route
8. Restore: On R-SS `no shutdown` on Gig0/1, re-add primary route on R-LCT

### 6.4 Cross-Domain Email Test
1. On PC-SS-1: Desktop > Email > Compose
   - To: flo1@flo.rs
   - Subject: Test from Sheriff
   - Send
2. On PC-FVC-1: Desktop > Email > Receive — verify email received
3. On PC-FVC-1: Reply or compose new email to sheriff1@sheriff.rs > Send
4. On PC-SS-1: Receive — verify reply received

### 6.5 Web Access Test
From a PC in **every unit**, open Desktop > Web Browser:
- Navigate to: `www.sheriff.radiatorsprings.nyc` — should display Sheriff Station page
- Navigate to: `www.flo.radiatorsprings.nyc` — should display Flo's V8 Cafe page
- Navigate to: `www.flosv8.radiatorsprings.nyc` — should also display Flo's page

---

## 7. Assumptions

1. **Router Model:** Cisco 2911. Routers requiring serial interfaces (R-SS, R-DC, R-FVC, R-RBA, R-WWM) have a **HWIC-2T** module installed in slot 0/1 before powering on.
2. **Switch Model:** Cisco 2960 for all LAN switches and the Downtown Hub central switch.
3. **Serial Link Parameters:** Clock rate 64000 bps, HDLC encapsulation (default). DCE side sets the clock rate; DTE side does not.
4. **DCE/DTE Assignment:** The first router listed in each serial link is DCE: R-SS is DCE on SS-FVC and SS-RBA links; R-RBA is DCE on RBA-DC; R-FVC is DCE on FVC-WWM.
5. **Proxy ARP:** Enabled by default on all router interfaces. Required for FVC's exit-interface static routes via the hub to function correctly (routers on the hub respond to ARP requests via proxy ARP).
6. **Email Passwords:** All email user accounts use the password `cisco` for simplicity.
7. **DHCP Exclusion Range:** The first 10 addresses in each LAN subnet are excluded from DHCP pools to reserve space for static devices (router, servers, printers).
8. **Web Server Domain Discrepancy:** Both `www.flo.radiatorsprings.nyc` and `www.flosv8.radiatorsprings.nyc` resolve to the same FVC web server (10.38.3.3), addressing the naming difference between the requirements and testing sections of the assignment.
9. **LCT Floating Route Test Procedure:** Since LCT connects to SS only via the shared hub, shutting down SS's hub interface alone does not remove LCT's static route from its routing table. To demonstrate the floating route, the primary route is manually removed after shutting down SS's hub interface.
10. **DNS for All Devices:** Every device (PCs, servers, printers) is configured with DNS server 10.38.0.3 (DNS-SS at Sheriff Station).
11. **Point-to-Point Links:** All inter-router direct connections (SS-FVC, SS-RBA, RBA-DC, FVC-WWM) use serial links. The Downtown Hub uses Ethernet connections to a shared Layer 2 switch.
12. **Server Device Separation:** Each service (DNS, Web, Email, DHCP) runs on a separate dedicated Server device in Packet Tracer for clarity.

---

## 8. Physical Topology — Connections Checklist

### Cabling Table (for building in Packet Tracer)

| # | From Device | From Port | To Device | To Port | Cable Type |
|---|------------|-----------|-----------|---------|-----------|
| 1 | R-SS | Gig0/0 | SW-SS | Fa0/1 | Straight-through |
| 2 | R-SS | Gig0/1 | Hub-Switch | Fa0/1 | Straight-through |
| 3 | R-SS | Se0/1/0 | R-FVC | Se0/1/0 | Serial DCE |
| 4 | R-SS | Se0/1/1 | R-RBA | Se0/1/0 | Serial DCE |
| 5 | R-LCT | Gig0/0 | SW-LCT | Fa0/1 | Straight-through |
| 6 | R-LCT | Gig0/1 | Hub-Switch | Fa0/2 | Straight-through |
| 7 | R-DC | Gig0/0 | SW-DC | Fa0/1 | Straight-through |
| 8 | R-DC | Gig0/1 | Hub-Switch | Fa0/3 | Straight-through |
| 9 | R-DC | Se0/1/0 | R-RBA | Se0/1/1 | Serial DTE |
| 10 | R-FVC | Gig0/0 | SW-FVC | Fa0/1 | Straight-through |
| 11 | R-FVC | Gig0/1 | Hub-Switch | Fa0/4 | Straight-through |
| 12 | R-FVC | Se0/1/1 | R-WWM | Se0/1/0 | Serial DCE |
| 13 | R-MTY | Gig0/0 | SW-MTY | Fa0/1 | Straight-through |
| 14 | R-MTY | Gig0/1 | Hub-Switch | Fa0/5 | Straight-through |
| 15 | R-RBA | Gig0/0 | SW-RBA | Fa0/1 | Straight-through |
| 16 | R-WWM | Gig0/0 | SW-WWM | Fa0/1 | Straight-through |

### LAN Device Connections (connect all to respective unit switch)

| Unit | Device | Connect to Switch Port |
|------|--------|----------------------|
| SS | PC-SS-1 | SW-SS Fa0/2 |
| SS | PC-SS-2 | SW-SS Fa0/3 |
| SS | Mail-SS | SW-SS Fa0/4 |
| SS | DNS-SS | SW-SS Fa0/5 |
| SS | Web-SS | SW-SS Fa0/6 |
| SS | Printer-SS | SW-SS Fa0/7 |
| DC | PC-DC-1 | SW-DC Fa0/2 |
| DC | PC-DC-2 | SW-DC Fa0/3 |
| DC | Mail-DC | SW-DC Fa0/4 |
| DC | Printer-DC | SW-DC Fa0/5 |
| FVC | PC-FVC-1 | SW-FVC Fa0/2 |
| FVC | PC-FVC-2 | SW-FVC Fa0/3 |
| FVC | Mail-FVC | SW-FVC Fa0/4 |
| FVC | Web-FVC | SW-FVC Fa0/5 |
| FVC | DHCP-FVC | SW-FVC Fa0/6 |
| FVC | Printer-FVC | SW-FVC Fa0/7 |
| LCT | PC-LCT-1 | SW-LCT Fa0/2 |
| LCT | PC-LCT-2 | SW-LCT Fa0/3 |
| LCT | Mail-LCT | SW-LCT Fa0/4 |
| LCT | DHCP-LCT | SW-LCT Fa0/5 |
| LCT | Printer-LCT | SW-LCT Fa0/6 |
| RBA | PC-RBA-1 | SW-RBA Fa0/2 |
| RBA | PC-RBA-2 | SW-RBA Fa0/3 |
| RBA | Mail-RBA | SW-RBA Fa0/4 |
| RBA | Printer-RBA | SW-RBA Fa0/5 |
| MTY | PC-MTY-1 | SW-MTY Fa0/2 |
| MTY | PC-MTY-2 | SW-MTY Fa0/3 |
| MTY | Mail-MTY | SW-MTY Fa0/4 |
| MTY | Printer-MTY | SW-MTY Fa0/5 |
| WWM | PC-WWM-1 | SW-WWM Fa0/2 |
| WWM | PC-WWM-2 | SW-WWM Fa0/3 |
| WWM | Mail-WWM | SW-WWM Fa0/4 |
| WWM | Printer-WWM | SW-WWM Fa0/5 |

### Devices Per Unit Summary

| Unit | Router | Switch | PCs | Servers | Printer | Total |
|------|--------|--------|-----|---------|---------|-------|
| SS | R-SS | SW-SS | 2 | 3 (Mail, DNS, Web) | 1 | 7 |
| DC | R-DC | SW-DC | 2 | 1 (Mail) | 1 | 6 |
| FVC | R-FVC | SW-FVC | 2 | 3 (Mail, Web, DHCP) | 1 | 7 |
| LCT | R-LCT | SW-LCT | 2 | 2 (Mail, DHCP) | 1 | 6 |
| RBA | R-RBA | SW-RBA | 2 | 1 (Mail) | 1 | 5 |
| MTY | R-MTY | SW-MTY | 2 | 1 (Mail) | 1 | 5 |
| WWM | R-WWM | SW-WWM | 2 | 1 (Mail) | 1 | 5 |
| Hub | — | Hub-Switch | — | — | — | 1 |
| **Total** | **7** | **8** | **14** | **12** | **7** | **42** |
