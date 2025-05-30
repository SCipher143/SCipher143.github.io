---
layout: post
title: "Installing Kali Linux for Your Lab"
date: 2025-04-27 10:19:00 -0700
categories: [Project Work, Home Lab]
tags: [HL]
description: Security and Pentest Home Lab Environment
permalink: /posts/HomeLab-Installing-Kali_Linux
---

# 🚀 Kali Linux Installation Module

In this module, we’ll install Kali Linux. This VM will also be used in the next module to complete the pfSense setup.

---

## ⬇️ Download Kali Linux

Go to the official Kali Linux website and download the 64-bit Recommended Installer:  
`https://www.kali.org/get-kali/#kali-installer-images`  
> The file is around 4GB, so download time may vary.

Once complete, you’ll have an `.iso` file.

---

## 🖥️ Create Kali Linux VM

1. Open VirtualBox and click **New** in the toolbar.  
2. Name the VM, choose a storage location, and leave the ISO image option empty.  
3. Set:  
   - Type: `Linux`  
   - Version: `Debian (64-bit)`  
4. Click **Next**.

![Desktop View](/assets/img/HomeLab/HL-45.png){: width="700" height="400" }

5. Keep default settings (unless you prefer KDE and have the resources). Click **Next**.

![Desktop View](/assets/img/HomeLab/HL-46.png){: width="700" height="400" }

6. Set disk size to `80GB` and click **Next**.

![Desktop View](/assets/img/HomeLab/HL-47.png){: width="700" height="400" }

7. Review and click **Finish**.

---

### 🗂️ Organize VMs in Groups

- Right-click the Kali Linux VM → **Move to Group** → **[New]**  
- Rename group to `Management`.

![Desktop View](/assets/img/HomeLab/HL-48.png){: width="700" height="400" }

- Create another group for firewall VM: select both groups (`Ctrl+Click`), right-click → **Move to Group** → **[New]**  
- Rename to `Home Lab`.

![Desktop View](/assets/img/HomeLab/HL-49.png){: width="700" height="400" }

---

## ⚙️ Configure Kali Linux VM Settings

1. Select Kali Linux VM → **Settings**

![Desktop View](/assets/img/HomeLab/HL-50.png){: width="700" height="400" }

2. In **System** → **Motherboard**:  
   - Boot Order: Hard Disk (top), then Optical  
   - Uncheck Floppy

![Desktop View](/assets/img/HomeLab/HL-51.png){: width="700" height="400" }

3. In **System** → **Processor**: Enable `PAE/NX`.

![Desktop View](/assets/img/HomeLab/HL-54.png){: width="700" height="400" }

4. In **Display** → **Screen**: Increase Video Memory to `128 MB`.

![Desktop View](/assets/img/HomeLab/HL-52.png){: width="700" height="400" }

5. In **Storage**:  
   - Click empty disk under `Controller: IDE`  
   - Click disk icon on the right → select downloaded Kali `.iso`

![Desktop View](/assets/img/HomeLab/HL-53.png){: width="700" height="400" }

---

### 🌐 Network Configuration

- Go to **Network** → **Adapter 1**  
- Attached to: `Internal Network`  
- Network Name: `LAN 0`  
- Adapter Type: `Paravirtualized Network (virtio-net)`

![Desktop View](/assets/img/HomeLab/HL-55.png){: width="700" height="400" }

---

## 🛠️ Install Kali Linux

> **Warning:**  
> Make sure your pfSense VM is running before starting Kali installation.  
{: .prompt-warning }

1. Select Kali Linux VM → Click **Start**  
2. In installer menu, choose **Graphical Install**

![Desktop View](/assets/img/HomeLab/HL-56.png){: width="700" height="400" }

3. Choose your language, location, and keyboard layout.

![Desktop View](/assets/img/HomeLab/HL-57.png){: width="700" height="400" }

![Desktop View](/assets/img/HomeLab/HL-58.png){: width="700" height="400" }

![Desktop View](/assets/img/HomeLab/HL-59.png){: width="700" height="400" }

4. Enter hostname (changeable later), leave domain empty, continue.

![Desktop View](/assets/img/HomeLab/HL-60.png){: width="700" height="400" }

![Desktop View](/assets/img/HomeLab/HL-61.png){: width="700" height="400" }

5. Set username and password for login.

![Desktop View](/assets/img/HomeLab/HL-62.png){: width="700" height="400" }

![Desktop View](/assets/img/HomeLab/HL-63.png){: width="700" height="400" }

6. Choose timezone and continue.

7. Select disk (usually `sda`) and continue.

![Desktop View](/assets/img/HomeLab/HL-65.png){: width="700" height="400" }

8. Choose **Guided - use entire disk** → continue.

![Desktop View](/assets/img/HomeLab/HL-64.png){: width="700" height="400" }

9. Choose **All files in one partition** → continue.

![Desktop View](/assets/img/HomeLab/HL-66.png){: width="700" height="400" }

10. Confirm partition changes → continue.

![Desktop View](/assets/img/HomeLab/HL-67.png){: width="700" height="400" }

11. After base install, choose desktop environment:  
- `GNOME`: visually rich  
- `XFCE`: lightweight and fast  
- `KDE Plasma`: feature-rich, needs ≥ 2 cores & 4GB RAM

![Desktop View](/assets/img/HomeLab/HL-68.png){: width="700" height="400" }

![Desktop View](/assets/img/HomeLab/Hl-69.png){: width="700" height="400" }

![Desktop View](/assets/img/HomeLab/HL-70.png){: width="700" height="400" }

12. Wait for install to finish → click **Continue** to reboot.

---

## 🔑 Log In & Final Setup

- After reboot, enter your password at login.

![Desktop View](/assets/img/HomeLab/HL-71.png){: width="700" height="400" }

- Kali Linux auto-installs Guest Additions in VM.

- Press `Right Ctrl + F` for fullscreen toggle.

- Open Terminal, run:

```bash
ip a
```

![Desktop View](/assets/img/HomeLab/HL-72.png){: width="700" height="400" }

#### 🌀 Update Kali Linux

Open the Terminal and run the following command to update your system:

```shell
sudo apt update && sudo apt full-upgrade
```

When prompted, type `Y` to continue the update.

![Desktop View](/assets/img/HomeLab/HL-73.png){: width="700" height="400" }

After the update, run the command to remove unused packages:

```shell
sudo apt autoremove
```

![Desktop View](/assets/img/HomeLab/HL-74.png){: width="700" height="400" }

> **Troubleshoot Black Screen on Boot**  
> If you see a black screen after booting Kali Linux, it might be related to a recent update. This issue can appear randomly.  
>  
> Try changing the "Graphics Controller" to `VBoxSVGA` in **Settings** → **Display** → **Screen**.  
>  
> Another fix is to power off the VM and restart it. You may need to restart it 2–3 times for it to work properly.  
{: .prompt-tip }

---

In the next module, we will access the pfSense Web UI and complete the remaining configuration.

👉 [Configuring pfSense Firewall for Security](/posts/HomeLab-pfSense_Configuration)