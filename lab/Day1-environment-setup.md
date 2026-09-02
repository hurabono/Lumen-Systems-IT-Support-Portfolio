# 🏠 Active Directory Home Lab — Day 1: Environment Setup

Building a Windows Server 2025 Active Directory environment using VMware Workstation, as part of the **Lumen Systems** IT support portfolio project.

## Objectives

Set up the foundational infrastructure needed to run an Active Directory domain in a home lab environment.

By the end of this session, I completed:

- [x] VMware Workstation installation
- [x] Windows Server 2025 installation
- [x] VMware Tools installation
- [x] Server rename (DC01)
- [x] Static IP configuration
- [x] Active Directory Domain Services (AD DS) role installation
- [x] Created a new forest: `lumen.local`
- [x] Promoted the server to a Domain Controller

## Lab Environment

| Component | Value |
|---|---|
| Hypervisor | VMware Workstation |
| Server OS | Windows Server 2025 |
| Domain | lumen.local |
| Server Name | DC01 |

## Downloads

- **VMware Workstation**: https://www.vmware.com/products/desktop-hypervisor/workstation-and-fusion
- **Broadcom Download Portal** (login required): https://support.broadcom.com/group/ecx/downloads
- **Windows Server 2025 (Evaluation)**: https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2025

---

## Step 1 — Install VMware Workstation

![VMware Installation](./screenshots/day1/Screenshot%202026-08-05%20140247.png)

**What I learned:**
- VMware is a Type-2 Hypervisor — it runs on top of an existing host OS rather than directly on hardware.
- It allows multiple operating systems to run on a single physical computer.
- Each virtual machine gets its own virtual CPU, RAM, storage, and network adapter.

---

## Step 2 — Install Windows Server 2025

![VMware Installation](./screenshots/day1/Screenshot%202026-08-05%20141658.png)

**Notes:**
Instead of using Easy Install, I manually configured each setting one by one:
- CPU allocation
- Memory (RAM)
- Disk size
- Network adapter type

This helped me understand how virtual hardware is actually allocated to a VM, rather than letting the wizard decide automatically.

**Troubleshooting:** Almost downloaded a language pack that wasn't needed / could have caused issues — caught it before installing.

---

## Step 3 — Install VMware Tools

![VMware Installation](./screenshots/day1/Screenshot%202026-08-05%20145432.png)

**Why install VMware Tools?**
VMware Tools improves the VM in several ways:
- Better graphics performance
- Mouse integration between host and guest
- Shared clipboard
- Better network drivers
- Time synchronization with the host

---

## Step 4 — Rename the Server

![VMware Installation](./screenshots/day1/Screenshot%202026-08-05%20150039.png)

The server was initially named WIN-Q60M6TI1AKG. Before promoting it to a Domain Controller, I renamed it to DC01 to follow enterprise naming conventions.

**Server Name:** `DC01`

**Why rename the server?**
Meaningful, standardized server names make administration much easier in enterprise environments — you can tell what a server does just from its name.

Example naming convention:
```
DC01  → Domain Controller
FS01  → File Server
SQL01 → SQL Server
WEB01 → Web Server
```

Rebooted after the rename to apply the change.

---

## Step 5 — Configure Static IP

![VMware Installation](./screenshots/day1/Screenshot%202026-08-05%20150248.png)

**IP Configuration** (checked via `ipconfig`):
```
IP Address:
Subnet Mask:
Default Gateway:
Preferred DNS:
```

**Why does a Domain Controller need a Static IP?**

A regular computer normally asks a DHCP server for an IP address, and that address can change from day to day (e.g. `192.168.1.50` today, `192.168.1.77` tomorrow).

But a Domain Controller is the "address" that every computer on the network looks up to authenticate and find domain resources. If that address keeps changing, client PCs lose track of where the domain controller is — similar to how a delivery can't find your house if your street address changes every day.

So: **Static IP = an address that never changes**, which is essential for a Domain Controller to stay reachable.

---

## Step 6 — Install Active Directory Domain Services (AD DS)

![VMware Installation](./screenshots/day1/Screenshot%202026-08-05%20151844.png)

**Installed Role:** Active Directory Domain Services (AD DS)

**What is AD DS?**
AD DS provides centralized authentication and management for users, computers, and groups across a network.

---

## Step 7 — Create the Forest

![VMware Installation](./screenshots/day1/Screenshot%202026-08-05%20153324.png)

**Forest Name:** `lumen.local`

**Why create a forest?**
A forest is the highest-level logical container in Active Directory — it's the top of the hierarchy that domains, trees, and organizational units live inside.

---

## Step 8 — Promote to Domain Controller

![VMware Installation](./screenshots/day1/Screenshot%202026-08-05%20154124.png)

**Result:** Successfully promoted the server to a Domain Controller.

**Note:** In Server Manager, the Active Directory tools are not visible immediately after installing Windows Server. They appear only after the server has been successfully promoted to a Domain Controller. Once the promotion process is complete and the server reboots, the Active Directory management tools become available under the Tools menu.

After reboot:
- **Server Name:** DC01
- **Domain:** lumen.local

---

## What I Learned Today
- What VMware Workstation is and how virtual machines work
- How to install Windows Server 2025 with manual hardware configuration
- Why VMware Tools matters for VM performance and usability
- Why meaningful server naming conventions matter
- Why Domain Controllers require a Static IP address
- How to install Active Directory Domain Services
- How to create a new Active Directory forest
- How to promote a server to a Domain Controller

## Next Steps
- [ ] DNS Manager
- [ ] Active Directory Users and Computers
- [ ] Organizational Units (OU)
- [ ] Create Users
- [ ] Join Windows 11 client to the domain
