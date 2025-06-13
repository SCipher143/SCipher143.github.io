---
layout: post
title: "Setting Up Active Directory Lab: Part 2"
date: 2025-05-21 08:15:00 -0700
categories: [Project Work, Home Lab]
tags: [HL, AD]
description: Security and Pentest Home Lab Environment
permalink: /posts/HomeLab-Active_Directory2
---

## 🛠️ Active Directory Lab Setup - Part 2

In the previous module, we installed `Windows Server 2019`, configured `Active Directory Domain Services`, set up `DNS Forwarding`, and enabled `DHCP`. In this module, we’ll complete the Domain Controller setup and begin integrating devices into the AD environment.

---

## 🏛️ Certificate Services Installation

### 🔐 Install AD Certificate Services

1. Open `Server Manager` → Click `Manage` → `Add Roles and Features`
2. Click `Next` until the **Server Roles** page
3. Enable `Active Directory Certificate Services` → Click `Add Features`
4. Click `Next` until the **Role Services** page
5. Enable `Certificate Authority` → Click `Next`
6. Click `Install` to begin setup
7. After installation, restart the server:
   - `Start` → `Power` → `Restart` → Click `Continue`

---

### ⚙️ Configure Certificate Services

1. After reboot, open `Server Manager`
2. Click the flag icon → Select `Configure Active Directory Certificate Services`
3. Click `Next`
4. Enable `Certification Authority` → Click `Next`
5. Continue clicking `Next` until the **Confirmation** page
6. Click `Configure` → Then `Close`

---

## 👤 User Configuration

### 👨‍💼 Create Domain Admin

1. Open `Start` → `Windows Administrative Tools` → `Active Directory Users and Computers`
2. Right-click your domain (e.g., `ad.lab`) → `New` → `User`
3. Enter:
   - First Name, Last Name
   - User logon name (e.g., `admin.ad`)
4. Set a password → Uncheck all options except `Password never expires` → Click `Next`
5. Expand the domain → Click `Users` → Double-click `Domain Admins`
6. Go to `Members` → Click `Add`
7. Enter the new user’s name → Click `Check Names` → `OK`
8. Click `Apply` → `OK`
9. Sign out → From login screen, select `Other user` → Log in with the new domain admin credentials

---

### 👤 Create AD User 1

1. Open `Active Directory Users and Computers`
2. Right-click the domain → `New` → `User`
3. Enter user details
4. Set a password → Check:
   - `User cannot change password`
   - `Password never expires`
5. Click `Next` → `Finish`

---

### 👤 Create AD User 2

Repeat the same steps as above to create a second AD user.

---

## 🧪 Making the AD Lab Exploitable

> ⚠️ **Optional**: This section is for creating a vulnerable AD environment for testing and learning purposes. Skip this if you do not intend to simulate attacks.

### 💻 Run Vulnerable AD Script

1. Right-click `Start` → Select `Windows PowerShell (Admin)`
2. Run the following commands:

```powershell
# Allow script execution
Set-ExecutionPolicy -ExecutionPolicy Bypass -Force

# Download and execute the vulnerable AD script
[System.Net.WebClient]::new().DownloadString('https://raw.githubusercontent.com/WaterExecution/vulnerable-AD-plus/master/vulnadplus.ps1') -replace 'change\.me', 'ad.lab' | Invoke-Expression
```


## 🛡️ Group Policy Configuration

With the Domain Controller fully set up, we will now configure Group Policies to manage security settings, remote access, and administrative behavior across the AD environment.

---

## 🚫 Disable Windows Defender & Firewall

1. Open `Start` → `Windows Administrative Tools` → `Group Policy Management`
2. Expand: `Forest` → `Domains` → your domain (e.g., `ad.lab`)
3. Right-click the domain → `Create a GPO in this domain and link here`
   - Name: `Disable Protections`
4. Right-click `Disable Protections` → `Edit`

### 🔧 Disable Defender Antivirus

- Navigate to:  
  `Computer Configuration` → `Policies` → `Administrative Templates` → `Windows Components` → `Windows Defender Antivirus`
