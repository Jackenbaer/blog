---
layout: post
title:  "Thoughts about YubiKeys in SSH Authentication"
date:   2023-05-01 16:16:23 +0200
categories: ssh yubikey
---



---
# Table of Contents
- [General Yubikey Information] (#background)
- [Yubikeys and SSH Authentication] (#authentication)
- [Yubikey PIV Authentication vs. Standard Public Key Authentication] (#critic)
- [Summary] (#summary)


---
<a name="background"></a>
# What is a YubiKey?
A YubiKey is a hardware authentication device that enhances security by storing cryptographic keys and supporting multiple authentication protocols. It connects via USB or NFC and requires physical access to the device to authenticate, adding an extra layer of protection. Using a YubiKey provides an additional layer of security, as the private key never leaves the device, making it less susceptible to theft or unauthorized access.

---
<a name="authentication"></a>
# How is it used for ssh authentication?
There are two main ways to authenticate using a yubikey. You can use FIDO2 Authentication (an blog post will follow) or use the Yubikey as a PIV Smartcard. We will use the Yubikey as a smartcard and follow the steps at the [official website](https://developers.yubico.com/PIV/Guides/SSH_user_certificates.html).
The SSH Authentication Challenge will be signed by the Yubikey using the ssh-agent. 


---
<a name="critic"></a>

# What is the benefit of a Yubikey? 
To compare SSH authentication using YubiKeys as a PIV Smartcard with standard public key authentication, we will explore attack models in which both authentication methods perform differently.

The most notable example occurs when your own system is compromised, or when you need to log in to a server from an untrusted device. Let's examine how this impacts some security aspects.

## Standard Public Key Authentication
A compromised system could result in a cleartext copy of the private key, even if it is password-protected (as the attacker could deploy a keylogger). The attacker would then be able to log in to all current and future remote systems, independently of the infected system. He is also able to login again after he logged out without installing a backdoor. 

## Yubikey PIV Smartcard Authentication

The YubiKey protects the private key but offers an interface through the ssh-agent that can be exploited by attackers to sign SSH authentication challenges. I have uploaded a brief Python demo to [my Git repository](https://github.com/jackenbaer/remote-ssh-challenge-demo) to illustrate this type of attack.

An attacker could use this access to the ssh-agent to gain entry to all current remote systems. Since the attacker does not yet know the SSH authentication challenges for future remote systems, they remain secure for the time being.

Additionally, the attacker must install a backdoor or maintain an active session on remote systems, as they cannot log in without the assistance of the infected system with the YubiKey.



---
<a name="summary"></a>
# Summary 
The YubiKey protects your private key. It doesn't protect your remote system if you connect your YubiKey to an untrusted device. Nethertheless there are benefits (future systems stay protected if an attacker had only access for a limited time) compared to classical publickey authentication and there are no clear disadvantages. 