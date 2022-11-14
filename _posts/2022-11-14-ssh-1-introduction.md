---
layout: post
title:  "SSH - Introduction"
date:   2022-11-14 16:16:23 +0200
categories: ssh 
---

# Overview SSH 
This is the start of a guide through the ssh protocol especially the [openssh](https://www.openssh.com) implementation. 
It's going to be a mix of explained RFC standards, parts of the [source code](https://github.com/openssh) as well as easier to understand reimplementations like [paramiko](https://www.paramiko.org) and pcap analysis. 


---
# Table of Contents
- [Password vs. Public Key Authentication](https://jackenbaer.de/ssh/2022/11/08/ssh-2-password-vs-pubkey-auth.html)

---
# Overview RFC standards

| Summary | RFC Standards (*main*, update, ...) |
|:--|:--|
| Authentication Protocol | *[RFC4252](https://datatracker.ietf.org/doc/html/rfc4252)*, [RFC8308](https://datatracker.ietf.org/doc/html/rfc8308), [RFC8332](https://datatracker.ietf.org/doc/html/rfc8332) |
| Connection Protocol | *[RFC4254](https://www.rfc-editor.org/rfc/rfc4254)*, [RFC8308](https://www.rfc-editor.org/rfc/rfc8308)|
| Transport Layer Protocol | *[RFC4253](https://datatracker.ietf.org/doc/html/rfc4253)*, [RFC6668](https://datatracker.ietf.org/doc/html/rfc6668), [RFC8268](https://datatracker.ietf.org/doc/html/rfc8268), [RFC8308](https://datatracker.ietf.org/doc/html/rfc8308), [RFC8332](https://datatracker.ietf.org/doc/html/rfc8332),[RFC8709](https://datatracker.ietf.org/doc/html/rfc8709), [RFC8758](https://datatracker.ietf.org/doc/html/rfc8758), [RFC9142](https://datatracker.ietf.org/doc/html/rfc9142) |

