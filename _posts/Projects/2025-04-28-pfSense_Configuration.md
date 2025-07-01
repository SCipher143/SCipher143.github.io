---
layout: post
title: "Configuring pfSense Firewall for Security"
date: 2025-04-28 10:19:00 -0700
categories: [Project Work, Home Lab]
tags: [HomeLab]
description: Security and Pentest Home Lab Environment
permalink: /posts/HomeLab-pfSense_Configuration
---

## 🔧 Finishing pfSense Setup

In this module, we’ll complete the setup of `pfSense` and configure firewall rules for the subnets in your home lab.

---

## ⚙️ pfSense General Configuration

### 🌐 Web Portal Setup

1. On the `Kali Linux` VM, open a browser and visit:  
   `https://10.0.0.1`
2. Ignore the security warning and click:
   - `Advanced`
   - `Accept the Risk and Continue`
3. Log in to the `pfSense` Web UI using:
   - Username: `admin`
   - Password: `pfsense`
4. Click `Next` through the initial steps.

#### 🔧 General Settings

- Set a hostname and domain name for the `pfSense` VM.
- Uncheck `Override DNS`, then click `Next`.
- Set your local timezone, then click `Next`.
- Scroll to the **RFC1918 Networks** section and **uncheck** `Block RFC1918 Private Networks`.  
  This is required because we’re using a private IP on the WAN interface.
- Leave other settings as-is, then click `Next`.
- Set a new `admin` password, store it securely, and click `Reload`.
- Click `Finish` to access the `pfSense` dashboard.

---

## ✏️ Interface Renaming

Rename interfaces to make management easier:

1. Go to `Interfaces` → `OPT1`
   - Description: `CYBER_RANGE`
   - Click `Save` → then `Apply Changes`
2. Go to `Interfaces` → `OPT2`
   - Description: `AD_LAB`
   - Click `Save` → then `Apply Changes`

---

## 🧭 DNS Resolver Configuration

1. Navigate to: `Services` → `DNS Resolver`
2. Enable all recommended options at the bottom.
3. Click `Advanced Settings` and enable additional resolver options.
4. Click `Save` → then `Apply Changes`.

---

## ❌ Disable DHCPv6

To prevent IPv6 address assignment on the WAN interface:

1. Go to `Interfaces` → `WAN`
2. Set `IPv6 Configuration Type` to `None`
3. Click `Save` → then `Apply Changes`

---

## 🔁 Restart pfSense

Restart the VM to ensure settings take effect and the WAN interface receives an IPv4 address.

---

## 🧠 Advanced Configuration

1. Go to `System` → `Advanced` → `Networking` tab
2. Under **Network Interfaces**, enable performance optimization
3. Click `Save` → `OK` to reboot

After reboot, log in with the new `admin` password.

---

## 🌍 Kali Linux Static IP Assignment

1. Go to `Status` → `DHCP Leases`
2. Find the `Kali Linux` VM → Click the `+` icon to assign a static IP
3. Set the IP to: `10.0.0.2` → Click `Save` → `Apply Changes`

### 💻 Refresh IP in Kali

In Kali’s terminal:

```bash
ip a l eth0
```

## 🔄 Refresh Kali Static IP

To make sure `Kali Linux` uses its static IP:

### 💻 Restart the Network Interface

In Kali’s terminal, run:

```bash
sudo ip l set eth0 down && sudo ip l set eth0 up
```

## 💻 Verify Kali Static IP

After restarting the network interface, confirm your Kali VM is using the static IP:

```bash
ip a l eth0
```

# 🔥 pfSense Firewall Configuration

## 🔐 LAN Rules

1. Go to: `Firewall` → `Rules` → `LAN` tab  
2. Click **Add rule to top**  
3. Set:  
   - Action: `Block`  
   - Address Family: `IPv4+IPv6`  
   - Protocol: `Any`  
   - Source: `LAN subnets`  
   - Destination: `WAN subnets`  
   - Description: `Block access to WAN services`  
4. Click **Save** → **Apply Changes**

---

## 🌐 CYBER_RANGE Rules

### 📋 Create RFC1918 Alias

1. Go to: `Firewall` → `Aliases`  
2. Click **Add** under the IP tab  
3. Enter:  
   - Name: `RFC1918`  
   - Description: `Private IPv4 Address Space`  
   - Type: `Network(s)`  
   - Networks:  
     - `10.0.0.0/8`  
     - `172.16.0.0/12`  
     - `192.168.0.0/16`  
     - `169.254.0.0/16`  
     - `127.0.0.0/8`  
4. Click **Save** → **Apply Changes**

### 🔐 Add Rules for CYBER_RANGE

1. Go to: `Firewall` → `Rules` → `CYBER_RANGE`  
2. Add these rules in order:

   - ✅ Allow intra-network traffic  
     - Source: `CYBER_RANGE subnets`  
     - Destination: `CYBER_RANGE address`  

   - ✅ Allow access to Kali Linux  
     - Source: `CYBER_RANGE subnets`  
     - Destination: `10.0.0.2`  

   - ✅ Allow traffic to public IPs only  
     - Source: `CYBER_RANGE subnets`  
     - Destination: `RFC1918`  
     - **Enable Invert match**

   - ❌ Block all other traffic  
     - Action: `Block`  
     - Address Family: `IPv4+IPv6`  
     - Protocol: `Any`  
     - Source: `CYBER_RANGE subnets`  

3. Click **Save** after each rule → **Apply Changes**

---

## 🧱 AD_LAB Rules

1. Go to: `Firewall` → `Rules` → `AD_LAB`  
2. Add these rules:

   - ❌ Block access to WAN  
     - Action: `Block`  
     - Source: `AD_LAB subnets`  
     - Destination: `WAN subnets`  

   - ❌ Block access to CYBER_RANGE  
     - Action: `Block`  
     - Source: `AD_LAB subnets`  
     - Destination: `CYBER_RANGE subnets`  

   - ✅ Allow all other traffic  
     - Source: `AD_LAB subnets`  
     - Destination: `Any`  

3. Click **Save** after each rule → **Apply Changes**

---

## 🔁 Reboot pfSense

1. Go to: `Diagnostics` → `Reboot`  
2. Click **Submit**  
3. After reboot, you’ll be redirected to the login page.

---

## 🧩 What’s Next?

pfSense is now configured and secured. Next steps:  

- Add more vulnerable VMs to `CYBER_RANGE`  
- Test connectivity from the Kali Linux VM

👉 - [Next → Building Your Cyber Range Environment](/posts/HomeLab-Cyber_Range)