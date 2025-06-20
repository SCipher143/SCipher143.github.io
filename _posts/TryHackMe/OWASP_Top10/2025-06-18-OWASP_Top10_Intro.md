---
layout: post
title: "OWASP Top 10"
date: 2025-06-16 12:57:00 -0700
categories: [TryHackMe, Web Security]
tags: [THM, Exploits]
description: OWASP Top 10-2021 from TryHackMe
---

![Desktop View](/assets/img/OWASP/OWASP_img.png){: width="700" height="400" }

## 1. Broken Access Control

Websites have pages that are protected from regular visitors. For example, only the site's admin user should be able to access a page to manage other users. If a website visitor can access protected pages they are not meant to see, then the access controls are broken.

A regular visitor being able to access protected pages can lead to the following:

Being able to view sensitive information from other users
Accessing unauthorized functionality

Simply put, broken access control allows attackers to bypass authorisation, allowing them to view sensitive data or perform tasks they aren't supposed to.

### Insecure Direct Object Reference

**IDOR** or **Insecure Direct Object Reference** refers to an access control vulnerability where you can access resources you wouldn't ordinarily be able to see. This occurs when the programmer exposes a Direct Object Reference, which is just an identifier that refers to specific objects within the server. By object, we could mean a file, a user, a bank account in a banking application, or anything really.

For example, let's say we're logging into our bank account, and after correctly authenticating ourselves, we get taken to a URL like this `https://bank.thm/account?id=111111`. On that page, we can see all our important bank details, and a user would do whatever they need to do and move along their way, thinking nothing is wrong.

There is, however, a potentially huge problem here, anyone may be able to change the `id` parameter to something else like `222222`, and if the site is incorrectly configured, then he would have access to someone else's bank information.

The application exposes a direct object reference through the id parameter in the URL, which points to specific accounts. Since the application isn't checking if the logged-in user owns the referenced account, an attacker can get sensitive information from other users because of the IDOR vulnerability. Notice that direct object references aren't the problem, but rather that the application doesn't validate if the logged-in user should have access to the requested account.

### Challenge

**What is the Flag?**

Logging in and viewing the site you can change the id direct object reference. First I changed it to 2 which brought me to a different page but not with the flag I wanted. So then I chhanged the ID to 0 and that brought me to the flag that I needed.

![Desktop View](/assets/img/OWASP/1.png){: width="700" height="400" }

![Desktop View](/assets/img/OWASP/2.png){: width="700" height="400" }

![Desktop View](/assets/img/OWASP/3.png){: width="700" height="400" }

![Desktop View](/assets/img/OWASP/4.png){: width="700" height="400" }

---

## 2. Cryptographic Failure

A cryptographic failure refers to any vulnerability arising from the misuse (or lack of use) of cryptographic algorithms for protecting sensitive information. Web applications require cryptography to provide confidentiality for their users at many levels.

Cryptographic failures often end up in web apps accidentally divulging sensitive data. This is often data directly linked to customers (e.g. names, dates of birth, financial information), but it could also be more technical information, such as usernames and passwords.

At more complex levels, taking advantage of some cryptographic failures often involves techniques such as "Man in The Middle Attacks", whereby the attacker would force user connections through a device they control. Then, they would take advantage of weak encryption on any transmitted data to access the intercepted information (if the data is even encrypted in the first place). Of course, many examples are much simpler, and vulnerabilities can be found in web apps that can be exploited without advanced networking knowledge. Indeed, in some cases, the sensitive data can be found directly on the web server itself.

The most common way to store a large amount of data in a format easily accessible from many locations is in a database. This is perfect for something like a web application, as many users may interact with the website at any time. Database engines usually follow the Structured Query Language (SQL) syntax.

In a production environment, it is common to see databases set up on dedicated servers running a database service such as MySQL or MariaDB; however, databases can also be stored as files. These are referred to as "flat-file" databases, as they are stored as a single file on the computer. This is much easier than setting up an entire database server and could potentially be seen in smaller web applications. Accessing a database server is outwith the scope of today's task, so let's focus instead on flat-file databases.

As mentioned previously, flat-file databases are stored as a file on the disk of a computer. Usually, this would not be a problem for a web app, but what happens if the database is stored underneath the root directory of the website (i.e. one of the files accessible to the user connecting to the website)? Well, we can download and query it on our own machine, with full access to everything in the database. Sensitive Data Exposure, indeed!

