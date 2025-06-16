---
layout: post
title: "Setting Up Active Directory Lab: Part 1"
date: 2025-05-20 08:10:00 -0700
categories: [Project Work, Home Lab]
tags: [HL, AD]
description: Security and Pentest Home Lab Environment
permalink: /posts/HomeLab-Active_Directory1
---

## 🧱 Active Directory Lab Overview

In this module, we’ll configure a three-VM Active Directory (AD) lab:

- **Domain Controller (DC)** – Built using **Windows Server 2019**.
- **Client VMs** – Two instances of **Windows 10 Enterprise**.

Microsoft provides **evaluation copies**:
- **Windows Server 2019** – 180-day license.
- **Windows 10 Enterprise** – 90-day license.

> `Note`  
> These evaluation versions continue to function after expiration. To reset the trial period, create **snapshots** after setup and roll back when needed.  
{: .prompt-info }

> `Tip`  
> While one client is enough for most scenarios, some AD attacks require two clients. You can skip the second client based on your use case.  
{: .prompt-tip }

---

## 📥 Downloading Windows ISO Files

### 🔹 Windows Server 2019

- Visit: [Windows Server 2019 | Microsoft Evaluation Center](https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2019?utm_source=chatgpt.com)
- Download the **64-bit ISO** (~5GB)

### 🔹 Windows 10 Enterprise