- Double-click `Turn off Windows Defender Antivirus` → Set to `Enabled` → Click `Apply` → `OK`
- Double-click `Real-time Protection` → Edit `Turn off real-time protection` → Set to `Enabled` → `Apply` → `OK`

### 🔧 Disable Firewall

- Navigate to:  
  `Computer Configuration` → `Policies` → `Administrative Templates` → `Network` → `Network Connections` → `Windows Defender Firewall` → `Domain Profile`
- Edit `Windows Defender Firewall: Protect all network connections` → Set to `Disabled` → `Apply` → `OK`

5. Close the editor → Right-click `Disable Protections` in Group Policy Management → Select `Enforced`

---

## 🔓 Enable Remote Login for Local Admins

1. Right-click the domain → `Create a GPO in this domain and link here`
   - Name: `Local Admin Remote Login`
2. Right-click `Local Admin Remote Login` → `Edit`
3. Navigate to:  
   `Computer Configuration` → `Preferences` → `Windows Settings` → `Registry`
4. Right-click `Registry` → `New` → `Registry Item`

### 🧾 Registry Item Settings

- **Hive**: `HKEY_LOCAL_MACHINE`  
- **Key Path**: `SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System`  
- **Value name**: `LocalAccountTokenFilterPolicy`  
- **Value type**: `REG_DWORD`  
- **Value data**: `1`

Click `Apply` → `OK` → Close the editor

---

## 🌐 Enable WinRM Server

1. Right-click the domain → `Create a GPO in this domain and link here`
   - Name: `Enable WinRM Server`
2. Right-click `Enable WinRM Server` → `Edit`

### 🔧 WinRM Settings

- Navigate to:  
  `Computer Configuration` → `Policies` → `Administrative Templates` → `Windows Components` → `Windows Remote Management (WinRM)` → `WinRM Service`
- Edit `Allow remote server management through WinRM` → Set to `Enabled`  
  - **IPv4 filter**: `*` → `Apply` → `OK`
- Edit `Allow Basic authentication` → `Enabled` → `Apply` → `OK`
- Edit `Allow unencrypted traffic` → `Enabled` → `Apply` → `OK`

### 🔧 Start WinRM Service

- Navigate to:  
  `Computer Configuration` → `Preferences` → `Control Panel Settings` → `Services`
- Right-click `Services` → `New` → `Service`
  - **Startup**: `Automatic`
  - **Service Name**: `Windows Remote Management (WS-Management)`
  - **Service Action**: `Start service` → `Apply` → `OK`

### 🔧 Enable Remote Shell

- Navigate to:  
  `Computer Configuration` → `Policies` → `Administrative Templates` → `Windows Components` → `Windows Remote Shell`
- Edit `Allow Remote Shell Access` → Set to `Enabled` → `Apply` → `OK`

---

## 🖥️ Enable RDP (Remote Desktop Protocol)

1. Right-click the domain → `Create a GPO in this domain and link here`
   - Name: `Enable RDP`
2. Right-click `Enable RDP` → `Edit`
3. Navigate to:  
   `Computer Configuration` → `Policies` → `Administrative Templates` → `Windows Components` → `Remote Desktop Services` → `Remote Desktop Session Host` → `Connections`
4. Edit `Allow users to connect remotely using Remote Desktop Services` → Set to `Enabled` → `Apply` → `OK`

---

## 🔁 Enable RPC (Remote Procedure Call)

1. Right-click the domain → `Create a GPO in this domain and link here`
   - Name: `Enable RPC`
2. Right-click `Enable RPC` → `Edit`
3. Navigate to:  
   `Computer Configuration` → `Administrative Templates` → `System` → `Remote Procedure Call`
4. Edit `Enable RPC Endpoint Mapper Client Authentication` → Set to `Enabled` → `Apply` → `OK`

---

## 📥 Enforce Domain Policies

1. Open `Windows PowerShell (Admin)`
2. Run the following command:

```powershell
gpupdate /force
```


## 💻 Windows 10 Enterprise VM1 Setup

### 💿 OS Installation

