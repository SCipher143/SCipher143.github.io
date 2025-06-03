---
layout: post
title: "Setting Up pfSense: A Step-by-Step Guide"
date: 2025-04-26 10:19:00 -0700
categories: [Project Work, Home Lab]
tags: [HL]
description: Security and Pentest Home Lab Environment
permalink: /posts/HomeLab-Installing-pfSense
---

# 🧱 Part 2 – pfSense Setup & Configuration

In this module, we’ll install and configure `pfSense`, the open-source firewall that segments and secures your home lab network.

Go to the official download page:  
[pfSense CE Download](https://atxfiles.netgate.com/mirror/downloads/)

![Desktop View](/assets/img/HomeLab/HL-6.png){: width="700" height="400" }

The downloaded file will have the extension `.iso.gz`. Use a tool like `7-Zip` to extract the `.iso` image.

---

## 🖥️ Create pfSense VM

1. Open **VirtualBox** → Click `Tools` → `New`
2. Set:
   - **Name:** `pfSense`
   - **Type:** `BSD`
   - **Version:** `FreeBSD (64-bit)`
   - Choose the extracted `.iso` as the ISO image
3. Click `Next`

![Desktop View](/assets/img/HomeLab/HL-7.png){: width="700" height="400" }

4. Accept the default memory/CPU settings → Click `Next`

![Desktop View](/assets/img/HomeLab/HL-9.png){: width="700" height="400" }

5. Set disk size to `20GB` → Click `Next`

![Desktop View](/assets/img/HomeLab/HL-10.png){: width="700" height="400" }

6. Review → Click `Finish`

---

## 🗂️ Organize VM into Group

1. Right-click pfSense VM → `Move to Group` → `[New]`
2. Right-click `New Group` → Rename to `Firewall`

![Desktop View](/assets/img/HomeLab/HL-12.png){: width="700" height="400" }

---

## ⚙️ Configure pfSense VM Settings

Select pfSense VM → Click `Settings`

### 🧠 System

- `System → Motherboard`  
  - Boot Order: `Hard Disk`, then `Optical`  
  - Uncheck `Floppy`

![Desktop View](/assets/img/HomeLab/HL-14.png){: width="700" height="400" }

### 🔇 Disable Unused Hardware

- `Audio`: Uncheck `Enable Audio`  
- `USB`: Uncheck `Enable USB Controller`

![Desktop View](/assets/img/HomeLab/HL-16.png){: width="700" height="400" }

### 🌐 Network Adapters

Configure 4 adapters:

- **Adapter 1 (NAT):**  
  - Attached to: `NAT`  
  - Type: `Paravirtualized Network (virtio-net)`

![Desktop View](/assets/img/HomeLab/HL-17.png){: width="700" height="400" }

- **Adapter 2 (LAN 0):**  
  - Attached to: `Internal Network`  
  - Name: `LAN 0`

- **Adapter 3 (LAN 1):**  
  - Attached to: `Internal Network`  
  - Name: `LAN 1`

- **Adapter 4 (LAN 2):**  
  - Attached to: `Internal Network`  
  - Name: `LAN 2`

![Desktop View](/assets/img/HomeLab/HL-20.png){: width="700" height="400" }

> **Note:** VirtualBox only supports 4 adapters via UI. We’ll add more using CLI later.  
{: .prompt-info }

---

## 🔧 Install pfSense

1. Select pfSense VM → Click `Start`
2. Wait for the license screen → Press `Enter`
3. Begin install → `Auto (ZFS)` → `Proceed with Installation`

![Desktop View](/assets/img/HomeLab/HL-24.png){: width="700" height="400" }

4. Choose: `Stripe - No Redundancy`
5. Select disk (`ada0`) → `Spacebar` → `Enter`
6. Confirm → Highlight `YES` → `Enter`

![Desktop View](/assets/img/HomeLab/HL-28.png){: width="700" height="400" }

7. Reboot after install completes

![Desktop View](/assets/img/HomeLab/HL-30.png){: width="700" height="400" }

---

## 🔌 Assign Interfaces

When prompted about VLANs → type `n`

Assign:

- WAN: `vtnet0`  
- LAN: `vtnet1`  
- OPT1: `vtnet2`  
- OPT2: `vtnet3`

Type `y` to confirm

![Desktop View](/assets/img/HomeLab/HL-32.png){: width="700" height="400" }

> The WAN IP will be different on your system (set by VirtualBox DHCP).  
{: .prompt-info }

---

## 🛠️ Configure Interfaces

### 🔐 LAN (vtnet1)

- Press `2` → then `2` again
- Use static IP: `10.0.0.1/24`
- Enable DHCP:
  - Start: `10.0.0.11`
  - End: `10.0.0.243`
- Decline HTTP switch: `n`

![Desktop View](/assets/img/HomeLab/HL-35.png){: width="700" height="400" }

### ⚔️ OPT1 (vtnet2)

- IP: `10.6.6.1/24`
- DHCP Range: `10.6.6.11` – `10.6.6.243`
- Decline HTTP switch: `n`

![Desktop View](/assets/img/HomeLab/HL-39.png){: width="700" height="400" }

### 🧬 OPT2 (vtnet3)

- IP: `10.80.80.1/24`
- **DHCP: Disabled**  
  (AD Domain Controller will handle DHCP)

![Desktop View](/assets/img/HomeLab/HL-41.png){: width="700" height="400" }

> Final Interface IPs:  
> - LAN: `10.0.0.1`  
> - OPT1: `10.6.6.1`  
> - OPT2: `10.80.80.1`  
{: .prompt-tip }

---

## ⏹️ Shutdown pfSense

When you're done:

```text
Enter an option: 6
Do you want to proceed?: y
```

This halts the system safely.

![Desktop View](/assets/img/HomeLab/HL-43.png){: width="700" height="400" }

---

## 🧽 Post-Install Cleanup

1. Go to `Settings → Storage`
2. Select `.iso` image → Click the disk icon → `Remove Disk from Virtual Drive`

![Desktop View](/assets/img/HomeLab/HL-34.png){: width="700" height="400" }

> You can delete the `.iso` and `.iso.gz` files if you don’t plan to reuse them.  
{: .prompt-info }

---

## 🔜 What’s Next?

We’ll now set up **Kali Linux** on the `LAN` interface. This VM will be used to:

- Access the pfSense Web UI  
- Configure network rules  
- Launch attacks on the `CYBER_RANGE`  

👉 [Next → Installing Kali Linux for Your Lab](/posts/HomeLab-Installing-Kali_Linux)