That is a big hint for the challenge, so let's briefly cover some of the syntax we would use to query a flat-file database.

The most common (and simplest) format of a flat-file database is an SQLite database. These can be interacted with in most programming languages and have a dedicated client for querying them on the command line. This client is called `sqlite3` and is installed on many Linux distributions by default.

Let's suppose we have successfully managed to download a database:

```Text
user@linux$ ls -l 
-rw-r--r-- 1 user user 8192 Feb  2 20:33 example.db
                                                                                                                                                              
user@linux$ file example.db 
example.db: SQLite 3.x database, last written using SQLite version 3039002, file counter 1, database pages 2, cookie 0x1, schema 4, UTF-8, version-valid-for 1
```

We can see that there is an SQLite database in the current folder.

To access it, we use sqlite3 <database-name>:

```Text
user@linux$ sqlite3 example.db                     
SQLite version 3.39.2 2022-07-21 15:24:47
Enter ".help" for usage hints.
sqlite> 
```

From here, we can see the tables in the database by using the .tables command:

```Text
user@linux$ sqlite3 example.db                     
SQLite version 3.39.2 2022-07-21 15:24:47
Enter ".help" for usage hints.
sqlite> .tables
customers
```
At this point, we can dump all the data from the table, but we won't necessarily know what each column means unless we look at the table information. First, let's use PRAGMA table_info(customers); to see the table information. Then we'll use SELECT * FROM customers; to dump the information from the table:

```Text
sqlite> PRAGMA table_info(customers);
0|cudtID|INT|1||1
1|custName|TEXT|1||0
2|creditCard|TEXT|0||0
3|password|TEXT|1||0

sqlite> SELECT * FROM customers;
0|Joy Paulson|4916 9012 2231 7905|5f4dcc3b5aa765d61d8327deb882cf99
1|John Walters|4671 5376 3366 8125|fef08f333cc53594c8097eba1f35726a
2|Lena Abdul|4353 4722 6349 6685|b55ab2470f160c331a99b8d8a1946b19
3|Andrew Miller|4059 8824 0198 5596|bc7b657bd56e4386e3397ca86e378f70
4|Keith Wayman|4972 1604 3381 8885|12e7a36c0710571b3d827992f4cfe679
5|Annett Scholz|5400 1617 6508 1166|e2795fc96af3f4d6288906a90a52a47f
```

We can see from the table information that there are four columns: custID, custName, creditCard and password. You may notice that this matches up with the results. Take the first row:

0|Joy Paulson|4916 9012 2231 7905|5f4dcc3b5aa765d61d8327deb882cf99
 

We have the custID (0), the custName (Joy Paulson), the creditCard (4916 9012 2231 7905) and a password hash (5f4dcc3b5aa765d61d8327deb882cf99).

When it comes to hash cracking, Kali comes pre-installed with various tools. If you know how to use these, then feel free to do so; however, they are outwith the scope of this material.

Instead, we will be using the online tool: `Crackstation`. This website is extremely good at cracking weak password hashes. For more complicated hashes, we would need more sophisticated tools; however, all of the crackable password hashes used in today's challenge are weak `MD5` hashes, which Crackstation should handle very nicely.

### Challenge 

It's now time to put what you've learnt into practice! For this challenge, connect to the web application at `http://10.10.51.0:81/`.

**What is the name of the mentioned directory?**

![Desktop View](/assets/img/OWASP/5.png){: width="700" height="400" }

![Desktop View](/assets/img/OWASP/6.png){: width="700" height="400" }

**Navigate to the directory you found in question one. What file stands out as being likely to contain sensitive data?**

![Desktop View](/assets/img/OWASP/7.png){: width="700" height="400" }

![Desktop View](/assets/img/OWASP/8.png){: width="700" height="400" }

**Use the supporting material to access the sensitive data. What is the password hash of the admin user?**

![Desktop View](/assets/img/OWASP/9.png){: width="700" height="400" }

![Desktop View](/assets/img/OWASP/10.png){: width="700" height="400" }

**Crack the hash. What is the admin's plaintext password?**

