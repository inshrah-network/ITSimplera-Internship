# Enterprise Multi-Protocol Routing Challenge
### OSPF Multi-Area, Static Routing & RIP v1/v2 | Week 3 — Network Administration Internship

**Institute:** IT-Simplera Institute
**Intern:** Inshrah (NETB01-0022)
**Supervisor:** Jawad Qayum, Senior Network Administrator
**Tools:** GNS3, Cisco IOS (c3745), PuTTY

---

## 📌 Project Overview

This project implements a medium-sized enterprise network in GNS3 that integrates **three routing technologies** — OSPF Multi-Area Routing, RIP v1/v2, and Static Routing — into a single, fully redistributed topology. The goal was to design a scalable, redundant network and demonstrate real-world enterprise routing practices: VLSM addressing, route redistribution, floating static routes, and systematic troubleshooting.

## 🗺️ Network Design

| Router | Routing Domain | Role | LAN |
|--------|----------------|------|-----|
| R1 | OSPF Area 1 | Access router | HR-LAN |
| R2 | OSPF Area 0 | Backbone router | IT-LAN |
| R3 | OSPF Area 0/1 | ABR + Redistribution Point (OSPF ↔ RIP) | Finance-LAN |
| R4 | RIP | RIPv2 router | Sales-LAN |
| R5 | RIP | RIPv2 / RIPv1 boundary | Ops-LAN |
| R6 | RIP / Static | Redistribution Point (RIP ↔ Static) | Store-LAN |

Two backup links (R1–R3 and R4–R6) provide network redundancy, with a floating static route on R6 (AD 130) serving as backup for the Ops-LAN subnet.

## 🌐 IP Addressing (VLSM)

Base network: `192.168.10.0/24`, subnetted using VLSM to match host requirements per LAN, plus `/30` links for all point-to-point connections. Full addressing table is in [`Report.pdf`](./Report.pdf).

## ⚙️ Key Configurations

- **OSPF Multi-Area:** Area 0 backbone (R2, R3) with Area 1 (R1); R3 configured as ABR
- **RIP v2:** R3–R4 and R4–R5 links
- **RIP v1:** R5–R6 link, explicitly configured with `ip rip send/receive version 1` to demonstrate classful vs. VLSM-aware behavior
- **Static Routing:** Default route + floating static route (AD 130) on R6
- **Redistribution:** Two-way redistribution at R3 (OSPF ↔ RIP) and R6 (RIP ↔ Static)

## ✅ Verification

- OSPF neighbor adjacencies reached **FULL** state across Area 0 and Area 1
- RIP database and `show ip protocols` confirmed correct v1/v2 operation per interface
- Full routing table cross-checks confirmed successful redistribution in both directions
- End-to-end **ping and traceroute** from PC1 (OSPF Area 1) to PC11 (Static zone) succeeded across all six routers

## 🔧 Troubleshooting Highlights

- Diagnosed and corrected a scenario where backup links were being preferred over primary paths, using OSPF cost and RIP offset-list tuning
- Identified a real RIPv1 limitation — inability to carry VLSM subnet masks — causing certain subnets to be dropped on the R5–R6 link, resolved with a summarized static route
- Resolved interface/module mismatches during router configuration by verifying available interfaces before applying commands

## 📁 Repository Structure

```
Week3/
├── Report.pdf              # Full project documentation (10+ pages)
├── GNS3-Project/           # Complete GNS3 topology and router configs
├── Screenshots/            # Verification and connectivity test evidence
└── README.md
```

## 📚 Learning Outcomes

This project reinforced core enterprise networking concepts: VLSM subnetting, OSPF area design, administrative distance, floating static routes, and route redistribution — along with practical, hands-on troubleshooting skills using systematic diagnostic techniques (`debug ip rip`, routing table analysis, and traceroute path verification).

---

*Part of the Network Administration Internship Program at IT-Simplera Institute.*
