---
layout: post
title: "Investigating with ELK: ItsyBitsy"
date: 2025-07-25 9:00:00 -0700
categories: [SOC, Blue Team, ELK]
tags: [THM, ELK, Kibana, IDS, C2 Detection, Threat Hunting, Elastic Stack]
description: Apply your ELK Stack skills in the ItsyBitsy TryHackMe room by investigating a potential Command & Control (C2) communication alert. Learn how to use IDS alerts, analyze suspicious traffic, and correlate data in a simulated SOC scenario.
---

> **Note**
> **Username**: `Admin`
> **Password**: `elastic123`

---

## Scenario - Investigate a potential C2 communication alert

During normal SOC monitoring, Analyst John observed an alert on an IDS solution indicating a potential C2 communication from a user Browne from the HR department. A suspicious file was accessed containing a malicious pattern `THM:{ ________ }`. A week-long HTTP connection logs have been pulled to investigate. Due to limited resources, only the connection logs could be pulled out and are ingested into the `connection_logs` index in Kibana.

Our task in this room will be to examine the network connection logs of this user, find the link and the content of the file, and answer the questions.

### How many events were returned for the month of March 2022?

![Desktop View](/assets/img/ItsyBitsy/1.png){: width="700" height="400" }

#### What is the IP associated with the suspected user in the logs?

![Desktop View](/assets/img/ItsyBitsy/2.png){: width="700" height="400" }

#### The user’s machine used a legit windows binary to download a file from the C2 server. What is the name of the binary?

![Desktop View](/assets/img/ItsyBitsy/3.png){: width="700" height="400" }

#### The infected machine connected with a famous filesharing site in this period, which also acts as a C2 server used by the malware authors to communicate. What is the name of the filesharing site?

![Desktop View](/assets/img/ItsyBitsy/4.png){: width="700" height="400" }

#### What is the full URL of the C2 to which the infected host is connected?

![Desktop View](/assets/img/ItsyBitsy/5.png){: width="700" height="400" }

#### ![Desktop View](/assets/img/ItsyBitsy/1.png){: width="700" height="400" }

![Desktop View](/assets/img/ItsyBitsy/6.png){: width="700" height="400" }

#### The file contains a secret code with the format THM{_____}.

`THM{SECRET__CODE`