![Desktop View](/assets/img/OWASP/11.png){: width="700" height="400" }

**Log in as the admin. What is the flag?**

![Desktop View](/assets/img/OWASP/12.png){: width="700" height="400" }

---

## 3. Injection

Injection flaws are very common in applications today. These flaws occur because the application interprets user-controlled input as commands or parameters. Injection attacks depend on what technologies are used and how these technologies interpret the input. Some common examples include:

**SQL Injection**: This occurs when user-controlled input is passed to SQL queries. As a result, an attacker can pass in SQL queries to manipulate the outcome of such queries. This could potentially allow the attacker to access, modify and delete information in a database when this input is passed into database queries. This would mean an attacker could steal sensitive information such as personal details and credentials.

**Command Injection**: This occurs when user input is passed to system commands. As a result, an attacker can execute arbitrary system commands on application servers, potentially allowing them to access users' systems.
The main defence for preventing injection attacks is ensuring that user-controlled input is not interpreted as queries or commands. There are different ways of doing this:

**Using an allow list**: when input is sent to the server, this input is compared to a list of safe inputs or characters. If the input is marked as safe, then it is processed. Otherwise, it is rejected, and the application throws an error.

**Stripping input**: If the input contains dangerous characters, these are removed before processing.
Dangerous characters or input is classified as any input that can change how the underlying data is processed. Instead of manually constructing allow lists or stripping input, various libraries exist that can perform these actions for you.

#### Command Injection

Command Injection occurs when server-side code (like PHP) in a web application makes a call to a function that interacts with the server's console directly. An injection web vulnerability allows an attacker to take advantage of that call to execute operating system commands arbitrarily on the server. The possibilities for the attacker from here are endless: they could list files, read their contents, run some basic commands to do some recon on the server or whatever they wanted, just as if they were sitting in front of the server and issuing commands directly into the command line. 

Once the attacker has a foothold on the web server, they can start the usual enumeration of your systems and look for ways to pivot around.

### Chalenge 

What strange text file is in the website's root directory?

![Desktop View](/assets/img/OWASP/13.png){: width="700" height="400" }

![Desktop View](/assets/img/OWASP/14.png){: width="700" height="400" }

How many non-root/non-service/non-daemon users are there?

![Desktop View](/assets/img/OWASP/15.png){: width="700" height="400" }

What user is this app running as?

![Desktop View](/assets/img/OWASP/16.png){: width="700" height="400" }

What is the user's shell set as?

`$(cat /etc/passwd)` not only finds the users but helped us find what the user's shell is set at.

![Desktop View](/assets/img/OWASP/17.png){: width="700" height="400" }

What version of Alpine Linux is running?

![Desktop View](/assets/img/OWASP/18.png){: width="700" height="400" }

## 4. Insecure Design

Insecure design refers to vulnerabilities which are inherent to the application's architecture. They are not vulnerabilities regarding bad implementations or configurations, but the idea behind the whole application (or a part of it) is flawed from the start. Most of the time, these vulnerabilities occur when an improper threat modelling is made during the planning phases of the application and propagate all the way up to your final app. Some other times, insecure design vulnerabilities may also be introduced by developers while adding some "shortcuts" around the code to make their testing easier. A developer could, for example, disable the OTP validation in the development phases to quickly test the rest of the app without manually inputting a code at each login but forget to re-enable it when sending the application to production.

### Challenge

Navigate to `http://10.10.51.0:85` and get into joseph's account. This application also has a design flaw in its password reset mechanism. Can you figure out the weakness in the proposed design and how to abuse it?

**What is the value of the flag in joseph's account?**

![Desktop View](/assets/img/OWASP/19.png){: width="700" height="400" }

![Desktop View](/assets/img/OWASP/20.png){: width="700" height="400" }

![Desktop View](/assets/img/OWASP/21.png){: width="700" height="400" }

![Desktop View](/assets/img/OWASP/22.png){: width="700" height="400" }

![Desktop View](/assets/img/OWASP/23.png){: width="700" height="400" }

---

## 5. Security Misconfiguration

Security Misconfigurations are distinct from the other Top 10 vulnerabilities because they occur when security could have been appropriately configured but was not. Even if you download the latest up-to-date software, poor configurations could make your installation vulnerable.

Security misconfigurations include:

