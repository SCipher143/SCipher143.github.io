---
layout: post
title: "Building Your Cyber Range Environment"
date: 2025-04-29 08:00:00 -0700
categories: [Project Work, Home Lab]
tags: [HL]
description: Security and Pentest Home Lab Environment
permalink: /posts/HomeLab-Cyber_Range
---

# Setting Up Vulnerable Virtual Machines in VirtualBox

When downloading virtual machines (VMs) from the internet, you'll often come across two common file types: `.vmdk` (Virtual Machine Disk) and `.ova` (Open Virtualization Format). In this guide, we’ll walk through setting up a couple of intentionally vulnerable `VMs—Metasploitable 2` and `Chronos—inside VirtualBox`. These will be used in a home lab environment, with Kali Linux and pfSense acting as key components on the network.

---

## 🛠️ Prerequisites

Before you begin:
- Make sure your `pfSense` VM is running.
- Start your `Kali Linux` VM as it will be used for connectivity testing.

---

## 🧪 VM 1: Metasploitable 2

### 📥 Step 1: Download & Extract

Download `Metasploitable 2` from [VulnHub](https://www.vulnhub.com/entry/metasploitable-2,29/). You'll get a `.zip` file.

Extract it using a tool like `7-Zip`. Inside, you'll find multiple files. The one you need is the `.vmdk` file.

### 🖥️ Step 2: Create a New VM

1. Open `VirtualBox`.
2. Go to `Tools` → `New`.
3. Name your VM (e.g., `Metasploitable 2`).
4. Choose:
   - Type: `Linux`
   - Version: `Ubuntu (32-bit)`
5. Set memory to `1024MB`.
6. Select `Do Not Add a Virtual Hard Disk`.
7. Finish the setup and ignore any warnings.

### 💾 Step 3: Attach the Disk

1. Move the `.vmdk` file into the VM’s folder.
2. Go to `Settings` → `Storage`.
3. Select `Controller: SATA` → Click `Add Hard Disk` → Choose the `.vmdk`.

### 🌐 Step 4: Configure Networking

- Go to `Settings` → `Network`.
- Adapter 1:
  - Attached to: `Internal Network`
  - Name: `LAN 1`

### ✅ Step 5: Boot & Test

Start the VM. Log in using:

```text
Username: msfadmin
Password: msfadmin
```

**Check IP:**

```bash
ip a l eth0
```

**Test connectivity:**

```bash
ping google.com -c 5
ping 10.0.0.2 -c 5    # Ping Kali
ping 10.6.6.12 -c 5   # From Kali, ping Metasploitable
```
## 🧪 VM 2: Chronos

### 📥 Step 1: Download the `.ova`

Download `Chronos` from [VulnHub](https://www.vulnhub.com/entry/chronos-1,368/). It's provided as a `.ova` file.

### 📦 Step 2: Import into `VirtualBox`

1. Open `VirtualBox` → `Tools` → `Import`.
2. Select the `.ova` file.
3. Adjust the settings:
   - RAM: `1024MB`
   - MAC Address Policy: `Generate new MAC addresses for all network adapters`
4. Click `Finish` and wait for the import to complete.

### 🌐 Step 3: Configure Network

- Go to `Settings` → `Network`.
- Adapter 1:
  - Attached to: `Internal Network`
  - Name: `LAN 1`
  - Adapter Type: `Paravirtualized Network (virtio-net)`

> ⚠️ **Note:** Some older VMs (like `Metasploitable 2`) don’t work well with the `Paravirtualized Network` adapter. If the VM doesn't connect, try switching to `Intel PRO/1000`.

### 🕵️ Step 4: Find IP via `pfSense`

Since you can't log into `Chronos` directly:

1. Go to the `pfSense` web UI.
2. Navigate to: `Status` → `DHCP Leases`.
3. Find the IP assigned to `Chronos` (e.g., `10.6.6.13`).

Test from the `Kali Linux` VM:

```bash
ping 10.6.6.13 -c 5
```

## 🗂️ Organizing VMs in Groups

To keep things tidy in `VirtualBox`:

- Right-click a VM → `Move to Group` → `New Group` (e.g., `Cyber Range`)
- You can also nest groups like:
  - `Home Lab` → `Cyber Range`

---

## ⚙️ Adapter Tip

The `Paravirtualized Network Adapter` provides better performance, but compatibility can vary depending on the VM.

**Recommended steps:**

1. First try using `Paravirtualized Network`.
2. If the VM doesn't connect:
   - Shut down the VM.
   - Change the adapter type to `Intel PRO/1000` (or another compatible option).
   - Start the VM again.

---

## 🔜 What’s Next?

With both `Metasploitable 2` and `Chronos` set up and running inside your virtual lab, you're now ready to start building out your `Active Directory` lab — which we'll walk through in the next post.


👉 - [Next → Setting Up Active Directory Lab: Part 1](/posts/HomeLab-Active_Directory1)