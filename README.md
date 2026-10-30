# 🔐 Cybersecurity Lab Environment Setup

> Building an isolated virtual lab for penetration testing and ethical hacking practice

---

## 📌 Project Overview

This project sets up a virtual cybersecurity and penetration-testing laboratory using **VirtualBox** and **Kali Linux**.

The lab provides a controlled environment where cybersecurity tools, network scanning, reconnaissance, vulnerability assessment, and other security-testing activities can be performed safely and repeatedly.

It is configured on a private virtual network so that additional machines can be added later and used as targets for authorized security testing.

## 🎯 Objectives

1. Install and configure VirtualBox.
2. Install/import Kali Linux as a virtual machine.
3. Create a private NAT Network for the lab.
4. Configure network connectivity for Kali Linux.
5. Assign a consistent IP address to the Kali VM.
6. Verify network connectivity and DNS resolution.
7. Take a clean VM snapshot for recovery.
8. Document the complete setup process.
9. Prepare the environment for future cybersecurity projects.

## 🛡️ Purpose of the Lab

The lab is an isolated, controlled environment for cybersecurity learning and authorized security testing. It can be used for:

- Network reconnaissance
- Port scanning
- Vulnerability assessment
- Packet analysis
- Web security testing
- Exploitation practice
- Security-tool experimentation

> ⚠️ **Important:** This laboratory must only be used on systems that you own or have explicit permission to test. Do not use the lab or its tools to attack unauthorized systems.

## 🏗️ Lab Architecture

```
                 ┌──────────────────────────┐
                 │  Host: Windows 10        │
                 │  VirtualBox 7.2          │
                 └────────────┬─────────────┘
                              │
                 ┌────────────┴─────────────┐
                 │  NatNetwork 10.0.0.0/24  │
                 │  Gateway: 10.0.0.1       │
                 └────────────┬─────────────┘
                              │
          ┌───────────────────┼───────────────────┐
   ┌──────┴───────┐    ┌──────┴───────┐    ┌──────┴───────┐
   │ Kali Linux   │    │ Target VM    │    │ Target VM    │
   │ 10.0.0.2     │    │ (future)     │    │ (future)     │
   └──────────────┘    └──────────────┘    └──────────────┘
```

Additional target machines can be added to the same virtual network in future projects.

## ⚙️ Lab Configuration

| Component | Configuration |
|---|---|
| 🖥️ Host OS | Windows 10 |
| 🧠 Host RAM | 8 GB |
| ⚡ Processor | Intel Core i7 |
| 🧰 Hypervisor | VirtualBox 7.2 |
| 🐉 Security OS | Kali Linux 2026.2 |
| 🧠 Kali RAM | 2048 MB |
| 🌐 Virtual Network | NAT Network |
| 📡 Network Address | 10.0.0.0/24 |
| 🐧 Kali IP Address | 10.0.0.2/24 |
| 🚪 Default Gateway | 10.0.0.1 |
| 🌍 DNS Server | 8.8.8.8 |
| 🔮 Future VM Range | 10.0.0.3 – 10.0.0.99 |

## 🪜 Lab Setup Procedure

### Step 1. Install 7-Zip
7-Zip is used to extract the Kali Linux virtual-machine package, which may be distributed as a `.7z` archive.

### Step 2. Install VirtualBox
VirtualBox is installed as the hypervisor.

### Step 3. Create the NAT Network
A dedicated NAT Network is created in VirtualBox:

| Setting | Value |
|---|---|
| Network Name | NatNetwork |
| IPv4 Prefix | 10.0.0.0/24 |
| DHCP | Enabled |
| IPv6 | Disabled |

A NAT Network is used because multiple VMs attached to it can talk to each other while still having outbound internet access. This lets future attacker and target VMs communicate within the lab.

### Step 4. Import Kali Linux
The Kali Linux VM is downloaded from the official Kali website and imported into VirtualBox.

**Network adapter (Adapter 1):**

```
Attached to:  NAT Network
Network:      NatNetwork
Adapter Type: Intel PRO/1000 MT Desktop
```

**Resources:**

```
RAM: 2048 MB
```

A shared folder is also configured to transfer files between the host and the Kali VM.

### Step 5. Configure the Kali Linux Network
Configure a consistent IPv4 address:

```
IP Address:  10.0.0.2
Subnet Mask: 255.255.255.0
Gateway:     10.0.0.1
DNS:         8.8.8.8
```

A fixed IP makes the lab easier to document and reference in future exercises.

### Step 6. Create a Clean VM Snapshot
After the initial configuration, create a VirtualBox snapshot, for example:

```
Clean Kali - Network Setup
```

This is the clean baseline of the lab. If a later exercise breaks the VM, you can restore it to this state.

## 🔎 Lab Verification

| Test | Command | Expected Result |
|---|---|---|
| 🌐 Check IP address | `ip a` | Correct Kali IP displayed |
| 📡 Test gateway | `ping 10.0.0.1` | Successful replies |
| 🌍 Test internet connectivity | `ping 8.8.8.8` | Successful replies |
| 🔎 Test DNS resolution | `nslookup networkwalks.com` | Domain resolves |
| 🧰 Verify Nmap | `nmap --version` | Nmap version displayed |
| 🔄 Verify snapshot | Restore snapshot, run `ip a` | Baseline configuration restored |

**Example results**

```
IP Address: 10.0.0.2/24
Gateway:    10.0.0.1
DNS:        8.8.8.8
```

## 🐞 Problems Encountered & Solutions

### Problem 1. Internet connectivity after static IP configuration
After manually setting the IPv4 details, internet access may fail depending on the Kali/NetworkManager configuration.

Workaround used in this lab:

```bash
sudo nmcli connection modify "Wired connection 1" ipv4.dad-timeout 0
```

Then restart the connection (or reboot) and test connectivity again.

> **Note:** Connection and interface names differ between systems. Identify your actual connection name first (e.g. with `nmcli connection show`) before running the command.

### Problem 2. VirtualBox VT-x / virtualization error
The VM initially failed to start because hardware virtualization was disabled in the BIOS/UEFI.

**Fix:**

1. Restart the computer.
2. Enter BIOS/UEFI settings.
3. Enable Intel VT-x / hardware virtualization.
4. Save the configuration.
5. Restart the computer.
6. Start the Kali VM again.

## 💡 What I Learned

1. **NAT vs NAT Network** – A standard NAT isolates each VM, while a NAT Network lets multiple VMs on the same virtual network communicate with each other and still reach the outside world. This makes it ideal for a multi-machine lab.
2. **Virtual machine networking** – How VirtualBox network adapters connect VMs to different network types, and how that affects communication.
3. **Static IP configuration** – Configuring and verifying IPv4 address, subnet mask, gateway, and DNS in Kali Linux.
4. **VM snapshots** – Always create a clean snapshot before risky or experimental work, so there is a known-good recovery point.
5. **Documentation** – Recording commands, configuration, screenshots, problems, and solutions is a core part of professional cybersecurity work.

## 🔐 Security & Ethical Use

This laboratory is intended **strictly for educational purposes**. Only test systems you own or have explicit written permission to test.

## 🔗 Tools & Resources

- [7-Zip](https://7-zip.org/download.html)
- [VirtualBox](https://virtualbox.org/wiki/Downloads)
- [Kali Linux](https://kali.org/get-kali)

## 👤 Author

**Jenil Patel**

## 📌 Project Information

| | |
|---|---|
| **Program** | Cybersecurity at Networkwalks |
| **Week** | 01 |
| **Project** | Cybersecurity & Pentesting Lab Setup |
| **Repository** | GitHub |
