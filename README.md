# Networkwalks-B082-week1-Cybersecurity-lab-setup
Networkwalks B082 – Week 1, Week 1 covers the basics of setting up a cybersecurity lab, including virtual machine configuration, network topology, and installation of the required security tools. This repository documents the exact setup, tools, and step-by-step process used to build the lab environment. 🔐💻
---

# 🔐 Cybersecurity Lab Environment Setup
**Spinning up an isolated virtual workspace for penetration testing and ethical hacking**

---

## 📌 What This Project Is About

This project walks through the complete setup of a personal cybersecurity and penetration-testing lab built on VirtualBox and Kali Linux.

The goal is a self-contained, repeatable environment where security tools, network scanning, reconnaissance, vulnerability assessment, and hands-on attack simulation can be practiced safely — without touching real-world infrastructure.

The lab runs on a private virtual network, designed to scale as new target machines are added down the line.

---

## 🎯 What This Project Sets Out to Do

- Deploy and configure VirtualBox as the hypervisor
- Set up Kali Linux as the primary attack machine
- Build a dedicated NAT Network to isolate the lab
- Establish reliable network connectivity for the Kali VM
- Lock in a fixed IP address for consistent referencing
- Validate network stack — connectivity, gateway, and DNS
- Capture a baseline VM snapshot before any experiments begin
- Maintain thorough documentation throughout
- Leave the environment ready for follow-on security projects

---

## 🛡️ Why This Lab Exists

This workspace gives a controlled, sandboxed space for cybersecurity study and authorized security testing.

Activities it supports:

- Network reconnaissance
- Port scanning
- Vulnerability assessment
- Packet capture and analysis
- Web application security testing
- Exploitation practice
- Security tool evaluation and experimentation

> ⚠️ **Strict rule:** This lab and everything in it is for use only against systems you own or have written authorization to test. Unauthorized use is illegal and unethical.

---

## 🏗️ Lab Layout

Target machines can be introduced to the same virtual network in future phases of this project.

---

## ⚙️ Environment Specifications

| 🧩 Element | ⚙️ Details |
|---|---|
| 🖥️ Host OS | Windows 10 |
| 🧠 Host RAM | 8 GB |
| ⚡ CPU | Intel Core i7 |
| 🧰 Hypervisor | VirtualBox 7.2 |
| 🐉 Attack OS | Kali Linux 2026.2 |
| 🧠 Kali RAM | 2048 MB |
| 🌐 Network Type | NAT Network |
| 📡 Network Range | 10.0.0.0/24 |
| 🐧 Kali IP | 10.0.0.2/24 |
| 🚪 Gateway | 10.0.0.1 |
| 🌍 DNS | 8.8.8.8 |
| 🔮 Target VM Range | 10.0.0.3–10.0.0.99 |

---

## 🪜 Build Procedure

**Step 1 — Install 7-Zip**
Used to unpack the Kali Linux VM archive, which ships as a `.7z` compressed file.
Tool: 7-Zip

**Step 2 — Install VirtualBox**
The core hypervisor that hosts and manages all virtual machines in this lab.

**Step 3 — Build the NAT Network**
A dedicated NAT Network was created inside VirtualBox with the following settings:
- Network Name: `NatNetwork`
- IPv4 Prefix: `10.0.0.0/24`
- DHCP: Enabled
- IPv6: Disabled

A NAT Network was chosen because it allows VMs on the same virtual network to communicate with each other while still reaching the internet outbound — exactly what a multi-machine lab requires.

**Step 4 — Bring In Kali Linux**
The Kali Linux VM was pulled from the official site and imported into VirtualBox.

Network adapter settings applied:
```
Adapter 1
Attached to:  NAT Network
Network:      NatNetwork
Adapter Type: Intel PRO/1000 MT Desktop
```
Resources allocated: **2048 MB RAM**
A shared folder was also set up for easy file transfer between host and VM.

**Step 5 — Network Configuration on Kali**
The VM's network stack was manually configured with a fixed IPv4 address for stability:
```
IP Address:   10.0.0.2
Subnet Mask:  255.255.255.0
Gateway:      10.0.0.1
DNS:          8.8.8.8
```
A fixed address makes lab documentation cleaner and future referencing straightforward.

**Step 6 — Snapshot the Clean State**
Once setup was complete, a VirtualBox snapshot was taken:
```
Snapshot name: Clean Kali - Network Setup
```
This is the lab's known-good baseline. Any future exercise that corrupts or alters the VM can be rolled back to this point instantly.

---

## 🔎 Verification Checklist

| ✅ Check | 🧾 Command | 🎯 Expected Outcome |
|---|---|---|
| 🌐 Confirm IP assignment   | `ip a`                       | Correct Kali IP visible  |
| 📡 Reach the gateway       | `ping 10.0.0.1`              | Replies received         |
| 🌍 Reach the internet      | `ping 8.8.8.8`               | Replies received         |
| 🔎 Confirm DNS works       | `nslookup networkwalks.com`  | Domain resolves correctly|
| 🧰 Confirm Nmap is present | `nmap --version`             | Version string printed   |
| 🔄 Validate snapshot       | Restore → run `ip a`         | Baseline config restored |

**Confirmed values post-setup:**
- IP: `10.0.0.2/24`
- Gateway: `10.0.0.1`
- DNS: `8.8.8.8`

---

## 🐞 Issues Hit & How They Were Resolved

**Issue 1 — Lost Internet After Assigning Static IP**

Manually setting the IPv4 address broke outbound internet access due to how NetworkManager handled the new configuration.

Fix applied:
```bash
sudo nmcli connection modify "Wired connection 1" ipv4.dad-timeout 0
```
The connection was then restarted and internet access was verified.

> **Note:** Interface and connection names vary by machine. Always check your actual connection name before running `nmcli` commands.

**Issue 2 — VM Refused to Launch (VT-x Error)**

The VM failed to boot because hardware virtualization was turned off in the system BIOS.

Resolution steps:
1. Restart the machine
2. Enter BIOS/UEFI
3. Locate and enable Intel VT-x (hardware virtualization)
4. Save and exit
5. Restart the machine
6. Launch the Kali VM

The VM booted cleanly after this change.

---

## 💡 Core Takeaways

**NAT vs NAT Network** — These are not the same thing. A standard NAT gives a VM internet access but keeps it isolated from other VMs. A NAT Network connects multiple VMs to each other *and* gives them outbound internet — the right choice for a lab with multiple machines.

**VM Networking Fundamentals** — How virtual adapters work, how they bind to different network types, and how that determines what a VM can and can't reach.

**Static IP Assignment** — How to manually set and verify IPv4 address, subnet mask, gateway, and DNS inside Kali Linux.

**Snapshot Strategy** — Always snapshot a clean, working state before running anything experimental. One restore operation beats rebuilding from scratch.

**Documentation Discipline** — Recording every command, configuration change, problem, and fix is a professional habit that pays off immediately and compounds over time.

---

## 🔐 Ethical Use Statement

This lab exists strictly for education and authorized testing. It is not a tool for attacking systems without permission.

---

## 🔗 Resources

- **7-Zip:** https://7-zip.org/download.html
- **VirtualBox:** https://virtualbox.org/wiki/Downloads
- **Kali Linux:** https://kali.org/get-kali

---

## 👤 Author

** Nason Kasumpa ** — Cybersecurity student B082
LinkedIn: https://www.linkedin.com/in/nason-kasumpa-0b8a4942a/

---

## 📌 Project Details

Program: Cybersecurity at Networkwalks  
Week: 01  
Project: Cybersecurity & Pentesting Lab Setup | Repository: GitHub