- Poorly configured permissions on cloud services, like S3 buckets.

- Having unnecessary features enabled, like services, pages, accounts or privileges.

- Default accounts with unchanged passwords.

- Error messages that are overly detailed and allow attackers to find out more about the system.

- Not using HTTP security headers.

- This vulnerability can often lead to more vulnerabilities, such as default credentials giving you access to sensitive data, XML External Entities (XXE) or command injection on admin pages.

For more info, look at the OWASP top 10 entry for Security Misconfiguration.

Debugging Interfaces

A common security misconfiguration concerns the exposure of debugging features in production software. Debugging features are often available in programming frameworks to allow the developers to access advanced functionality that is useful for debugging an application while it's being developed. Attackers could abuse some of those debug functionalities if somehow, the developers forgot to disable them before publishing their applications.

### Challenge

This VM showcases a Security Misconfiguration as part of the OWASP Top 10 Vulnerabilities list.

Navigate to `http://10.10.51.0:86` and try to exploit the security misconfiguration to read the application's source code.

![Desktop View](/assets/img/OWASP/24.png){: width="700" height="400" }

![Desktop View](/assets/img/OWASP/25.png){: width="700" height="400" }

Use the Werkzeug console to run the following Python code to execute the ls -l command on the server:

```Text
import os; print(os.popen("ls -l").read())
```

**What is the database file name (the one with the .db extension) in the current directory?**

![Desktop View](/assets/img/OWASP/26.png){: width="700" height="400" }

**Modify the code to read the contents of the app.py file, which contains the application's source code. What is the value of the secret_flag variable in the source code?**

![Desktop View](/assets/img/OWASP/27.png){: width="700" height="400" }

---

##  6. Vulnerable and Outdated Components

Occasionally, you may find that the company/entity you're pen-testing is using a program with a well-known vulnerability.

For example, let's say that a company hasn't updated their version of WordPress for a few years, and using a tool such as WPScan, you find that it's version 4.6. Some quick research will reveal that WordPress 4.6 is vulnerable to an unauthenticated remote code execution(RCE) exploit, and even better, you can find an exploit already made on Exploit-DB.

As you can see, this would be quite devastating because it requires very little work on the attacker's part. Since the vulnerability is already well known, someone else has likely made an exploit for the vulnerability already. The situation worsens when you realise that it's really easy for this to happen. If a company misses a single update for a program they use, it could be vulnerable to any number of attacks.

### Challenge 

**What is the content of the /opt/flag.txt file?**

![Desktop View](/assets/img/OWASP/28.png){: width="700" height="400" }

Looking at the site we learn that this is a online bookstore. In command prompt in Kali I ran the searchsploit command to find an exploit for online book store.

![Desktop View](/assets/img/OWASP/29.png){: width="700" height="400" }

![Desktop View](/assets/img/OWASP/30.png){: width="700" height="400" }

Next I ran `searchsploit -m php/webapps/47887.py` to download to my machine

![Desktop View](/assets/img/OWASP/31.png){: width="700" height="400" }

![Desktop View](/assets/img/OWASP/32.png){: width="700" height="400" }

---

## 7.  Identification and Authentication Failures

Authentication and session management constitute core components of modern web applications. Authentication allows users to gain access to web applications by verifying their identities. The most common form of authentication is using a username and password mechanism. A user would enter these credentials, and the server would verify them. The server would then provide the users' browser with a session cookie if they are correct. A session cookie is needed because web servers use HTTP(S) to communicate, which is stateless. Attaching session cookies means the server will know who is sending what data. The server can then keep track of users' actions. 

If an attacker is able to find flaws in an authentication mechanism, they might successfully gain access to other users' accounts. This would allow the attacker to access sensitive data (depending on the purpose of the application). Some common flaws in authentication mechanisms include the following:

- **Brute force attacks**: If a web application uses usernames and passwords, an attacker can try to launch brute force attacks that allow them to guess the username and passwords using multiple authentication attempts. 

- **Use of weak credentials**: Web applications should set strong password policies. If applications allow users to set passwords such as "password1" or common passwords, an attacker can easily guess them and access user accounts.

- **Weak Session Cookies**: Session cookies are how the server keeps track of users. If session cookies contain predictable values, attackers can set their own session cookies and access users' accounts. 
There can be various mitigation for broken authentication mechanisms depending on the exact flaw:

