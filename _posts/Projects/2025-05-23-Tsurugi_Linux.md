---
layout: post
title: "Installing Tsurugi Linux for DFIR Work"
date: 2025-05-23 08:25:00 -0700
categories: [Project Work, Home Lab]
tags: [HL]
description: Security and Pentest Home Lab Environment
permalink: /posts/HomeLab-Tsurugi_Linux
---

In this module, we are going to set up **Tsurugi Linux**, an OS pre-configured with many Digital Forensics & Incident Response tools. Before deploying the VM, we’ll create a new interface in `pfSense` named `SECURITY`. This segment will isolate DFIR tools from the rest of the network.

---

## ➕ Add New Interface in VirtualBox

VirtualBox only allows 4 adapters via the GUI, but we can add more using the **command-line**. Before proceeding:

- Ensure your pfSense VM is **powered off**
- Confirm the VM name (e.g., `"pfSense"`)

### 🔧 Create the Interface via PowerShell

Run the following in **PowerShell**:

```powershell
# Attach a new internal network
VBoxManage modifyvm "pfSense" --nic6 intnet

# Set adapter type to virtio-net
VBoxManage modifyvm "pfSense" --nictype6 virtio

# Name the internal network "LAN 4"
VBoxManage modifyvm "pfSense" --intnet6 "LAN 4"

# Mark the interface as connected
VBoxManage modifyvm "pfSense" --cableconnected6 on
```

## 🧮 Assign Interface in pfSense

1. **Start the pfSense VM**
2. At the terminal prompt, press `1` to **Assign Interfaces**
3. When asked about VLANs → type `n`

Assign the interfaces:

- **WAN:** `vtnet0`
- **LAN:** `vtnet1`  
- **OPT1:** `vtnet2`  
- **OPT2:** `vtnet3`  
- **OPT3:** `vtnet4`  
- **OPT4:** `vtnet5`

Type `y` to confirm and onboard the new interface.

---

## 🌐 Configure Interface IP

1. Back at the pfSense terminal, press `2` to **Set interface(s) IP address**
2. Choose **interface 6 (OPT4)**

Configure the settings:

- **IPv4 Address:** `10.10.10.1`
- **Subnet Bit Count:** `24`
- Skip upstream gateway (just press Enter)
- **IPv6:** Disabled

Enable **DHCP Server**:

- **Start Range:** `10.10.10.11`
- **End Range:** `10.10.10.243`

Decline HTTP switch: `n`

> OPT4 is now active and ready for segmentation.  
{: .prompt-tip }

---

## ✏️ Rename OPT4 to SECURITY

1. Log in to the **pfSense web UI**
2. Navigate to: **Interfaces → OPT4**
3. Set **Description** to: `SECURITY`
4. Scroll down → Click **Save**
5. Click **Apply Changes** when prompted

---

## 🛡️ SECURITY Interface – Firewall Rules

Go to: **Firewall → Rules → SECURITY**

### 🟥 Rule 1 – Block WAN Access

- **Action:** Block  
- **Address Family:** IPv4+IPv6  
- **Protocol:** Any  
- **Source:** SECURITY net  
- **Destination:** WAN net  
- **Description:** Block access to WAN  

Click **Save**. Ignore popup for now.

### 🟥 Rule 2 – Block LAN Access

- **Action:** Block  
- **Address Family:** IPv4+IPv6  
- **Protocol:** Any  
- **Source:** SECURITY net  
- **Destination:** LAN net  
- **Description:** Block access to LAN  

Click **Save**. Ignore popup.

### ✅ Rule 3 – Allow Internet & Internal Traffic

- **Action:** Pass  
- **Address Family:** IPv4+IPv6  
- **Protocol:** Any  
- **Source:** SECURITY net  
- **Destination:** Any  
- **Description:** Allow general traffic  

Click **Save**, then **Apply Changes** on the popup.

---

## 🔄 Reboot pfSense

To apply the firewall changes:

1. Navigate to **Diagnostics → Reboot**
2. Click **Submit**

After reboot, pfSense will return to the login page, confirming changes have been applied.

## 📥 Download Tsurugi Linux