1. Select `Windows 10 Enterprise VM1` → Click `Start`
2. Click `Next` → `Install now`
3. Accept the license → Click `Next`
4. Choose: `Custom: Install Windows only (advanced)`
5. Select `Disk 0` → Click `Next`
6. The VM will reboot several times during installation
7. Select your **Region** and **Keyboard Layout** → Click `Skip`
8. Choose `Domain join instead` to create a local account
9. Enter a username (e.g., `John`) → Click `Next`
10. Set a password → Click `Next`
11. Configure **Security Questions** → Click `Next`
12. Disable all features → Click `Accept`
13. Select `Not now` when prompted for additional setup
14. On the desktop, click `Yes` to allow internet access

---

### 📦 Guest Additions Installation

1. From the VM toolbar:  
   - `Devices` → `Remove disk from virtual drive`
2. Then:  
   - `Devices` → `Insert Guest Additions CD image`
3. Open File Explorer → Select the mounted disk
4. Run `VBoxWindowsAdditions`
5. Click `Next` → `Next` → `Install`
6. Choose `Reboot now` → Click `Finish`
7. After reboot, remove the Guest Additions disk
8. Use `Right Ctrl + F` to toggle fullscreen mode

---

## 🏷️ Adding VM1 to the Domain

1. Search for `This PC` → Right-click → `Properties`
2. Click `Advanced system settings` → `Computer Name` tab → `Change`
3. Set a **Computer Name** (e.g., `WIN10-JOHN`)
4. Under **Member of**, select `Domain` → Enter: `ad.lab`
5. Click `More` → Set **Primary DNS suffix** to `ad.lab` → Click `OK`
6. Enter Domain Admin credentials → Click `OK`
7. Click `OK` through confirmation prompts → Click `Restart Now`
8. On login screen → Select `Other user`
9. Enter AD user credentials (e.g., `ad.lab\john`) → Press `Enter`
10. Open PowerShell → Run `whoami` to confirm domain login

---

## 💻 Windows 10 Enterprise VM2 Setup

Follow the same steps as above to configure the second VM for the second user (e.g., `Jane`).

- Use the first name of the second AD user during local account setup
- Join the domain using the same process
- Log in using the second AD users credentials

---

## 📘 Appendix

### 🧩 Lab Summary

In this module, we:

- Set up 3 VMs:
  - `Windows Server 2019` as the **Domain Controller**
  - Two `Windows 10 Enterprise` VMs as **client devices**
- Enabled:
  - **DHCP**
  - **DNS Forwarding**
  - **AD Certificate Services**
  - **Group Policies** for all domain devices

> 🧹 You may delete the Windows Server 2019 ISO if not needed.  
> Keep the Windows 10 ISO for future use (e.g., FlareVM setup).

---

## 🔍 DNS & DHCP Verification

- Open **DHCP Manager** → Confirm IP addresses match assigned values
- Open **DNS Manager** → Verify DNS entries for client devices

---

## 📸 Taking VM Snapshots

> ⚠️ Power off VMs before taking snapshots to avoid instability

1. Select a VM → Click the `☰` (hamburger menu) → `Snapshots`
2. Click `Take` → Enter a descriptive name → Click `OK`
3. Repeat for:
   - `Windows Server 2019`
   - `Windows 10 Enterprise VM1`
   - `Windows 10 Enterprise VM2`
4. Return to VM settings via `☰` → `Details`

---

## 🧪 Alternative AD Setup

Explore more AD lab variations:

- [How to Setup a Basic Home Lab Running Active Directory – YouTube](https://www.youtube.com/watch?v=MHsI8hJmggI)
- [How to Build an Active Directory Hacking Lab – YouTube](https://www.youtube.com/watch?v=xftEuVQ7kY0)

## 🧨 Hacking AD Lab

Learn about common AD attack techniques:

- [Hack Your VirtualBox AD Lab](https://benheater.com/hack-your-virtualbox-ad-lab/)
- [Active Directory Methodology – HackTricks](https://book.hacktricks.wiki/en/index.html)

---

## 🚀 What’s Next?

With the AD Lab complete, we are ready to move on to malware analysis.

👉 - [Next → Setting Up a Malware Analysis Lab](/posts/HomeLab-Malware_Lab)