- Visit: [Windows 10 Enterprise | Microsoft Evaluation Center](https://www.microsoft.com/en-us/evalcenter/evaluate-windows-10-enterprise)

> `Reminder`  
> Microsoft names ISO files using build numbers. Rename them for clarity.  
{: .prompt-warning }

| ISO Name | OS Name |
|----------|---------|
| `17763.3650...SERVER_EVAL_x64FRE_en-us` | Windows Server 2019 |
| `19045.2006...CLIENTENTERPRISEEVAL_x64FRE_en-us` | Windows 10 Enterprise |

> `Info`  
> These builds were current as of **December 2023**. Yours may differ slightly.  
{: .prompt-info }

---

## 🛠️ Creating the VMs

### 🖥️ Windows Server 2019 (Domain Controller)

1. In VirtualBox, go to `Tools → New`.
2. Name the VM and set the **Folder** to your Home Lab directory.
3. Select the **Windows Server 2019 ISO**.
4. Check `Skip Unattended Installation` → Click `Next`.
5. Set **Memory** to `4096MB (4GB)` → Click `Next`.
6. Set **Hard Drive** to `100GB` → Click `Next`.
7. Review settings → Click `Finish`.

### 🗂️ Organizing the VM

- Right-click the VM → `Move to Group → [New]`
- Rename the group to **Active Directory**
- Right-click the group → `Move to Group → Home Lab`

---

### 🖥️ Windows 10 Enterprise (Client VM1)

1. In VirtualBox, go to `Tools → New`.
2. Name the VM and set the **Folder** to your Home Lab directory.
3. Select the **Windows 10 Enterprise ISO**.
4. Check `Skip Unattended Installation` → Click `Next`.
5. Leave **Memory** and **CPU** at default → Click `Next`.
6. Set **Hard Drive** to `100GB` → Click `Next`.
7. Review settings → Click `Finish`.

### 🗂️ Organizing the VM

- Right-click the VM → `Move to Group → Home Lab/Active Directory`

Afterwards it should look like this 

### 🖥️ Creating Windows 10 Enterprise VM2

Follow the same steps used for `Windows 10 Enterprise VM1` to create a second VM for the additional AD user.

---

## 🧩 Grouping the VMs

Organize your VMs into logical groups for easier management and configuration.

---

## ⚙️ VM Configuration
### 🖥️ Windows Server 2019

1. Select the `Windows Server 2019` VM → Click `Settings`.
2. Navigate to: `System` → `Motherboard`  
   - Boot Order:  
     - ✅ Hard Disk  
     - ✅ Optical  
     - ❌ Floppy (Disable)
3. Go to: `Network` → `Adapter 1`  
   - Attached to: `Internal Network`  
   - Name: `LAN 2`  
   - Click `OK` to save.

---

### 💻 Windows 10 Enterprise VM1

1. Select `Windows 10 Enterprise VM1` → Click `Settings`.
2. Navigate to: `System` → `Motherboard`  
   - Boot Order:  
     - ✅ Hard Disk  
     - ✅ Optical  
     - ❌ Floppy (Disable)
3. Go to: `Network` → `Adapter 1`  
   - Attached to: `Internal Network`  
   - Name: `LAN 2`  
   - Click `OK` to save.

---

### 💻 Windows 10 Enterprise VM2

Repeat the same configuration steps as above for the second AD user VM.

---

## 🧱 Windows Server 2019 Setup

### 💿 OS Installation

1. Start the `Windows Server 2019` VM.
2. Click `Next` → `Install now`.
3. Choose: `Windows Server 2019 Standalone Evaluation (Desktop Experience)` → Click `Next`.
4. Accept the license → Click `Next`.
5. Select: `Custom: Install Windows only (Advanced)`.
6. Choose `Disk 0` → Click `Next`.
7. The VM will reboot several times during installation.

---

### 🔐 Initial Setup

1. Set a password for the `Administrator` account → Click `Finish`.
2. Use `Right Ctrl + Delete` to access the login screen.
3. Log in with the password you set.

> 💡 To view VirtualBox shortcuts:  
> `File` → `Preferences` → `Input` → `Virtual Machine`

4. Close the Windows Admin Center popup.

---

## 📦 Guest Additions Installation
1. From the VM toolbar:  
   - `Devices` → `Optical Devices` → `Remove disk from virtual drive`
2. Then:  
   - `Devices` → `Insert Guest Additions CD image`
3. Open File Explorer → Click the mounted disk.
4. Run `VBoxWindowsAdditions` (4th from bottom).
5. Click `Next` → `Next` → `Next` → `Install`.
6. Choose `Reboot now` → Click `Finish`.
7. After reboot, remove the Guest Additions disk.
8. Use `Right Ctrl + F` to toggle fullscreen mode.

---

## 🌐 Network Configuration

Since DHCP is disabled on `AD_LAB`, assign a static IP:

1. Right-click the network icon → `Open Network & Internet settings`
2. Click `Change adapter options`
3. Right-click `Ethernet` → `Properties`
4. Select `Internet Protocol Version 4 (TCP/IPv4)` → `Properties`
5. Enter:

   - IP address: `10.80.80.2`  
   - Subnet mask: `255.255.255.0`  
   - Default gateway: `10.80.80.1`  
   - Preferred DNS: `10.80.80.2`

6. Click `OK` → `OK` again → Click `Yes` on the access prompt.

---

## 🖥️ Rename the Server

1. Open `Settings` → `System` → `About`
2. Click `Rename this PC` → Enter a name → Click `Next`
3. Click `Restart now` to apply changes

---

## 🧠 Install AD & DNS Roles

1. Open `Server Manager` → Click `Manage` → `Add Roles and Features`
2. Click `Next` until `Server Roles`
3. Enable:
   - `Active Directory Domain Services`
   - `DNS Server`
4. Confirm with `Add Features` → Click `Next` → `Install`
5. After installation, click `Close`

---

## 🏷️ Promote to Domain Controller

1. Click the flag icon → `Promote this server to a domain controller`
2. Select: `Add a new Forest`  
   - Domain name: `ad.lab`
3. Set a DSRM password → Click `Next`
4. Accept defaults → Click `Next` until `Install`
5. Click `Install` → VM will reboot

> 🧠 After reboot, login name will show as `AD_DOMAIN\Administrator`

---

## 🌐 DNS Forwarder Configuration

1. Open Start → `Windows Administrative Tools` → `DNS`
2. Select your server (e.g., `DC1`) → Double-click `Forwarders`
3. Click `Edit` → Add: `10.80.80.1` (pfSense IP)
4. Click `OK` → `Apply` → `OK`

---


## 📡 DHCP Installation

Since DHCP is disabled on the `AD_LAB` interface, new devices won’t receive IP addresses automatically. We’ll now enable the DHCP service on the Domain Controller to handle IP assignments for the `AD_LAB` network.

1. In `Server Manager`, click `Manage` → `Add Roles and Features`
2. Click `Next` until you reach the **Server Roles** page
3. Enable `DHCP Server` → Click `Add Features`
4. Continue clicking `Next` → On the **Confirmation** page, click `Install`

---

## ⚙️ DHCP Configuration

1. After installation, click the flag icon in the toolbar → Select `Complete DHCP configuration`
2. Click `Commit` → Then `Close`

### 🧭 Define a New Scope

1. Open `Start` → `Windows Administrative Tools` → `DHCP`
2. Expand your DHCP server (e.g., `dc1.ad.lab`)
3. Right-click `IPv4` → Select `New Scope`
4. Enter a **Name** and **Description** for the scope

#### 📋 Scope Settings

- **Start IP address**: `10.80.80.11`  
- **End IP address**: `10.80.80.253`  
- **Length**: `24`  
- **Subnet mask**: `255.255.255.0`

> 💡 You can start from `10.80.80.3` if preferred. Reserving lower IPs allows for future static assignments.

5. Leave **Exclusions** empty → Click `Next`
6. Set **Lease Duration** to `365 days` → Click `Next`

> 🧠 This ensures devices retain their IPs for a full year without renewal.

7. Select `Yes, I want to configure these options now` → Click `Next`

#### 🌐 Default Gateway

- Enter: `10.80.80.1` → Click `Add` → Then `Next`

8. Click `Next` through the DNS and WINS configuration pages  
   (We’re not using WINS in this setup)

9. Select `Yes, I want to activate this scope now` → Click `Next` → `Finish`

---

## ✅ Summary

At this point, we’ve completed the following:

- Installed **Windows Server 2019**
- Installed **Guest Additions**
- Promoted the server to a **Domain Controller**
- Configured **DNS Forwarding**
- Set up **DHCP** for the `AD_LAB` network

In the next part, we’ll:

- Create **Active Directory users**
- Join **Windows 10 clients** to the domain

👉 - [Next → Active Directory Lab Setup: Part 2](/posts/HomeLab-Active_Directory2)