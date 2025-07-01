---
layout: post
title: "File Inclusion"
date: 2025-07-01 09:00:00 -0700
categories: [TryHackMe, Web Vulnerabilities, File Inclusion]
tags: [tryhackme, file inclusion, lfi, rfi, directory traversal, web security]
description: Learn about file inclusion vulnerabilities in web applications, including Local File Inclusion (LFI), Remote File Inclusion (RFI), and directory traversal. Understand how attackers exploit these flaws and how to defend against them. Based on the TryHackMe File Inclusion room.
---

## Intro

In some scenarios, web applications are written to request access to files on a given system, including images, static text, and so on via parameters. Parameters are query parameter strings attached to the URL that could be used to retrieve data or perform actions based on user input. The following diagram breaks down the essential parts of a URL.

![Desktop View](/assets/img/File_Inclusion/1.png){: width="700" height="400" }

For example, parameters are used with Google searching, where `GET` requests pass user input into the search engine. `https://www.google.com/search?q=TryHackMe`. If you are not familiar with the topic, you can view the [How The Web Works module](https://tryhackme.com/module/how-the-web-works) to understand the concept.  

Let's discuss a scenario where a user requests to access files from a webserver. First, the user sends an HTTP request to the webserver that includes a file to display. For example, if a user wants to access and display their CV within the web application, the request may look as follows, `http://webapp.thm/get.php?file=userCV.pdf`, where the `file` is the parameter and the `userCV.pdf`, is the required file to access.

![Desktop View](/assets/img/File_Inclusion/2.png){: width="700" height="400" }

### Why do File inclusion vulnerabilities happen?

File inclusion vulnerabilities are commonly found and exploited in various programming languages for web applications, such as PHP that are poorly written and implemented. The main issue of these vulnerabilities is the input validation, in which the user inputs are not sanitized or validated, and the user controls them. When the input is not validated, the user can pass any input to the function, causing the vulnerability.
 
### What is the risk of File inclusion?

By default, an attacker can leverage file inclusion vulnerabilities to leak data, such as code, credentials or other important files related to the web application or operating system. Moreover, if the attacker can write files to the server by any other means, file inclusion might be used in tandem to gain remote command execution (RCE).

