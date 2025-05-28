---
layout: post
title: "Configuring pfSense Firewall for Security"
date: 2025-05-27 10:19:00 -0700
categories: [Project Work, Home Lab]
tags: [HL]
description: Security and Pentest Home Lab Environment
permalink: /posts/HomeLab-pfSense_Configuration
---

**Finishing pfSense Setup**

In this module, we will complete the pfSense setup and configure firewall rules for the subnets we created in our home lab.

# pfSense General Configuration

## Web Portal Setup

On the Kali Linux VM, open the web browser and go to `https://10.0.0.1`.

You’ll see a warning: "Warning: Potential Security Risk Ahead." This can be ignored because the URL uses HTTPS, which isn’t secure by default. Click `Advanced` and then `Accept the Risk and Continue`.

This will take you to the pfSense Web UI login page. Log in with the default credentials:

Username: `admin`

Password: `pfsense`

Click `Next`, then `Next` again.

General Settings
In the General Information section, give the pfSense VM a Hostname and Domain Name. This name will help identify the pfSense VM on your network.

Uncheck Override DNS and click `Next`.

Set your Timezone and click `Next`.

Scroll down and find the RFC1918 Networks section. Uncheck the Block RFC1918 Private Networks option.

This is necessary because our WAN interface uses a private IP (not a real WAN IP) to communicate with the host system.

Don’t change anything else on this page. Click `Next`.

Set a new admin password, store it securely, then click Reload to apply the changes.

Click Finish.

Once the onboarding process is complete, you will see the pfSense dashboard.

Interface Renaming
In the navigation bar, go to Interfaces → OPT1.

In the Description field, enter CYBER_RANGE and click Save.

A popup will appear at the top of the page. Click Apply Changes.

Now, go to Interfaces → OPT2.

In the Description field, enter AD_LAB and click Save.

A popup will appear at the top of the page. Click Apply Changes again.

DNS Resolver Configuration
From the navigation bar, go to Services → DNS Resolver.

Scroll to the bottom and enable the highlighted options. No need to save yet.

Click on Advanced Settings, scroll down to the Advanced Resolver Options, and enable the highlighted options. Then, scroll to the bottom and click Save.

A popup will appear at the top of the page. Click Apply Changes.

Disabling DHCPv6
Some newer versions of pfSense/VirtualBox use IPv6 for dynamic IP assignment. Disable it to prevent IPv6 from being assigned to the WAN interface.

Go to Interfaces → WAN and set IPv6 Configuration Type to None.

Scroll down and click Save.

Click Apply Changes when the popup appears.

Restart pfSense
Restart the pfSense VM to ensure the WAN interface has an IPv4 address.

Advanced Configuration
From the navigation bar, go to System → Advanced and select the Networking tab.

Scroll down to the Network Interfaces section and enable the highlighted option to improve pfSense performance. Click Save.

A popup will appear; click OK to reboot pfSense.

After reboot, you will be prompted to log in again. Use the new password to access the dashboard.

Kali Linux Static IP Assignment
From the navigation bar, go to Status → DHCP Leases.

In the Leases section, find the Kali Linux VM and click the + icon to assign a static IP.

Enter 10.0.0.2 in the IP Address field and click Save.

Click Apply Changes when the popup appears.

Refresh Kali Linux IP Address
Open the terminal on Kali and use the following command to check the current IP address:

css
Copy
Edit
ip a l eth0
To force Kali to release the current IP address and use the static one, run:

arduino
Copy
Edit
sudo ip l set eth0 down && sudo ip l set eth0 up
Afterward, check again with:

css
Copy
Edit
ip a l eth0
pfSense Firewall Configuration
From the navigation bar, go to Firewall → Rules.

LAN Rules
Go to the LAN tab, which contains predefined rules.

Click Add rule to top to create a new rule.

Action: Block

Address Family: IPv4+IPv6

Protocol: Any

Source: LAN subnets

Destination: WAN subnets

Description: Block access to services on WAN interface

Scroll down and click Save.

Click Apply Changes in the popup.

The final LAN rules should look like the example provided. Make sure the rule order is correct; drag the rules to reorder if needed.

CYBER_RANGE Rules
Go to Firewall → Aliases.

Click Add under the IP tab to create a new alias:

Name: RFC1918

Description: Private IPv4 Address Space

Type: Network(s)

Networks:

10.0.0.0/8

172.16.0.0/12

192.168.0.0/16

169.254.0.0/16

127.0.0.0/8

Click Save, then Apply Changes.

Now go to Firewall → Rules and select the CYBER_RANGE tab.

Click Add rule to end to create the following rules:

Allow traffic to all devices on the CYBER_RANGE network:

Address Family: IPv4+IPv6

Protocol: Any

Source: CYBER_RANGE subnets

Destination: CYBER_RANGE address

Allow traffic to Kali Linux VM (10.0.0.2):

Source: CYBER_RANGE subnets

Destination: Address or Alias - 10.0.0.2

Allow traffic to non-private IPv4 Addresses:

Source: CYBER_RANGE subnets

Destination: Address or Alias - RFC1918 (Select Invert match)

Block all other traffic:

Action: Block

Address Family: IPv4+IPv6

Protocol: Any

Source: CYBER_RANGE subnets

Click Save after each rule.

AD_LAB Rules
Select the AD_LAB tab and use Add rule to end for the following rules:

Block access to services on WAN interface:

Action: Block

Source: AD_LAB subnets

Destination: WAN subnets

Block traffic to CYBER_RANGE:

Action: Block

Source: AD_LAB subnets

Destination: CYBER_RANGE subnets

Allow traffic to all other subnets and the internet:

Source: AD_LAB subnets

Destination: Any

Click Save after each rule and then Apply Changes.

pfSense Reboot
To apply the new firewall rules, go to Diagnostics → Reboot.

Click Submit to reboot pfSense.

Once pfSense reboots, you’ll be redirected to the login page.

In the next module, we’ll add some vulnerable VMs to the CYBER_RANGE interface and test the connectivity from the Kali Linux VM.

