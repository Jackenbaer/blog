---
layout: post
title: "Replace IP in BSD Loopback PCAP"
date:   2022-06-05 16:16:23 +0200
categories: freebsd pcap
---


# Problem description
BSD loopback header is not supported by tcprewrite if you want to replace ip adresses. Executing tcprewrite overwrites parts of the ip header.

# Solution summary
Change DLT and add ethernet header to each packet.


# Setup 
| Software | Version |
|:--|:--|
| FreeBSD | 13.0-RELEASE-p11 |
| tcprewrite | 4.4.1 |
| tcpprep | 4.4.1 |
| tcpdump | 4.9.3 | 


# Installation
```
pkg install -y tcpreplay 
```

[Offical website of tcpwrite](https://tcpreplay.appneta.com/wiki/tcprewrite)



## Adding Ethernet EN10MB headers

![image](/blog/assets/images/replace_ip_bsd_loopback_original.png)


Insert bytes:
```
tcprewrite --dlt=user --user-dlink=01,02,03,04,05,06,1A,2B,3C,4D,5E,6F,08,00 --infile=orig.pcap --outfile=tmp.pcap
```

Change dtl:
```
tcprewrite --dlt=enet --infile=tmp.pcap --outfile=enet.pcap
```

![image](/blog/assets/images/replace_ip_bsd_loopback_enet.png)



## Replacing ip addresses 
Create the cachefile: 
```
tcpprep --port --pcap=enet.pcap --cachefile=in.cache
```

Replace ip addresses:
```
tcprewrite --cachefile=in.cache --endpoints=1.1.1.1:2.2.2.2 --infile=enet.pcap --outfile=replaced.pcap
```

Maybe you have to recalc ethernet checksums.

# Check the result
```
tcpdump -r replaced.pacp
```



