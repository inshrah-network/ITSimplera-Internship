# Enterprise Services Infrastructure Deployment
### DHCP, DNS, HTTP/HTTPS, FTP, TFTP, NTP, Syslog & SNMP | Week 5 — Network Administration Internship

**Institute:** IT-Simplera Institute
**Intern:** Inshrah (NETB01-0022)
**Supervisor:** Jawad Qayum, Senior Network Administrator
**Tools:** Cisco Packet Tracer, Command Prompt, Web Browser

---

## 📌 Project Overview

This project implements a centralized enterprise services infrastructure in Cisco Packet Tracer that integrates **eight core network services** — DHCP, DNS, HTTP/HTTPS, FTP, TFTP, NTP, Syslog, and SNMP — across a multi-department network (HR and Finance). The goal was to design, configure, verify, and troubleshoot a unified enterprise environment reflecting real-world organizational requirements.

---

## 🗺️ Network Design

| Device | Role | Interface | IP Address |
|---|---|---|---|
| Router0 (Cisco 2911) | Core router | Gig0/0 → Server0 | 192.168.100.1 |
| Router0 | Core router | Gig0/1 → Switch0 (HR) | 192.168.10.1 |
| Router0 | Core router | Gig0/2 → Switch1 (Finance) | 192.168.20.1 |
| Server0 (Server-PT) | Enterprise services host | Fa0 | 192.168.100.10 |
| PC1–PC4 | HR department clients | Fa0 | DHCP — 192.168.10.0/24 |
| PC5–PC8 | Finance department clients | Fa0 | DHCP — 192.168.20.0/24 |

DHCP broadcasts from HR and Finance are relayed to the centralized server using `ip helper-address` configured on both LAN-facing router interfaces.

---

## ⚙️ Services Implemented

| Service | Function | Verification Method |
|---|---|---|
| **DHCP** | Automatic IP addressing for HR & Finance | Client IP Configuration (auto-assigned) |
| **DNS** | Resolves `www.itsimplera.local` → server IP | Browser access + successful ping by hostname |
| **HTTP/HTTPS** | Hosts the enterprise web portal | Portal loads via IP and hostname |
| **FTP** | Authenticated file transfer | `ftp` login + `dir` listing from client CLI |
| **TFTP** | Router configuration backup | `copy running-config tftp` + file confirmed on server |
| **NTP** | Clock synchronization | `show ntp status` — synchronized to 192.168.100.10 |
| **Syslog** | Centralized event logging | Log messages received on server after interface flap test |
| **SNMP** | Read-only/read-write device monitoring | Community strings confirmed via `show running-config` |

---

## 🔍 Connectivity Testing

- ✅ Same-department ping (HR ↔ HR)
- ✅ Gateway reachability (client → router)
- ✅ Cross-department ping (HR ↔ Finance)
- ✅ Server reachability (192.168.100.10)
- ✅ DNS name resolution + ping (`www.itsimplera.local`)
- ✅ Traceroute to server (2 hops: gateway → server)

---

## 🛠️ Troubleshooting Highlights

- `ip helper-address` was required on router LAN interfaces since the DHCP server sits on a separate subnet from the clients.
- `snmp-server location` / `snmp-server contact` commands were unsupported on this router's IOS image — skipped without affecting core SNMP functionality.
- `show snmp` returns blank output in Packet Tracer since no active SNMP manager generates polling traffic; verification was instead performed at the configuration level via `show running-config`.
- NTP required a short polling interval before `show ntp status` reported a synchronized clock.

---

## 📁 Folder Contents

```
Week5/
├── Report.pdf                # Full documentation with concepts, configs & verification
├── PacketTracer_Project.pkt
├── Screenshots/               # Evidence for each configured service
└── README.md
```

---

## 🎯 Key Learning

This deployment reinforced how enterprise services depend on one another — DHCP relay across subnets, centralized logging via Syslog, and time synchronization via NTP all rely on correct reachability to the central server. It also clarified the difference between configuration-level and live-traffic verification, an important distinction when working without a dedicated NMS tool.