- To avoid password-guessing attacks, ensure the application enforces a strong password policy. 

- To avoid brute force attacks, ensure that the application enforces an automatic lockout after a certain number of attempts. This would prevent an attacker from launching more brute-force attacks.

- Implement Multi-Factor Authentication. If a user has multiple authentication methods, for example, using a username and password and receiving a code on their mobile device, it would be difficult for an attacker to get both the password and the code to access the account.

For this example, we'll look at a logic flaw within the authentication mechanism.

Many times, what happens is that developers forget to sanitise the input(username & password) given by the user in the code of their application, which can make them vulnerable to attacks like SQL injection. However, we will focus on a vulnerability that happens because of a developer's mistake but is very easy to exploit, i.e. re-registration of an existing user.

Let's understand this with the help of an example, say there is an existing user with the name `admin`, and we want access to their account, so what we can do is try to re-register that username but with slight modification. We will enter " admin" without the quotes (notice the space at the start). Now when you enter that in the username field and enter other required information like email id or password and submit that data, it will register a new user, but that user will have the same right as the `admin` account. That new user will also be able to see all the content presented under the user `admin`.

### Challenge

To see this in action, go to `http://10.10.202.133:8088` and try to register with `darren` as your username. You'll see that the user already exists, so try to register " darren" instead, and you'll see that you are now logged in and can see the content present only in darren's account, which in our case, is the flag that you need to retrieve.

**What is the flag that you found in darren's account?**

![Desktop View](/assets/img/OWASP/33.png){: width="700" height="400" }

![Desktop View](/assets/img/OWASP/34.png){: width="700" height="400" }

![Desktop View](/assets/img/OWASP/35.png){: width="700" height="400" }

![Desktop View](/assets/img/OWASP/36.png){: width="700" height="400" }

**Now try to do the same trick and see if you can log in as arthur.**

**What is the flag that you found in arthur's account?**

![Desktop View](/assets/img/OWASP/37.png){: width="700" height="400" }

## 8. Software nad Data Integrity Failures

**What is Integrity?**

When talking about integrity, we refer to the capacity we have to ascertain that a piece of data remains unmodified. Integrity is essential in cybersecurity as we care about maintaining important data free from unwanted or malicious modifications. For example, say you are downloading the latest installer for an application. How can you be sure that while downloading it, it wasn't modified in transit or somehow got damaged by a transmission error?

To overcome this problem, you will often see a hash sent alongside the file so that you can prove that the file you downloaded kept its integrity and wasn't modified in transit. A hash or digest is simply a number that results from applying a specific algorithm over a piece of data. When reading about hashing algorithms, you will often read about MD5, SHA1, SHA256 or many others available.

### Software Integrity Failures

Suppose you have a website that uses third-party libraries that are stored in some external servers that are out of your control. While this may sound a bit strange, this is actually a somewhat common practice. Take as an example jQuery, a commonly used javascript library. If you want, you can include jQuery in your website directly from their servers without actually downloading it by including the following line in the HTML code of your website:

`<script src="https://code.jquery.com/jquery-3.6.1.min.js"></script>`
When a user navigates to your website, its browser will read its HTML code and download jQuery from the specified external source.

![Desktop View](/assets/img/OWASP/43.png){: width="700" height="400" }
JS without integrity checks

The problem is that if an attacker somehow hacks into the jQuery official repository, they could change the contents of `https://code.jquery.com/jquery-3.6.1.min.js` to inject malicious code. As a result, anyone visiting your website would now pull the malicious code and execute it into their browsers unknowingly. This is a software integrity failure as your website makes no checks against the third-party library to see if it has changed. Modern browsers allow you to specify a hash along the library's URL so that the library code is executed only if the hash of the downloaded file matches the expected value. This security mechanism is called Subresource Integrity (SRI), and you can read more about it here.

The correct way to insert the library in your HTML code would be to use SRI and include an integrity hash so that if somehow an attacker is able to modify the library, any client navigating through your website won't execute the modified version. Here's how that should look in HTML:

