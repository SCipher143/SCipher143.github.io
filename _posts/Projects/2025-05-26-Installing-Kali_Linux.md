---
layout: post
title: "Installing Kali Linux"
date: 2025-05-26 10:19:00 -0700
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

Keep the default settings and click `Next`.

Set the disk size to `80GB`, then click `Next`.

Review the settings and click `Finish`.

Organize VMs in Groups

Right-click the Kali Linux VM in the sidebar, select `Move to Group`, and choose [New].

Rename the group to `Management`.

Create another group for your firewall VM (`Ctrl+Click` to select both groups), right-click, and choose `Move to Group` → [`New`].

Rename the new group to `Home Lab`.

## Configure Kali Linux VM Settings

Click on the Kali Linux VM and select `Settings` from the toolbar.

In `System` → `Motherboard`, set the boot order with Hard Disk at the top, followed by `Optical`. Uncheck `Floppy`.

Under `System` → `Processor`, enable `PAE/NX`.

In `Display` → `Screen`, increase Video Memory to `128 MB`.

In `Storage`, click the empty disk under `Controller: IDE`, then select the small disk icon on the right side of the Optical Drive. Choose the downloaded Kali Linux .iso file.

## Network Configuration

In `Network` → `Adapter 1`, select Internal Network for Attached to, and set the network name to `LAN 0`. Under Adapter Type, select `Paravirtualized Network (virtio-net)`.

## Install Kali Linux

Make sure your pfSense VM is running (if previously shut down) before starting the Kali installation.

Select Kali Linux in VirtualBox and click `Start`.

In the installer menu, select `Graphical Install`.

Choose your language, location, and keyboard layout.

Enter a hostname for the VM (you can change it later), leave the domain name field empty, then continue.

Set a username and password. This will be used for logging in.

Choose your timezone, then continue.

Select the disk (usually `sda`) and continue.

Select `Guided - use entire disk`, then continue.

Choose `All files in one partition` and continue.

Confirm the partitioning changes and continue.

Once the base system is installed, choose a desktop environment. `GNOME` is a good choice if you want a visually appealing setup, while `XFCE` is lighter and faster. `KDE Plasma` is more resource-heavy but has more features, so it’s best with at least `2 cores and 4GB of RAM`.

Wait for the installation to finish. Once done, click `Continue` to reboot.

Log In and Final Setup

After rebooting, you’ll see the login screen. Enter the password you set earlier to log in.

Kali Linux automatically installs Guest Additions when it detects it's running in a VM.

Press `Right Ctrl+F` to switch to Fullscreen mode (and you can press it again to exit fullscreen mode.) You should now see the VM scale to your screen.

Open the Terminal and run `ip a` to check your IP address. The VM should be connected to the LAN and have internet access.

#### Update Kali Linux

Open the Terminal and run the following command to update your system:

sudo apt update && sudo apt full-upgrade

When prompted, type Y to continue the update.

After the update, run the command to remove unused packages:

sudo apt autoremove

Delete the .iso File

If you don’t plan on using the .iso file again, you can delete it now to free up space.

Troubleshoot Black Screen on Boot

If you see a black screen after booting Kali Linux, it might be related to a recent update. This issue can appear randomly.

Try changing the "Graphics Controller" to VBoxSVGA in Settings → Display → Screen.

Another fix is to power off the VM and restart it. You may need to restart it 2-3 times for it to work properly.

