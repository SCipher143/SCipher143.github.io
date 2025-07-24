---
layout: post
title: "Investigating with Splunk"
date: 2025-07-23 17:30:00 -0700
categories: [SOC, Blue Team, SIEM]
tags: [THM, Splunk, Investigation, Threat Hunting, Blue Team]
description: Investigate suspicious activity using Splunk in this TryHackMe room. Learn how to analyze logs, detect anomalies, and uncover attacker behavior in a realistic SOC environment.
---

SOC Analyst Johny has observed some anomalous behaviours in the logs of a few windows machines. It looks like the adversary has access to some of these machines and successfully created some backdoor. His manager has asked him to pull those logs from suspected hosts and ingest them into Splunk for quick investigation. Our task as SOC Analyst is to examine the logs and identify the anomalies.

## Investigating With Splunk Questions

---

### How many events were collected and Ingested in the index main?

![Desktop View](/assets/img/Splunk_IV/1.png){: width="700" height="400" }

---

### On one of the infected hosts, the adversary was successful in creating a backdoor user. What is the new username?

![Desktop View](/assets/img/Splunk_IV/2.png){: width="700" height="400" }

![Desktop View](/assets/img/Splunk_IV/3.png){: width="700" height="400" }

`A1berto`

---

### On the same host, a registry key was also updated regarding the new backdoor user. What is the full path of that registry key?

![Desktop View](/assets/img/Splunk_IV/4.png){: width="700" height="400" }

`HKLM\SAM\SAM\Domains\Account\Users\Names\A1berto`

---

### Examine the logs and identify the user that the adversary was trying to impersonate.

![Desktop View](/assets/img/Splunk_IV/5.png){: width="700" height="400" }

`Alberto`

---

### What is the command used to add a backdoor user from a remote computer?

![Desktop View](/assets/img/Splunk_IV/6.png){: width="700" height="400" }

---

### How many times was the login attempt from the backdoor user observed during the investigation?

![Desktop View](/assets/img/Splunk_IV/7.png){: width="700" height="400" }

Looking through the events we seee there is no log on. And no Event ID to signal it.

![Desktop View](/assets/img/Splunk_IV/8.png){: width="700" height="400" }

`0`

---

### What is the name of the infected host on which suspicious Powershell commands were executed?

`James.browne`

![Desktop View](/assets/img/Splunk_IV/9.png){: width="700" height="400" }

---

### PowerShell logging is enabled on this device. How many events were logged for the malicious PowerShell execution?

![Desktop View](/assets/img/Splunk_IV/10.png){: width="700" height="400" }

![Desktop View](/assets/img/Splunk_IV/11.png){: width="700" height="400" }

`79`

---

### An encoded Powershell script from the infected host initiated a web request. What is the full URL?

> **Note**
> `https://gchq.github.io/CyberChef/`

![Desktop View](/assets/img/Splunk_IV/12.png){: width="700" height="400" }

![Desktop View](/assets/img/Splunk_IV/13.png){: width="700" height="400" }

![Desktop View](/assets/img/Splunk_IV/14.png){: width="700" height="400" }

![Desktop View](/assets/img/Splunk_IV/15.png){: width="700" height="400" }

![Desktop View](/assets/img/Splunk_IV/16.png){: width="700" height="400" }

`hxxp[://]10[.]10[.]10[.]5/news[.]php`