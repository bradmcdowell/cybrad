---
title: "#10 - CyberArk Privilege Cloud | Windows Proxy Configuration"
date: 2023-12-13 10:10:10 +1100
categories: [Privilege Cloud,PSM,CPM,Secure Tunne,Identity Connector,squid proxy]
tags: [cyberark,privilegecloud,psm,cpm,squidproxy]     # TAG names should always be lowercase
---

> ##### WARNING
>
> Be Aware That This Is Currently Under Controlled Availability. For This To Be Supported You Need To Get Written Approval From PM, Contact Your Account Executive To Start This Process.
> If You Do Not, CyberArk Can't Guarantee Support.
{: .block-warning }

This video covers the Windows connector install with a proxy configuraiton.
<!---
[<img src="https://i.ytimg.com/vi/lbRTorlbV5s/maxresdefault.jpg" width="50%">](https://www.youtube.com/watch?v=lbRTorlbV5s)
--->

[![Video Preview](https://i.ytimg.com/vi/lbRTorlbV5s/maxresdefault.jpg)](https://www.youtube.com/watch?v=lbRTorlbV5s)

## Objectives
- Proxy / Network Requirements
- Pre-Requisites checks and tests
- Install CPM and PSM v14.0 using web proxy
- Install and configure Secure Tunnel v3.1 using web proxy
- Install and configure Identity Connector using web proxy
- Configure Group Policy hardening
- Test CPM, PSM, Secure Tunnel and Identity connector


### Links

[Proxy Configuration in Privilege Cloud](https://cyberark.my.site.com/s/article/Proxy-Configuration-in-Privilege-Cloud)


#### Curl Proxy testing commands

```
.\curl -v https://cybrad.cyberark.cloud --proxy http://webproxy.cybrad.au:3128
.\curl -v https://connector-cybrad.privilegecloud.cyberark.cloud --proxy http://webproxy.cybrad.au:3128
.\curl -v https://console.privilegecloud.cyberark.cloud --proxy http://webproxy.cybrad.au:3128
```

#### .NET machine.config

```
notepad C:\Windows\Microsoft.NET\Framework\v4.0.30319\Config\machine.config
notepad C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config\machine.config
```

# Timeline
- Intro 0:00
- Lab Overview 1:08
- Proxy Setup 2:40
- Documentaiton 4:10
- Connectivity Testing 4:50
- Install CPM and PSM 10:08
- PSM Hardening 16:00
- PSM registration 18:55
- Secure Tunnel Install 20:56
- PSM Certificate 22:59
- Identity Connector 24:50
- Privilege Cloud Settings 25:53
- Group Policy Hardening 27:18
- Test CPM, PSM and Identity 29:02