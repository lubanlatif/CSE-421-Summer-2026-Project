# Ka-Chow Net — Work Distribution

**Group ID:** 1401  
**Course:** CSE421 — Computer Networks

---

## Group Members

| Student ID | Name |
|-----------|------|
| 23301038 | Luban Latif |
| 22299322 | Farhan Labib |
| 23301591 | MD Rawnak Ahasan |
| 23301634 | Kazi MD Tanvirul Hasan Sadd |

---

## Work Distribution

### Luban Latif (23301038)
- **Units Configured:** Sheriff Station (SS), Ramone Body Art (RBA)
- **Services:**
  - VLSM design and address allocation for all 7 units
  - SS router configuration (interfaces, DHCP pools for SS/RBA/DC, RIPv2 with redistribution)
  - RBA router configuration (interfaces, DHCP relay, RIPv2)
  - DNS server setup (DNS-SS) with all A records
  - Email server setup (Mail-SS) for sheriff.rs domain
  - Cross-domain email configuration (sheriff.rs ↔ flo.rs)
  - Web server setup (Web-SS) for www.sheriff.radiatorsprings.nyc

### Farhan Labib (22299322)
- **Units Configured:** Luigi Casa-Della Tires (LCT), Mater Tow Yard (MTY)
- **Services:**
  - LCT router configuration (interfaces, all static routes, floating static backup route)
  - MTY router configuration (interfaces, all static routes, DHCP relay to LCT)
  - DHCP server setup (DHCP-LCT) with pools for LCT LAN and MTY LAN
  - Email server setup (Mail-LCT for luigi.rs, Mail-MTY for mater.rs)
  - LCT floating static route failover testing and verification

### MD Rawnak Ahasan (23301591)
- **Units Configured:** Flo V8 Café (FVC), Wheel Well Motel (WWM)
- **Services:**
  - FVC router configuration (interfaces, exit-interface static routes, recursive backup route)
  - WWM router configuration (interfaces, default route, DHCP relay to FVC)
  - DHCP server setup (DHCP-FVC) with pools for FVC LAN and WWM LAN
  - Email server setup (Mail-FVC for flo.rs, Mail-WWM for wheelwell.rs)
  - Web server setup (Web-FVC) for www.flo.radiatorsprings.nyc
  - Cross-domain email configuration (flo.rs ↔ sheriff.rs)
  - FVC recursive route failover testing and verification

### Kazi MD Tanvirul Hasan Sadd (23301634)
- **Units Configured:** Doc-Hudson Clinic (DC)
- **Services:**
  - DC router configuration (interfaces, DHCP relay, RIPv2)
  - Email server setup (Mail-DC for doc.rs, Mail-RBA for ramone.rs)
  - Downtown Hub central switch setup and cabling
  - Topology diagram creation and labeling
  - End-to-end connectivity testing (ping tests across all 7 units)
  - Web access verification from all units
  - Cross-domain email exchange verification
  - Final documentation compilation and report formatting

---

## Summary Table

| Task Category | Luban | Farhan | Rawnak | Sadd |
|--------------|-------|--------|--------|------|
| VLSM Design | ✅ | | | |
| Router Config — SS | ✅ | | | |
| Router Config — LCT | | ✅ | | |
| Router Config — DC | | | | ✅ |
| Router Config — FVC | | | ✅ | |
| Router Config — RBA | ✅ | | | |
| Router Config — MTY | | ✅ | | |
| Router Config — WWM | | | ✅ | |
| DNS Server | ✅ | | | |
| Web Servers | ✅ | | ✅ | |
| Email Servers | ✅ | ✅ | ✅ | ✅ |
| DHCP Servers | ✅ (router) | ✅ (LCT srv) | ✅ (FVC srv) | |
| Routing (RIPv2) | ✅ | | | ✅ |
| Routing (Static) | | ✅ | ✅ | |
| Failover Tests | | ✅ | ✅ | |
| Full Connectivity Test | | | | ✅ |
| Topology Diagram | | | | ✅ |
| Documentation | | | | ✅ |
