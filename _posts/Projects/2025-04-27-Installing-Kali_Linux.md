---
layout: post
title: "Installing Kali Linux for Your Lab"
date: 2025-04-27 10:19:00 -0700
categories: [Project Work, Home Lab]
tags: [HL]
description: Security and Pentest Home Lab Environment
permalink: /posts/HomeLab-Installing-Kali_Linux
---

In this module, we are going to install Kali Linux. We will use this VM in the next module also to complete the pfSense setup.

## Download Kali Linux

Go to the official Kali Linux website and download the 64-bit Recommended Installer. (https://www.kali.org/get-kali/#kali-installer-images) The file is around 4GB, so it might take some time to download.

Once the download is complete, you’ll have an `.iso` file.

## Create Kali Linux VM

Open VirtualBox and click on `New` in the toolbar.

Name the VM, choose a location to store it, and leave the ISO image option empty.

Set `Type` to Linux and `Version` to Debian (64-bit), then click `Next`.

![Desktop View](/assets/img/HomeLab/HL-45.png){: width="700" height="400" }

Keep the default settings and click `Next` (unles you plan to use the KDE and have the power to do so).

![Desktop View](/assets/img/HomeLab/HL-46.png){: width="700" height="400" }

Set the disk size to `80GB`, then click `Next`.

![Desktop View](/assets/img/HomeLab/HL-47.png){: width="700" height="400" }

Review the settings and click `Finish`.

#### Organize VMs in Groups

Right-click the Kali Linux VM in the sidebar, select `Move to Group`, and choose [New].

Rename the group to `Management`.

![Desktop View](/assets/img/HomeLab/HL-48.png){: width="700" height="400" }

Create another group for your firewall VM (`Ctrl+Click` to select both groups), right-click, and choose `Move to Group` → [`New`].

Rename the new group to `Home Lab`.

![Desktop View](/assets/img/HomeLab/HL-49.png){: width="700" height="400" }

## Configure Kali Linux VM Settings

Click on the Kali Linux VM and select `Settings` from the toolbar.

![Desktop View](/assets/img/HomeLab/HL-50.png){: width="700" height="400" }

In `System` → `Motherboard`, set the boot order with Hard Disk at the top, followed by `Optical`. Uncheck `Floppy`.

![Desktop View](/assets/img/HomeLab/HL-51.png){: width="700" height="400" }

Under `System` → `Processor`, enable `PAE/NX`.

![Desktop View](/assets/img/HomeLab/HL-54.png){: width="700" height="400" }

In `Display` → `Screen`, increase Video Memory to `128 MB`.

![Desktop View](/assets/img/HomeLab/HL-52.png){: width="700" height="400" }

In `Storage`, click the empty disk under `Controller: IDE`, then select the small disk icon on the right side of the Optical Drive. Choose the downloaded Kali Linux .iso file.

![Desktop View](/assets/img/HomeLab/HL-53.png){: width="700" height="400" }

#### Network Configuration

In `Network` → `Adapter 1`, select Internal Network for Attached to, and set the network name to `LAN 0`. Under Adapter Type, select `Paravirtualized Network (virtio-net)`.

![Desktop View](/assets/img/HomeLab/HL-55.png){: width="700" height="400" }

## Install Kali Linux

> `Warning`
Make sure your pfSense VM is running (if previously shut down) before starting the Kali installation.
{: .prompt-warning }

Select Kali Linux in VirtualBox and click `Start`.

In the installer menu, select `Graphical Install`.

![Desktop View](/assets/img/HomeLab/HL-56.png){: width="700" height="400" }

Choose your language, location, and keyboard layout.

![Desktop View](/assets/img/HomeLab/HL-57.png){: width="700" height="400" }

![Desktop View](/assets/img/HomeLab/HL-58.png){: width="700" height="400" }

![Desktop View](/assets/img/HomeLab/HL-59.png){: width="700" height="400" }

Enter a hostname for the VM (you can change it later), leave the domain name field empty, then continue.

![Desktop View](/assets/img/HomeLab/HL-60.png){: width="700" height="400" }

![Desktop View](/assets/img/HomeLab/HL-61.png){: width="700" height="400" }

Set a username and password. This will be used for logging in.

![Desktop View](/assets/img/HomeLab/HL-62.png){: width="700" height="400" }

![Desktop View](/assets/img/HomeLab/HL-63.png){: width="700" height="400" }

Choose your timezone, then continue.

Select the disk (usually `sda`) and continue.

![Desktop View](/assets/img/HomeLab/HL-65.png){: width="700" height="400" }

Select `Guided - use entire disk`, then continue.

![Desktop View](/assets/img/HomeLab/HL-64.png){: width="700" height="400" }

Choose `All files in one partition` and continue.

![Desktop View](/assets/img/HomeLab/HL-66.png){: width="700" height="400" }

Confirm the partitioning changes and continue.

![Desktop View](/assets/img/HomeLab/HL-67.png){: width="700" height="400" }

Once the base system is installed, choose a desktop environment. `GNOME` is a good choice if you want a visually appealing setup, while `XFCE` is lighter and faster. `KDE Plasma` is more resource-heavy but has more features, so it’s best with at least `2 cores and 4GB of RAM`.

![Desktop View](/assets/img/HomeLab/HL-68.png){: width="700" height="400" }

![Desktop View](/assets/img/HomeLab/Hl-69.png){: width="700" height="400" }

![Desktop View](/assets/img/HomeLab/HL-70.png){: width="700" height="400" }

Wait for the installation to finish. Once done, click `Continue` to reboot.

Log In and Final Setup

After rebooting, you’ll see the login screen. Enter the password you set earlier to log in.

![Desktop View](/assets/img/HomeLab/HL-71.png){: width="700" height="400" }

Kali Linux automatically installs Guest Additions when it detects it's running in a VM.

Press `Right Ctrl+F` to switch to Fullscreen mode (and you can press it again to exit fullscreen mode.) You should now see the VM scale to your screen.

Open the Terminal and run `ip a` to check your IP address. The VM should be connected to the LAN and have internet access.

![Desktop View](/assets/img/HomeLab/HL-72.png){: width="700" height="400" }

#### Update Kali Linux

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

>Troubleshoot Black Screen on Boot

If you see a black screen after booting Kali Linux, it might be related to a recent update. This issue can appear randomly.

Try changing the "Graphics Controller" to VBoxSVGA in Settings → Display → Screen.

Another fix is to power off the VM and restart it. You may need to restart it 2-3 times for it to work properly.
{: .prompt-tip }

In the next module, we will access the pfSense Web UI and complete the remaining configuration.

- [Next → Configuring pfSense Firewall for Security](/posts/HomeLab-pfSense_Configuration)