1. Visit the official download page: [Tsurugi Linux - Downloads](https://tsurugi-linux.org/downloads/)
2. Choose one of the **mirror links** and download the latest ISO (e.g., `tsurugi-linux-2023.2.iso`)
   - The ISO is ~16GB, so allow time for download.
3. Once complete, you’ll have a `.iso` image file ready for VM creation.

---

## 💻 Create Tsurugi Linux VM

1. In VirtualBox, click **Tools** → **New**.
2. Name the VM and select the downloaded `.iso` as the startup disk. Click **Next**.
3. Set **Base Memory** to `4096MB` → Click **Next**.
4. Set **Hard Disk** size to `150GB`
   - ⚠️ *Tsurugi will fail to install with less than 110GB of storage.*
5. Click **Finish** once the summary looks correct.

---

## 📂 Organize the VM into Groups

1. Right-click the VM → **Move to Group** → **New**
2. Right-click the new group → **Rename Group** → `Security`
3. Right-click the `Security` group → **Move to Group** → `Home Lab`
4. Final structure should look like:

## ⚙️ Configure the VM

1. Select the VM → **Settings** → **System** → **Motherboard**
   - **Boot Order**: Ensure `Hard Disk` is first, followed by `Optical`
   - Uncheck `Floppy`
2. ✅ For **Tsurugi Linux 2024.1+**, enable `Enable EFI` under **Motherboard**
3. Go to **Network** → **Adapter 1**
   - **Attached to**: `Internal Network`
   - **Name**: `LAN 4`
4. Click **OK** to save.

---

## 📀 Install Tsurugi Linux

1. Start the VM → Press `Enter` to boot into GUI mode.
2. On the desktop, double-click `Displays` → set resolution to `1600x1050` → **Apply** → **Keep This Configuration**
   - 📝 This is required to view installer buttons.

3. Double-click `Install Tsurugi Linux 2023.2` to launch the installer.
4. Scroll down in the window → Choose Language → **Continue**
5. Select Keyboard → **Continue**
6. Enable:
   - `Install third-party software for graphics and Wi-Fi hardware and additional media features`
   → Click **Continue**
7. Click **Install Now** → **Continue** to write changes
8. Set timezone → **Continue**
9. Create user and password → **Continue**
10. After installation completes, click **Restart Now**

If prompted with a removal message, just press `Enter` to continue. Login with the configured credentials.

---

## 🔧 Post-Install Configuration

### 📦 Install Guest Additions

1. Go to **Devices** → **Insert Guest Additions CD Image**
2. Authenticate with your password when prompted.
3. Click the **CD icon** in the top-right → Select `Mount VBox_GAs`
4. If the icon doesn’t appear on the desktop, double-click the CD icon from the file manager
5. Select **Tools** → `Open Current Folder in Terminal`
6. Run the install command:

```bash
sudo ./VBoxLinuxAdditions.run
```

## 🧩 Finalizing Setup

### 🖥️ Enter Fullscreen Mode

After Guest Additions installation:

1. Press `Right Ctrl + F` to enter **Fullscreen Mode**.
   - Press the same keys again to exit fullscreen.
   - The VM display will scale automatically to your monitor size.

2. In the top-right corner of the VM window, click the **CD icon** → Select **Eject VBox_GAs** to safely remove the Guest Additions ISO.

---

### ⏻ Shutdown the System

To properly shut down the VM:

1. Click the **power icon** next to the clock
2. Select **Shut Down** from the menu
3. Confirm by clicking **Shut Down** again

---

## 🔄 System Update

Keeping Tsurugi up to date is essential for tool compatibility and security.

1. Open the **Terminator** terminal app from the desktop.
2. Run the following command:

```bash
sudo apt update && sudo apt full-upgrade
```

If prompted during the update:

- Press `Enter` to begin the installation
- Enter your **password** when requested

This will ensure your Tsurugi system is fully up-to-date with the latest packages and patches.

---

## 🧷 Creating a VM Snapshot

Before proceeding to the next lab module, it’s good practice to save your VM’s current state.

1. **Shut down** the Tsurugi VM
2. Click the **hamburger menu** next to the VM name → choose **Snapshots**
3. Click **Take**
4. Enter a descriptive name like `Tsurugi Clean Install`
5. Click **OK**
6. Return to the main view via the **Details** tab

Creating a snapshot lets you quickly revert to this clean baseline if needed.

---

## ⏭️ What’s Next?

In the next module, we’ll set up:

- **Ubuntu Linux**
- Download and install **Splunk**
- Deploy the **Splunk Universal Forwarder** on the **Domain Controller**

This integration allows us to collect event logs and security telemetry from Windows systems in our Active Directory environment.

Next 👉 [Setting Up Splunk: Basics & Configuration](/posts/HomeLab-Splunk)