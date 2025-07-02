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

---

## Deploy the VM

Please visit the link `http://10.10.59.73/`, which will show you the following page:

![Desktop View](/assets/img/File_Inclusion/3.png){: width="700" height="400" }

---

## Path Traversal

Also known as `Directory traversal`, a web security vulnerability allows an attacker to read operating system resources, such as local files on the server running an application. The attacker exploits this vulnerability by manipulating and abusing the web application's URL to locate and access files or directories stored outside the application's root directory.>

Path traversal vulnerabilities occur when the user's input is passed to a function such as file_get_contents in PHP. It's important to note that the function is not the main contributor to the vulnerability. Often poor input validation or filtering is the cause of the vulnerability. In PHP, you can use the file_get_contents to read the content of a file. You can find more information about the function [here](https://www.php.net/manual/en/function.file-get-contents.php).

The following graph shows how a web application stores files in `/var/www/app`. The happy path would be the user requesting the contents of `userCV.pdf` from a defined path `/var/www/app/CVs`.

We can test out the URL parameter by adding payloads to see how the web application behaves. Path traversal attacks, also known as the dot-dot-slash attack, take advantage of moving the directory one step up using the double dots ../. If the attacker finds the entry point, which in this case get.php?file=, then the attacker may send something as follows, `http://webapp.thm/get.php?file=../../../../etc/passwd`

Suppose there isn't input validation, and instead of accessing the PDF files at `/var/www/app/CVs` location, the web application retrieves files from other directories, which in this case `/etc/passwd`. Each `..` entry moves one directory until it reaches the root directory `/`. Then it changes the directory to `/etc`, and from there, it read the `passwd` file.


As a result, the web application sends back the file's content to the user.



Similarly, if the web application runs on a Windows server, the attacker needs to provide Windows paths. For example, if the attacker wants to read the `boot.ini` file located in `c:\boot.ini`, then the attacker can try the following depending on the target OS version:

`http://webapp.thm/get.php?file=../../../../boot.ini` or

`http://webapp.thm/get.php?file=../../../../windows/win.ini`

The same concept applies here as with Linux operating systems, where we climb up directories until it reaches the root directory, which is usually .

Sometimes, developers will add filters to limit access to only certain files or directories. Below are some common OS files you could use when testing.

 

| **Location**                     | **Description**                                                                 |
|----------------------------------|----------------------------------------------------------------------------------|
| `/etc/issue`                     | Contains a message or system identification to be printed before the login prompt. |
| `/etc/profile`                   | Controls system-wide default variables, such as export variables, umask, terminal types, and mail notifications. |
| `/proc/version`                  | Specifies the version of the Linux kernel.                                       |
| `/etc/passwd`                    | Contains all registered users that have access to a system.                      |
| `/etc/shadow`                    | Contains information about users' encrypted passwords.                           |
| `/root/.bash_history`            | Stores the command history for the root user.                                    |
| `/var/log/dmesg`                 | Contains global system messages, including those from system startup.            |
| `/var/mail/root`                 | Stores all emails for the root user.                                             |
| `/root/.ssh/id_rsa`              | Private SSH key for the root or other valid users.                               |
| `/var/log/apache2/access.log`    | Logs accessed requests for the Apache web server.                                |
| `C:\boot.ini`                    | Contains the boot options for computers using BIOS firmware.                     |

---

## Local File Inclusion (LFI)

LFI attacks against web applications are often due to a developers' lack of security awareness. With PHP, using functions such as `include`, `require`, `include_once`, and `require_once` often contribute to vulnerable web applications. In this room, we'll be picking on PHP, but it's worth noting LFI vulnerabilities also occur when using other languages such as ASP, JSP, or even in Node.js apps. LFI exploits follow the same concepts as path traversal.

In this section, we will walk you through various `LFI` scenarios and how to exploit them.

#1. Suppose the web application provides two languages, and the user can select between the `EN` and `AR`

```php
<?PHP 
	include($_GET["lang"]);
?>
```

The PHP code above uses a `GET` request via the URL parameter `lang` to include the file of the page. The call can be done by sending the following HTTP request as follows: `http://webapp.thm/index.php?lang=EN.php` to load the English page or `http://webapp.thm/index.php?lang=AR.php` to load the Arabic page, where `EN.php` and `AR.php` files exist in the same directory.

Theoretically, we can access and display any readable file on the server from the code above if there isn't any input validation. Let's say we want to read the /etc/passwd file, which contains sensitive information about the users of the Linux operating system, we can try the following: `http://webapp.thm/get.php?file=/etc/passwd`

In this case, it works because there isn't a directory specified in the include function and no input validation.

`Now apply what we discussed and try to read /etc/passwd file. Also, answer question #1 below.`

#2. Next, In the following code, the developer decided to specify the directory inside the function.

 
```php
<?PHP 
	include("languages/". $_GET['lang']); 
?>
```

In the above code, the developer decided to use the `include` function to call `PHP` pages in the `languages` directory only via `lang` parameters.

If there is no input validation, the attacker can manipulate the URL by replacing the `lang` input with other OS-sensitive files such as `/etc/passwd`.

Again the payload looks similar to the `path traversal`, but the include function allows us to include any called files into the current page. The following will be the exploit:

`http://webapp.thm/index.php?lang=../../../../etc/passwd`

`Now apply what we discussed, try to read files within the server, and figure out the directory specified in the include function and answer question #2 below.`

### Challenge

**Give Lab #1 a try to read /etc/passwd. What would the request URI be?**

![Desktop View](/assets/img/File_Inclusion/4.png){: width="700" height="400" }

**In Lab #2, what is the directory specified in the include function?**

