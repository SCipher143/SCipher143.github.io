---
layout: post
title: "Installing pfSense"
date: 2025-04-26 10:19:00 -0700
categories: [Project Work, Home Lab]
tags: [HL]
description: Security and Pentest Home Lab Environment
permalink: /posts/HomeLab-Installing-pfSense
---

# Part 2 - pfSense Setup & Configuration

In this module, we will cover the installation process of pfSense and perform the initial configuration needed to integrate our lab subnets into the system.

Go to the following link: [pfSense CE Download](https://atxfiles.netgate.com/mirror/downloads/)

![Desktop View](/assets/img/HomeLab/HL-6.png){: width="700" height="400" }

The downloaded file will have the extension `.iso.gz.` Use a decompression software like `7-Zip` to extract the image.

## pfSense VM Creation

Launch VirtualBox. Check on `Tools` from the sidebar and then Select `New` from the Toolbar.

![Desktop View](/assets/img/HomeLab/HL-7.png){: width="700" height="400" }

For the `Name`, enter a label that clearly identifies the virtual machine. The Folder option specifies where the VM files will be stored. Under the ISO Image dropdown, choose Others and browse to select the `.iso` file you downloaded earlier. Set the Type to `BSD` and the Version to `FreeBSD (64-bit)`, then click `Next`.

![Desktop View](/assets/img/HomeLab/HL-8.png){: width="700" height="400" }

Next, allocate memory and CPU resources for the virtual machine. The default settings are usually fine, so you can proceed by clicking Next.

![Desktop View](/assets/img/HomeLab/HL-9.png){: width="700" height="400" }

On the storage configuration screen, set the disk size to 20GB to allocate that much space for the VM.

![Desktop View](/assets/img/HomeLab/HL-10.png){: width="700" height="400" }

Review your configuration settings. If everything looks correct, click Finish to complete the setup.

You should now see your new virtual machine listed in the sidebar.

## Adding VM to Group

I like to keep my VMs organized by using the Groups feature of VirtualBox. This makes it easy to store related VMs together.

Right-click on the pfSense VM from the sidebar, select `Move to Group -> [New]`. The VM will now be added to a Group called `New Group`.

![Desktop View](/assets/img/HomeLab/HL-11.png){: width="700" height="400" }

Right-click on the Group, and select Rename Group. Name the Group `Firewall`.

![Desktop View](/assets/img/HomeLab/HL-12.png){: width="700" height="400" }

## pfSense VM Configuration

Before we boot the VM we need to configure some settings related to VirtualBox. Select the pfSense VM from the sidebar and then click on `Settings`.

![Desktop View](/assets/img/HomeLab/HL-13.png){: width="700" height="400" }

#### System Configuration

Select `System -> Motherboard` in the Boot Order section use the arrows to move the `Hard Disk` to the top, `Optical` should be next. Ensure that `Floppy` is unchecked.

![Desktop View](/assets/img/HomeLab/HL-14.png){: width="700" height="400" }

#### Audio & USB Configuration

Go to the `Audio` tab and uncheck the `Enable Audio` option. Since the VM we are configuring is a router we do not need audio.

![Desktop View](/assets/img/HomeLab/HL-15.png){: width="700" height="400" }

Go to the `USB` tab and uncheck the `Enable USB Controller` option. Since the VM we are configuring is a router we do not need USB support.

![Desktop View](/assets/img/HomeLab/HL-16.png){: width="700" height="400" }

#### Network Configuration

Go to `Network` -> `Adapter 1`. For the Attached to field select `NAT`. Expand the Advanced section and for `Adaptor Type` select `Paravirtualized Network (virtio-net)`.

![Desktop View](/assets/img/HomeLab/HL-17.png){: width="700" height="400" }

Select `Adapter 2`. Tick the `Enable Network Adapter` option. For the Attached to option select `Internal Network`. For Name enter `LAN 0`. Expand the Advanced section. For Adapter Type select `Paravirtualized Network (virtio-net)`.

![Desktop View](/assets/img/HomeLab/HL-18.png){: width="700" height="400" }

Select `Adapter 3`. Tick the `Enable Network Adapter option`. For the Attached to option select `Internal Network`. For Name enter `LAN 1`. Expand the Advanced section. For Adapter Type select `Paravirtualized Network (virtio-net)`.

![Desktop View](/assets/img/HomeLab/HL-19.png){: width="700" height="400" }

Select `Adapter 4`. Tick the `Enable Network Adapter option`. For the Attached to option select `Internal Network`. For Name enter `LAN 2`. Expand the Advanced section. For Adapter Type select `Paravirtualized Network (virtio-net)`.

Once done click on OK to save the changes and close the configuration menu.

![Desktop View](/assets/img/HomeLab/HL-20.png){: width="700" height="400" }

> The network diagram shown in the first module consisted of 6 network interfaces. VirtualBox only allows us to configure 4 interfaces uses the UI. Towards the end of the guide we will see how to add more interfaces using VirtualBox CLI.
{: .prompt-info }

## pfSense Installation

Select the pfSense virtual machine from the list on the left, then click `Start` in the toolbar.

![Desktop View](/assets/img/HomeLab/HL-21.png){: width="700" height="400" }

As the VM boots, a splash screen and a stream of system messages will appear. Wait until the license agreement screen is displayed, then press `Enter` to accept it.

![Desktop View](/assets/img/HomeLab/HL-22.png){: width="700" height="400" }

When prompted to begin the installation, press `Enter`.

![Desktop View](/assets/img/HomeLab/HL-23.png){: width="700" height="400" }

Choose the `Auto (ZFS)` option by pressing `Enter` again.

![Desktop View](/assets/img/HomeLab/HL-24.png){: width="700" height="400" }

Next, confirm the installation by selecting `Proceed with Installation` and pressing `Enter`.

![Desktop View](/assets/img/HomeLab/HL-25.png){: width="700" height="400" }

On the following screen, select `Stripe - No Redundancy` by pressing `Enter`.

![Desktop View](/assets/img/HomeLab/HL-26.png){: width="700" height="400" }

When asked to choose the target disk, press the `Spacebar` to select the hard drive (usually labeled `ada0`), then hit `Enter` to continue.

![Desktop View](/assets/img/HomeLab/HL-27.png){: width="700" height="400" }

Use the Left Arrow key to highlight `YES`, and press `Enter` to confirm your selection.

![Desktop View](/assets/img/HomeLab/HL-28.png){: width="700" height="400" }

Wait for the system to finish installing.

![Desktop View](/assets/img/HomeLab/HL-29.png){: width="700" height="400" }

Once the process is complete, press `Enter` to reboot the virtual machine.

![Desktop View](/assets/img/HomeLab/HL-30.png){: width="700" height="400" }

## pfSense Configuration

After pfSense reboots, the next step is to assign the network adapters configured in the VM settings.

When prompted about setting up VLANs, type `n` and press Enter, as we’ll manually configure the interfaces next.

![Desktop View](/assets/img/HomeLab/HL-31.png){: width="700" height="400" }

Assign the interfaces as follows:

- WAN interface name: vtnet0

- LAN interface name: vtnet1

- Optional 1 interface name: vtnet2

- Optional 2 interface name: vtnet3

When asked to proceed, type `y` and press Enter.

![Desktop View](/assets/img/HomeLab/HL-32.png){: width="700" height="400" }

At this point, pfSense automatically receives an IPv4 address for the WAN interface from the VirtualBox DHCP server. The `LAN` interface also gets an IP address via pfSense’s own DHCP service. However, the `OPT1` and `OPT2` interfaces do not have IP addresses assigned. To ensure the IP addresses remain consistent across reboots, we will manually set static IPv4 addresses for the `LAN`, `OPT1`, and `OPT2` interfaces.

> The WAN interface IP address will be different in your case. The IP assignment is performed by the VirtualBox DHCP server.
{: .prompt-info }

![Desktop View](/assets/img/HomeLab/HL-33.png){: width="700" height="400" }

#### Configuring LAN (vtnet1)

To begin configuring the interfaces, press `2` from the main menu to choose “Set interface(s) IP address,” then press `2` again to select the `LAN` interface.

When asked if you want to configure the LAN interface via DHCP, enter `n`.

Set the IPv4 address to: `10.0.0.1`

Set the IPv4 subnet bit count to: `24`

![Desktop View](/assets/img/HomeLab/HL-34.png){: width="700" height="400" }

You can skip the gateway configuration by pressing `Enter`, as it's not required for LAN interfaces.

When prompted to configure an IPv6 address using DHCP6, enter `n`, and then press `Enter` again to skip the IPv6 address.

Enable the DHCP server for the LAN by entering `y`, then specify the address range:

Start: `10.0.0.11`

End: `10.0.0.243`

When asked to switch the webConfigurator protocol to HTTP, enter `n`.

![Desktop View](/assets/img/HomeLab/HL-35.png){: width="700" height="400" }

pfSense will now apply the provided settings. Press `Enter` to complete the `LAN` interface configuration.

![Desktop View](/assets/img/HomeLab/HL-36.png){: width="700" height="400" }

You’ll notice the LAN interface is now set to the static IP you entered.

![Desktop View](/assets/img/HomeLab/HL-37.png){: width="700" height="400" }

#### Configuring OPT1 (vtnet2)
Press `2` to set interface IP addresses again, then press `3` to choose `OPT1`.

When prompted about DHCP: enter `n`

Set the IPv4 address to: `10.6.6.1`

Set the subnet bit count to: `24`

![Desktop View](/assets/img/HomeLab/HL-38.png){: width="700" height="400" }

Skip the gateway by pressing `Enter`.

For IPv6, enter `n` and press `Enter` again to skip the address.

Enable DHCP on OPT1 by entering `y`:

Start: `10.6.6.11`

End: `10.6.6.243`

Decline reverting to HTTP by entering `n`, then press `Enter` to save and return to the main menu.

![Desktop View](/assets/img/HomeLab/HL-39.png){: width="700" height="400" }

#### Configuring OPT2 (vtnet3)
Repeat the process by pressing `2`, then `4` to configure `OPT2`.

For DHCP: enter `n`

Set the IPv4 address to: `10.80.80.1`

Subnet bit count: `24`

![Desktop View](/assets/img/HomeLab/HL-40.png){: width="700" height="400" }

Skip the gateway and IPv6 steps by pressing `Enter` when prompted.

For the DHCP server, choose `n`, since this interface will be used for the `Active Directory (AD)` lab. The `Domain Controller (DC)` in that setup will handle DHCP, so we’re disabling it on pfSense for `OPT2`.

When prompted about switching to HTTP, again enter `n`, then press `Enter` to return to the main menu.

![Desktop View](/assets/img/HomeLab/HL-41.png){: width="700" height="400" }

The final static IP assignments should be:

LAN: `10.0.0.1`

OPT1: `10.6.6.1`

OPT2: `10.80.80.1`

![Desktop View](/assets/img/HomeLab/HL-42.png){: width="700" height="400" }

At this stage, the network interfaces in pfSense have been fully set up. Additional configuration will be handled later once `Kali Linux` is installed. We’ll use `Kal`i to access the pfSense Web Interface and continue the setup from there.

The pfSense Web Interface is accessible from all configured LAN interfaces.

## Shutdown pfSeense

When we start the lab pfSense is the first VM that has to be booted. When we shut down the lab pfSense will be the last VM that is stopped.

Enter a option: `6` (Halt system) Do you want to process?: `y`

This will initiate the shutdown sequence.

![Desktop View](/assets/img/HomeLab/HL-43.png){: width="700" height="400" }

## Post-Installation Cleanup

After the VM is shut down. Click on Settings from the toolbar.

Go to the Storage tab. In the Storage Devices section click on the pfSense .iso image then click on the small disk image on the right side of the Optical Drive option.

From the dropdown select Remove Disk from Virtual Drive. Click on OK to save the changes and close the configuration menu.

![Desktop View](/assets/img/HomeLab/HL-34.png){: width="700" height="400" }

The `.iso` file along with the `.iso.gz` file that was downloaded to create the VM can be deleted if you do not want to store them.

In the next module, we will set up Kali Linux on the `LAN` interface. This VM will be used to configure and manage pfSense. It will also be used as the attack VM to target the vulnerable systems on the `OPT1 (CYBER_RANGE)`.

- [Next → Installing Kali Linux](/posts/HomeLab-Installing-Kali_Linux)