---
layout: post
title: "Metasploit Intro"
date: 2025-04-26 12:57:00 -0700
categories: [TryHackMe, Exploits, Metasploit]
tags: [THM, Exploits, Metasploit]
description: TryHackMe Cyber Security 101 Metasploit
---
# Metasploit Intro

### Quoted from TryHackMe Metasploit: Introduction

"Metasploit is the most widely used exploitation framework. Metasploit is a powerful tool that can support all phases of a penetration testing engagement, from information gathering to post-exploitation.



Metasploit has two main versions:

**Metasploit Pro**: The commercial version that facilitates the automation and management of tasks. This version has a **graphical user interface (GUI)**.

**Metasploit Framework**: The open-source version that works from the command line. This room will focus on this version, installed on the AttackBox and most commonly used penetration testing Linux distributions.


The Metasploit Framework is a set of tools that allow information gathering, scanning, exploitation, exploit development, post-exploitation, and more. While the primary usage of the Metasploit Framework focuses on the penetration testing domain, it is also useful for vulnerability research and exploit development.



The main components of the Metasploit Framework can be summarized as follows;

**msfconsole**: The main command-line interface.

**Modules**: supporting modules such as exploits, scanners, payloads, etc.

**Tools**: Stand-alone tools that will help vulnerability research, vulnerability assessment, or penetration testing. Some of these tools are msfvenom, pattern_create and pattern_offset. We will cover msfvenom within this module, but pattern_create and pattern_offset are tools useful in exploit development which is beyond the scope of this module."

"While using the Metasploit Framework, you will primarily interact with the Metasploit console."

### Practical Example

**Our IP- 10.10.*.***

**Target IP- 10.10.186.155**

Launch the Metasploit console with **msfconsole**

"**Exploit**: A piece of code that uses a vulnerability present on the target system.

**Vulnerability**: A design, coding, or logic flaw affecting the target system. The exploitation of a vulnerability can result in disclosing confidential information or allowing the attacker to execute code on the target system.

**Payload**: An exploit will take advantage of a vulnerability. However, if we want the exploit to have the result we want (gaining access to the target system, read confidential information, etc.), we need to use a payload. Payloads are the code that will run on the target system."

![Desktop View](/assets/img/THM-Metasploit/THM-1.png){: width="700" height="400" }

![Desktop View](/assets/img/THM-Metasploit/THM-2.png){: width="700" height="400" }

![Desktop View](/assets/img/THM-Metasploit/THM-3.png){: width="700" height="400" }

![Desktop View](/assets/img/THM-Metasploit/THM-4.png){: width="700" height="400" }

![Desktop View](/assets/img/THM-Metasploit/THM-5.png){: width="700" height="400" }

![Desktop View](/assets/img/THM-Metasploit/THM-6.png){: width="700" height="400" }

![Desktop View](/assets/img/THM-Metasploit/THM-7.png){: width="700" height="400" }

Regular comands can be used in the msfconsole such as help, ping, clear, ls, namp, and etc.

RHOST = Remote host and LHOST = local machine

### Important commands to rememeber: 

- **help**
- **history**
- **search**
- **info**
- **back**
- **use**
- **show options**
- **set**
- **unset**
- **show payloads**
- **run or exploit**
- **backround**
- **sessions**
- **g(sets the global value)**