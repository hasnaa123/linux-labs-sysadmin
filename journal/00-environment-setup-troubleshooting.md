# 00 - Troubleshooting Environment Setup

## Problem

SSH connection from the Windows host to the Ubuntu Server VM failed with `Connection timed out` on port `22`.

## Environment

* **Host:** Windows
* **Virtualization:** VirtualBox
* **VM:** Ubuntu Server 22.04.5 LTS
* **Adapter 1:** NAT
* **Adapter 2:** Host-Only

## Troubleshooting

### 1. Check Network Interfaces

```bash
ip addr
```

`enp0s3` (NAT) was UP with a valid `10.0.2.x` address, while `enp0s8` (Host-Only) was DOWN with no IP address.

### 2. Check Netplan Configuration

```bash
ls /etc/netplan/
```

Found `50-cloud-init.yaml`.

The file only configured `enp0s3` with DHCP. `enp0s8` was not included, most likely because the Host-Only adapter did not exist when `cloud-init` generated the initial configuration.

Since `50-cloud-init.yaml` is managed by **cloud-init**, I avoided modifying it directly.

### 3. Add Custom Netplan Configuration

Created:

```text
/etc/netplan/01-netcfg.yaml
```

Configured `enp0s8` with DHCP.

Using a separate configuration file keeps the cloud-init-managed file untouched.

### 4. Fix Netplan File Permissions

Running:

```bash
sudo netplan generate
```

showed that the new configuration file permissions were too open.

The file had `644` permissions, so they were changed to `600`:

```bash
sudo chmod 600 /etc/netplan/01-netcfg.yaml
```

This is important because Netplan configuration files can contain sensitive information.

### 5. Apply and Verify

```bash
sudo netplan apply
ip addr
```

`enp0s8` was now UP with:

```text
192.168.56.104/24
```

The Ubuntu side of the Host-Only network was correctly configured.

### 6. Test Connectivity from Windows

```text
ping 192.168.56.104
```

The response came from an unexpected public IP (`81.192.249.105`) with:

```text
TTL expired in transit
```

This indicated that Windows was routing the traffic incorrectly instead of treating `192.168.56.0/24` as the local Host-Only network.

### 7. Fix the Host-Only Network

The issue was traced to the Windows/VirtualBox Host-Only network configuration.

After correcting the Host-Only adapter configuration, Windows correctly recognized the `192.168.56.0/24` network.

### 8. Verify SSH

```bash
ssh sysadmin@192.168.56.104
```

SSH connection succeeded.

![Successful SSH connection][(assets/ssh-success.png)]

## Key Concepts

* VirtualBox **NAT vs Host-Only** networking
* Linux network interfaces
* **Netplan** configuration
* **cloud-init** generated configuration
* Netplan configuration layering
* Linux file permissions (`644` vs `600`)
* IP addressing and subnets
* Routing troubleshooting
* ICMP vs TCP connectivity
* SSH on port `22`
* `TTL expired in transit` as a routing diagnostic clue

## Main Lesson

Network troubleshooting should be performed step by step:

**Interface → IP Configuration → Routing → Connectivity → Service**

