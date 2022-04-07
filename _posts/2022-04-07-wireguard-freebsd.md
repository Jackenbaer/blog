---
layout: post
title:  "Wireguard VPN server on FreeBSD 13.0"
date:   2022-04-07 16:16:22 +0200
categories: jekyll update
---

# Wireguard VPN server on FreeBSD 13.0

## Overview
- [Description] (#description)
- [Install wireguard] (#install_wireguard)
- [Generate Keys] (#generate_keys)
- [Edit pf] (#edit_pf)
- [Debugging] (#debugging)



<a name="description"></a>
### Description 
This tutorial shows you, how to setup a wireguard VPN server using pf on FreeBSD.

Tested with: 
```
wireguard-2,1
FreeBSD 13.0-RELEASE-p3
```



<a name="install_wireguard"></a>
### Install wireguard
```
‌pkg install -y wireguard
sysrc wireguard_enable=YES
sysrc gateway_enabled=YES
sysrc wireguard_interfaces=wg0
sysctl net.inet.ip.forwarding=1
service wireguard start
```





<a name="generate_keys"></a>
### Generate keys
The following steps are based on the [official documentation](https://www.wireguard.com/quickstart/).


```
umask 077
```


Generate server keys:
```
wg genkey > server.priv
wg pubkey < server.priv > server.pub
```

Generate client keys:
```
wg genkey > client0.priv
wg pubkey < client0.priv > client0.pub
```


server.conf

```
/usr/local/etc/wireguard/wg0.conf
```


```
[Interface]
Address = 192.168.10.1/32
PrivateKey = SERVER_PRIVATEKEY
ListenPort = 51820

[Peer] #client0
PublicKey = CLIENT0_PUBLICKEY
AllowedIPs = 192.168.10.2/32

#[Peer] #client1
#PublicKey = CLIENT1_PUBLICKEY
#AllowedIPs = 192.168.10.3/32

```





client0.conf

```
[Interface]
PrivateKey = CLIENT0_PRIVATEKEY
Address = 192.168.10.2/32
DNS = 8.8.8.8

[Peer]
PublicKey = SERVER_PUBLICKEY
AllowedIPs = 0.0.0.0/0, ::/0
Endpoint = SERVER_IP:SERVER_PORT
```






<a name="edit_pf"></a>
### Edit pf
Before we start editing our pf.conf here are some basic advices for working with pf, because you easily get locked out by making a mistake.

Check your rules with the -n flag before loading them.
Make sure a fallback pf will be loaded if you made a mistake. 

Something like this does the job:
```
pfctl -nf /etc/pf_wireguard.conf && \
pfctl -f /etc/pf_wireguard.conf && \
sleep 600 && \
pfctl -f /etc/pf_fallback.conf
```
 


Simple pf.conf

```
ext_if="vtnet0"
wireguard_if="wg0"

tcp_services = "{ 22 }"
udp_services = "{ 51820 }"

scrub in on $ext_if all fragment reassemble
set skip on lo

# Wireguard Settings
wireguard_net_v4 = "192.168.10.1/24"

nat on $ext_if inet  from $wireguard_net_v4 to { any, !$wireguard_net_v4 } -> ($ext_if)
pass on $wireguard_if all

block in all
antispoof for $ext_if inet
pass out quick keep state
pass in on $ext_if proto tcp to port $tcp_services
pass in on $ext_if proto udp to port $udp_services

```


<a name="debugging"></a>

### Debugging 
Okay, following an internet step by step tutorial ends up not working in almost any case. 
Here are some ideas, that might help you to fix your problem. 


#### Wireguard is running ?
```
service wireguard status
```


#### Wireguard port isn't blocked by your firewall ?
```
pfctl -sr
```

#### Correct wireguard port is open?
```
sockstat -l 
```

#### Firewall rules are loaded?
```
pfctl -sr
```


#### Natting is working as it should?

```
tcpdump -ni wg0
```

```
tcpdump -ni vtnet0 port 51820
```


#### All keys are correct?
Check them!

#### Checked client log?
Take a look!

#### Checked server log?
Take a look!
