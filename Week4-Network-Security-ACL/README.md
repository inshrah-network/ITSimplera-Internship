# Week 4 – Enterprise Network Security and Access Control

**Network Administration Internship Program – IT-Simplera Institute**

| | |
|---|---|
| **Intern** | Inshrah Saeed |
| **Registration No.** | NETB01-0022 |
| **Supervisor** | Jawad Qayum (Senior Network Administrator) |
| **Tools** | GNS3, PuTTY, Cisco IOS (c7200 router, IOSvL2 switches) |
| **Submission Date** | 25 July 2026 |

## Overview

This project is the Week 4 deliverable of the Network Administration Internship Program. It builds on the multi-department network designed in Week 3 and focuses on **securing** it — controlling exactly which traffic is allowed between departments, locking down remote management, and hardening the switches against common Layer 2 mistakes and attacks.

## Objectives

- Configure and verify **Standard, Extended, and Named ACLs** to enforce a department-by-department security policy.
- Set up **SSH** for secure remote router management, restricted to the IT department only.
- Configure **Port Security**, **DHCP Snooping**, **BPDU Guard**, **Root Guard**, and **PortFast** to protect the switching layer.
- Test every feature end-to-end and document any issues found and fixed along the way.

## Topology

One router (**R1**) with VLAN sub-interfaces, connected to two access switches:

| Device | Role | VLAN(s) |
|---|---|---|
| R1 | Router — inter-VLAN routing, SSH server, ACL enforcement | 10, 20, 30 |
| SW1 | Access switch — HR & Finance | 10 (HR), 20 (Finance) |
| SW2 | Access switch — IT | 30 (IT) |
| PC1 / PC2 | HR / Finance hosts | 10 / 20 |
| PC3 / PC4 | IT hosts | 30 |

See `topology/` for the full labelled diagram.

## Security Features Implemented

| Feature | Purpose |
|---|---|
| Standard ACL | Blocks HR from reaching Finance entirely |
| Extended (Named) ACL | Allows Finance ping-only access to IT; blocks HR from IT completely |
| Named ACL (SSH) | Restricts router SSH access to the IT subnet only |
| SSH | Encrypted remote management, replacing Telnet |
| Port Security | Locks each access port to one known MAC address (sticky) |
| DHCP Snooping | Blocks rogue DHCP server replies on untrusted ports |
| PortFast + BPDU Guard | Fast port startup for end hosts, with loop protection |
| Root Guard | Prevents an unintended switch from becoming the STP root bridge |

## Repository Structure

```
Week4-Network-Security-ACL/
├── README.md
├── Week4_Report.pdf
├── Week4_Report.docx
├── configs/
│   ├── R1_running-config.txt
│   ├── SW1_running-config.txt
│   └── SW2_running-config.txt
├── screenshots/
│   └── (numbered verification screenshots — see report for index)
└── topology/
    └── topology_diagram.png
```

## Troubleshooting Highlights

Full details are in the report's Troubleshooting section; the main issues diagnosed and resolved were:

1. **GNS3 virtual link desynchronization** between devices after a long session — fixed by reloading the project.
2. **Duplex mismatch** (router fixed at half-duplex vs. switches on auto) — fixed by setting `duplex full` explicitly.
3. **SSH cipher mismatch** between an older IOS image (AES-CBC only) and a newer SSH client (AES-CTR only) — fixed by enabling CBC ciphers on the client side.
4. **ACL logic gap** — a catch-all `permit ip any any` rule was unintentionally letting HR traffic reach IT; fixed with an explicit deny rule.
5. **Root Guard MAC tie-breaker** — a blocked port stayed inconsistent after reverting a simulated attack, because of a naturally lower MAC address on the neighboring switch; fixed by permanently lowering the intended root switch's priority.

## Full Report

See `Week4_Report.pdf` (or `Week4_Report.docx`) in this folder for the complete write-up, including all configuration steps, verification screenshots, and connectivity test results.
