# Linux Labs — Linux System Administration Project

## Scenario

This lab is about setting up and maintaining a new application server environment.

The project starts with a fresh Ubuntu Server installation and progresses milestone by milestone toward a **secure, reliable, and maintainable server environment following production-oriented practices**.

This repository documents the process end-to-end — including **decisions, troubleshooting, mistakes, fixes, configurations, and lessons learned** — as they happen, rather than presenting the work as a cleaned-up tutorial.

## Project Structure
```text
linux-labs-sysadmin/
├── README.md
├── docs/
├── journal/
├── scripts/
├── configs/
└── diagrams/
```

Each milestone has two linked files: a clean doc in `docs/` and, where relevant, 
a companion journal entry in `journal/` documenting the actual struggle — 
including mistakes, dead ends, and fixes.

## Environment

- **Host:** Windows + Oracle VirtualBox
- **VM:** Ubuntu Server 22.04.5 LTS
- **Networking:** Dual adapter — NAT (internet) + Host-Only (Windows ↔ VM access)
- **SSH Authentication:** Password-based (key-based authentication planned for Milestone 6)

Full details: [`docs/00-environment-setup.md`](docs/00-environment-setup.md)  
Setup troubleshooting log: [`journal/00-environment-setup-troubleshooting.md`](journal/00-environment-setup-troubleshooting.md)

## Progress

| # | Milestone | Status | Doc |
|---|-----------|--------|-----|
| 0 | Environment Setup | ✅ | [docs/00-environment-setup.md](docs/00-environment-setup.md) |
| 1 | First Contact — Filesystem & Navigation | ✅ | [docs/01-first-contact.md](docs/01-first-contact.md)|
| 2 | Identity & Access — Users, Groups, Permissions | ⬜ | |
| 3 | The Toolbox — Package Management & Editors | ⬜ | |
| 4 | Under the Hood — Processes, Jobs, systemd | ⬜ | |
| 5 | Eyes and Ears — Logs & Monitoring Basics | ⬜ | |
| 6 | Getting Networked — Networking & SSH Hardening | ⬜ | |
| 7 | Locking the Doors — Firewall & Basic Hardening | ⬜ | |
| 8 | Storage Expansion — Disks, Partitions, Mounting | ⬜ | |
| 9 | The Clockwork — Cron, Timers, Automation | ⬜ | |
| 10 | Archiving & Backups | ⬜ | |
| 11 | Serving the App — Nginx & Deployment | ⬜ | |
| 12 | Hardening for Production — Security & Auditing | ⬜ | |
| 13 | Performance & Troubleshooting | ⬜ | |
| 14 | Maintenance Discipline — Updates & Documentation | ⬜ | |

## Final Architecture



## Why This Project

Most Linux tutorials teach commands in isolation. This project instead simulates 
one continuous, realistic sysadmin assignment — every milestone builds on the 
last, and every mistake is documented rather than edited out, because that's 
what real system administration actually looks like.