`<script src="https://code.jquery.com/jquery-3.6.1.min.js" integrity="sha256-o88AwQnZB+VDvE9tvIXrMQaPlFFSUTR+nldQm1LuPXQ=" crossorigin="anonymous"></script>`
You can go to `https://www.srihash.org/` to generate hashes for any library if needed.

### Challenge 

![Desktop View](/assets/img/OWASP/38.png){: width="700" height="400" }

### Data Integrity Failures

Let's think of how web applications maintain sessions. Usually, when a user logs into an application, they will be assigned some sort of session token that will need to be saved on the browser for as long as the session lasts. This token will be repeated on each subsequent request so that the web application knows who we are. These session tokens can come in many forms but are usually assigned via cookies. Cookies are key-value pairs that a web application will store on the user's browser and that will be automatically repeated on each request to the website that issued them.

For example, if you were creating a webmail application, you could assign a cookie to each user after logging in that contains their username. In subsequent requests, your browser would always send your username in the cookie so that your web application knows what user is connecting. This would be a terrible idea security-wise because, as we mentioned, cookies are stored on the user's browser, so if the user tampers with the cookie and changes the username, they could potentially impersonate someone else and read their emails! This application would suffer from a data integrity failure, as it trusts data that an attacker can tamper with.

One solution to this is to use some integrity mechanism to guarantee that the cookie hasn't been altered by the user. To avoid re-inventing the wheel, we could use some token implementations that allow you to do this and deal with all of the cryptography to provide proof of integrity without you having to bother with it. One such implementation is **JSON Web Tokens (JWT)**.

JWTs are very simple tokens that allow you to store key-value pairs on a token that provides integrity as part of the token. The idea is that you can generate tokens that you can give your users with the certainty that they won't be able to alter the key-value pairs and pass the integrity check. The structure of a JWT token is formed of 3 parts:

![Desktop View](/assets/img/OWASP/39.png){: width="700" height="400" }

- Header
- Payload
- Signature

The header contains metadata indicating this is a JWT, and the signing algorithm in use is HS256. The payload contains the key-value pairs with the data that the web application wants the client to store. The signature is similar to a hash, taken to verify the payload's integrity. If you change the payload, the web application can verify that the signature won't match the payload and know that you tampered with the JWT. Unlike a simple hash, this signature involves the use of a secret key held by the server only, which means that if you change the payload, you won't be able to generate the matching signature unless you know the secret key.

Notice that each of the 3 parts of the token is simply plaintext encoded with base64. You can use this online tool `https://appdevtools.com/base64-encoder-decoder` to encode/decode base64. Try decoding the header and payload of the following token:

`eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJ1c2VybmFtZSI6Imd1ZXN0IiwiZXhwIjoxNjY1MDc2ODM2fQ.C8Z3gJ7wPgVLvEUonaieJWBJBYt5xOph2CpIhlxqdUw`

Note: The signature contains binary data, so even if you decode it, you won't be able to make much sense of it anyways.

#### JWT and the None Algorithm

A data integrity failure vulnerability was present on some libraries implementing JWTs a while ago. As we have seen, JWT implements a signature to validate the integrity of the payload data. The vulnerable libraries allowed attackers to bypass the signature validation by changing the two following things in a JWT:

1. Modify the header section of the token so that the alg header would contain the value none.

2. Remove the signature part.

Taking the JWT from before as an example, if we wanted to change the payload so that the username becomes "admin" and no signature check is done, we would have to decode the header and payload, modify them as needed, and encode them back. Notice how we removed the signature part but kept the dot at the end.

![Desktop View](/assets/img/OWASP/40.png){: width="700" height="400" }

### Challenge 

Navigate to `http://10.10.202.133:8089/` and follow the instructions in the questions below.

**Try logging into the application as guest. What is guest's account password?**



---

If your login was successful, you should now have a JWT stored as a cookie in your browser. Press F12 to bring out the Developer Tools.

Depending on your browser, you will be able to edit cookies from the following tabs:

**Firefox**

![Desktop View](/assets/img/OWASP/41.png){: width="700" height="400" }

**Chrome**

![Desktop View](/assets/img/OWASP/42.png){: width="700" height="400" }

**What is the name of the website's cookie containing a JWT token?**



**Use the knowledge gained in this task to modify the JWT token so that the application thinks you are the user "admin".**



**What is the flag presented to the admin user?**