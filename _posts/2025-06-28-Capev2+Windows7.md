---
title: Malware analysis lab setup using Capev2 and Windows 7
date: 2025-06-28 12:35:00 +0530
categories: [Blogs]
tags: [Malware Analysis]  # TAG names should always be lowercase
layout: post
image:
  path: assets/Images/malware-lab.png
  alt: Malware Analysis Lab
---


#  Complete CAPEv2 Setup Using VirtualBox (Ubuntu + Windows 7)

---

##  Overview

This guide sets up CAPEv2 on an Ubuntu VM (controller) and uses a Windows 7 VM (sandbox) in a VirtualBox environment. Both VMs run in parallel. Ubuntu has internet access; Windows 7 is isolated and communicates only via a host-only network.

---

##  VM Roles

| VM         | Role               | Internet | IP Address       | Notes                          |
|------------|--------------------|----------|------------------|--------------------------------|
| Ubuntu     | CAPEv2 Controller  | ✅ Yes   | 192.168.56.100   | NAT + Host-Only Adapter        |
| Windows 7  | Sandbox            | ❌ No    | 192.168.56.101   | Host-Only Adapter only         |

---

## 🔧 Host-Only Network Setup (on Linux Host)

Run these commands on your **Linux host** (not inside the VM):

```bash
sudo VBoxManage hostonlyif create
sudo ip addr add 192.168.56.1/24 dev vboxnet0
sudo ip link set vboxnet0 up
```

>  This sets up `vboxnet0` interface for host-only communication between VMs.

---

##  VirtualBox VM Network Configuration

### Ubuntu VM
- Adapter 1: NAT (for internet access)
- Adapter 2: Host-Only Adapter (attach to `vboxnet0`)

### Windows 7 VM
- Adapter 1: Host-Only Adapter (attach to `vboxnet0`)
- ❌ Do not attach NAT — Windows should remain isolated.

---

##  Network Configuration Inside VMs

### Ubuntu VM (Controller)

Assign static IP to Host-Only interface:
```bash
sudo ip addr add 192.168.56.100/24 dev enp0s8
sudo ip link set enp0s8 up
```
> Replace `enp0s8` with your actual interface name for host-only.

### Windows 7 VM (Sandbox)

Set static IP manually:
- IP: `192.168.56.101`
- Subnet Mask: `255.255.255.0`
- Gateway: leave blank
- DNS: leave blank

---

##  CAPEv2 Installation on Ubuntu

### 1. Clone and Run Installer

```bash
git clone https://github.com/kevoreilly/CAPEv2.git
cd CAPEv2/installer
sudo chmod +x cape2.sh
sudo ./cape2.sh base $USER | tee cape-install.log
```

### 2. Set Up Poetry Environment

```bash
cd ..
sudo pip3 install poetry
poetry install
sudo -u cape poetry run pip install -r extra/optional_dependencies.txt
```

### 3. Set Database Ownership

```bash
sudo -u postgres psql -c "ALTER DATABASE cape OWNER TO cape;"
```

---

##  CAPEv2 Configuration for Physical Mode

### Configure `cuckoo.conf`

```ini
machinery = physical
```

### Configure `physical.conf`

```ini
[win7]
label = win7
platform = windows
ip = 192.168.56.101
```

---

##  Windows 7 Sandbox Setup

1. Install Python 2.7 and add to PATH.
2. Download `agent.py` from CAPEv2 repo.
3. Run `agent.py` on boot (via Startup folder or Task Scheduler).
4. Disable Firewall, UAC, and Auto Updates.
5. Test agent connection:
```bash
curl http://192.168.56.101:8000
```
Expected output:
```json
{"pending": false, "status": "idle"}
```

---

##  Running CAPE

Start services:

```bash
sudo systemctl restart cape
sudo systemctl restart cape-web
sudo systemctl restart cape-processor
sudo systemctl restart cape-rooter
```

Submit sample:
```bash
cape submit /path/to/malware.exe --machine win7
```

Web UI:
```
http://localhost:8000
```

---

## ✅ Summary

- CAPEv2 runs on Ubuntu VM
- Windows 7 is an isolated sandbox VM
- Host-only network allows communication without internet exposure
- VMs are managed manually (no VBoxManage control)
