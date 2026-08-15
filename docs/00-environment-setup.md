# 00 - Environment Setup

## Host Machine
- OS: Windows
- Virtualization: Oracle VirtualBox

## VM Specifications
- Ubuntu Server 22.04.5 LTS
- RAM: 4GB
- CPU cores: 1
- Disk: VDI, dynamically allocated, 10 GB

## Why Ubuntu 22.04 LTS
It offers five years of standard security and maintenance updates, a stable software environment, and a reliable platform for both desktop and enterprise server deployments

## Networking
Dual-adapter configuration:
- **Adapter 1 — NAT**: provides outbound internet access (package installs, updates)
- **Adapter 2 — Host-Only**: provides direct SSH access from the Windows host,
  isolated from the wider network/internet

| Interface | Adapter Type | IP Address       |
|-----------|-------------|-------------------|
| enp0s3    | NAT         | 10.0.2.15         |
| enp0s8    | Host-Only   | 192.168.56.104    |

Note: enp0s8 required a manual netplan configuration since it wasn't
present at install time. See `journal/00-environment-setup-troubleshooting.md`
for the full debugging process.

## Authentication
- Password-based SSH (chosen deliberately for this learning phase —
  see reasoning in project README/notes)
- SSH key-based hardening planned for Milestone 6

## Access
```
ssh sysadmin@192.168.56.104
```