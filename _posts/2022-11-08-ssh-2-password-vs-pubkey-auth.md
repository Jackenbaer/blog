---
layout: post
title:  "Password vs. Public Key Authentication"
date:   2022-11-08 21:34:23 +0200
categories: ssh 
---

## Overview
What is better? Public key authentication or password authentication. To answer this question we will compare each authentication method in different scenarios. 
If an important scenario is missing text me and i will try to integrate it. While reading the following scenarios keep in mind what you expierenced during your work.

---
## Table of Contents

- [Scenario 1: Bruteforce](#scenario1)
- [Scenario 2: MITM](#scenario2)
- [Scenario 3: Infect other servers](#scenario3)
- [Scenario 4: Reinfect server](#scenario4)
- [Scenario 5: Member leaves the team](#scenario5)
- [Scenario 6: Member joins the team](#scenario6)
- [Summary](#summary)
- [Result](#result)

---
<a name="scenario1"></a>
## Scenario 1: Bruteforce Access
# Description 
An attacker tries to brute force access to a server. 

# Password Authentication
Depends on the way the password is generated. Long and random password achieve the same resistance against brute force attacks as public keys. You don't know if other users really follow your instructions. 

# Public Key Authentication
Since keys are generated with ```ssh-keygen``` each key is long and random enough by default. 

---
<a name="scenario2"></a>
## Scenario 2: Man in the middle
# Description 
A Client connects to a man in the middle server. Not very likely since the client will be warned if he tries to connect to a known server with a changed host key. Maybe relevant for initial logins.

# Password Authentication
The MITM is able to forward the password to the target. Afterwards the password is known and the attacker is able to login in to the target. 


# Public Key Authentication
Not possible because the authentication is done via a signature other a challenge. The MITM is not able to replay or intercept the authentication process.(More details for public authentication coming soon)

---
<a name="scenario3"></a>
## Scenario 3: Infect other servers from infected Server
# Description 
An attacker got root access to one server. He want to gain access to other servers.

# Password Authentication
Depends on user behavior. The attacker is able to log your password while you authenticate to the server. If a user uses this password on other servers the attacker could login to them too. It doesn't matter if the password is secure (long and random). 

# Public Key Authentication
Users are able to use the same key for multiple machines without  risking that an attacker have access to more servers. 

---
<a name="scenario4"></a>
## Scenario 4: Reinfect server after it is resetted to a recovery point before infection
# Description 
Let's say you noticed that an attacker gained root access to a virtual machine. Because the setup on this machine is complicated you simply reset the machine to a secure recovery point and  upgrade the server to the latest version. 


# Password Authentication
If you change all passwords after the reset the attacker is not able to login anymore. Otherwise the attacker could have logged passwords while he had access to the server and reinfect the server again. 

# Public Key Authentication
You don't have to change your keys. 

---
<a name="scenario5"></a>
## Scenario 5: Member leaves the team
# Description 
A team member leaves the team and you have to remove his access to some user accounts.

# Password Authentication
You have to change each shared user account on each machine and share the new password in a secure way. 

# Public Key Authentication
You simply remove the public key in ```authorized_keys``` and access should be prohibited. Other team members don't have to do anything. Maybe the team member installed something like a backdoor. But if he was that criminal, you probably have more problems you are not able to protect against. 
 
---
<a name="scenario6"></a>
## Scenario 6: Member joins the team 
# Description 
Your new team member needs access to a user. 

# Password Authentication
Because passwords are long and random you need an encrypted container to store them. Each time you connect to a server you have to look up the password. But how are passwords initially shared to new team members?
Send a clear text password other an ecrypted channel and somehow delete the message?
Share a USB Stick with a text file and delete it afterwards ?
Type a secure passwords (probably multiple times because since it is secure it will be long and there will be typers)?

# Public Key Authentication
The new team member shares his public key. Even if an attacker knows the public key, he wont gain access to the server. The private keys stays on the laptop of the new team member. 

---
<a name="summary"></a>
## Summary

| Scenario | Password Authentication | Public Key Authentication | 
|:--|:--| :--| 
| [Bruteforce](#scenario1) | Can be secure but not by default | Secure by default |
| [MITM](#scenario2) | Insecure in some scenarios but not likely | Secure by default |
| [Infect other servers](#scenario3) | Can be secure but not by default | Secure by default |
| [Reinfect server](#scenario4) | Can be secure but not by default  | Secure by default |
| [Member leaves the team](#scenario5) | Can be secure but not by default | Secure by default |
| [Member joins the team](#scenario6) | Can be secure but not by default | Secure by default |

---
<a name="result"></a>
## Result 
Password authentication can be as secure as public key authentication. But you and other users have to do a lot manually. Public key authentication achieves this high level of security by default without human interaction. Make your life easy, use public key authentication. 