# Week 07 — Enterprise VPN, Firewall, and Secure Access Implementation

## Overview
This project implements a secure two-site enterprise network in GNS3, covering
IPsec Site-to-Site VPN, Zone-Based Firewall, AAA authentication with SSHv2,
OSPF routing security, and centralized monitoring — as part of the Network
Administration Internship Program at IT-Simplera Institute.

## Technologies Implemented
- **IPsec Site-to-Site VPN** — ISAKMP (AES-256, SHA-256, DH Group 14),
  IPsec transform-set (esp-aes 256, esp-sha256-hmac), crypto maps
- **Zone-Based Firewall (ZBF)** — INSIDE/OUTSIDE security zones, class-maps,
  policy-maps, stateful inspection
- **AAA Authentication + SSH v2** — local user database, RSA 2048-bit keys,
  Telnet disabled
- **OSPF Authentication** — MD5 authentication, passive-interface,
  prefix-list route filtering
- **Monitoring** — Syslog (centralized logging server), NTP (time sync),
  SNMPv3 (authenticated + encrypted monitoring)
- **Device Hardening** — CDP/HTTP disabled, password encryption, unused
  interfaces shut down

## Topology
Two sites — **HQ (R1)** and **Branch (R2)** — connected over a WAN link,
each with its own LAN switch and end-user PC. A centralized Syslog server
sits on the HQ LAN.

## Folder Structure
week-07/
├── Report.pdf # Full documentation (topology, configs, verification, troubleshooting)
├── GNS3/ # .gns3 project file and router configs
├── Screenshots/ # Verification screenshots for every feature
└── README.md
## Key Verifications
- VPN tunnel established with active encrypted packet counters
- Firewall inspection counters confirming traffic policy enforcement
- SSH login authenticated via AAA between routers
- OSPF neighbor adjacency with MD5 authentication confirmed
- Syslog messages successfully forwarded and logged

## Author
**Inshrah** — Network Administration Intern, IT-Simplera Institute
