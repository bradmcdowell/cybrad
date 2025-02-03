---
title: "#9 - CyberArk Privilege Cloud | PSM Health Check & Load Balancing"
date: 2023-12-06 10:10:10 +1100
categories: [Privilege Cloud,PSM,PSMP,Health Check]
tags: [cyberark,privilegecloud,psm,psmp,haproxy]     # TAG names should always be lowercase
---

This video covers the PSM Health check.

[![Video Preview](https://i.ytimg.com/vi/SeP7JISnF-s/maxresdefault.jpg)](https://www.youtube.com/watch?v=SeP7JISnF-s)

## Objectives
- Configure PSM Health Check
- Test PSM Health Check with PowerShell
- Configure PSM Load Balancing in Privilege Cloud Portal
- Configuration overview for HAProxy Load Balancer
- Test PSM Health Check with HAProxy Load Balancer
- Test PSMP Load Balancing via HAProxy


### PSM Health Check Links
[Deploy PSM Health Check](https://docs.cyberark.com/PrivCloud-SS/Latest/en/Content/Privilege%20Cloud/privCloud-psm-health-check.htm)

[PSM Health Check Installation](https://cyberark.my.site.com/s/article/PSM-Health-Check-Installation)

[Download .NET 8.0 Hosting Bundle](https://dotnet.microsoft.com/en-us/download/dotnet/8.0)

[CyberArk MarketPlace - PSM Health check](https://cyberark.my.site.com/mplace/s/#a352J000000ai0MQAQ-a392J000002QBA6QAO)

### HAProxy Configuration

HAProxy config file /etc/haproxy/haproxy.cfg

``` yml
global
        log /dev/log    local0
        log /dev/log    local1 notice
        chroot /var/lib/haproxy
        stats socket /run/haproxy/admin.sock mode 660 level admin expose-fd listeners
        stats timeout 30s
        user haproxy
        group haproxy
        daemon

defaults
        log     global
        mode    http
        option  httplog
        option  dontlognull
        option log-health-checks
        timeout connect 5000
        timeout client  50000
        timeout server  50000
        errorfile 400 /etc/haproxy/errors/400.http
        errorfile 403 /etc/haproxy/errors/403.http
        errorfile 408 /etc/haproxy/errors/408.http
        errorfile 500 /etc/haproxy/errors/500.http
        errorfile 502 /etc/haproxy/errors/502.http
        errorfile 503 /etc/haproxy/errors/503.http
        errorfile 504 /etc/haproxy/errors/504.http

# Cyberark Windows PSM Load Balancer
frontend CyberArk_PSM_3389_frontend
    bind *:3389
    mode tcp
    option tcplog
    default_backend CyberArk_PSM_3389_backend

backend CyberArk_PSM_3389_backend
    mode tcp
    balance leastconn
    option httpchk
    http-check connect port 80
    http-check send meth GET uri /psm/api/health
    http-check expect string PASS
    # Define your CyberArk PSM servers here
    server PSM_con1 con1.cybrad.au:3389 check inter 4s fall 3 rise 2
    server PSM_con2 con2.cybrad.au:3389 check inter 4s fall 3 rise 2

# CyberArk UNIX PSMP Load Balancer
frontend CyberArk_PSMP_22_front
    bind *:22
    mode tcp
    option tcplog
    default_backend CyberArk_PSMP_22_back

backend CyberArk_PSMP_22_back
    mode tcp
    balance leastconn
    option tcp-check
    # Define your CyberArk PSMP servers here
    server PSMP1 psmp1.cybrad.au:22 check inter 4s fall 3 rise 2
    server PSMP2 psmp2.cybrad.au:22 check inter 4s fall 3 rise 2

listen stats
  bind :8888
  mode http
  stats enable
  stats hide-version
  stats realm Haproxy\ Statistics
  stats uri /  # Choose a URL path for the stats interface
  stats refresh 5s
```


# Timeline
- Intro 0:00
- Lab Overview 1:02
- Documentation 1:25
- Install PSM Health Check on CON1 2:02
- Test PSM Health Check and fix cert issue 4:13
- Install PSM Health Check on CON2 6:00
- Set Health Check to listen on port 80 6:55
- Review Load Balancer config in Privilege Cloud Portal 8:14
- Review HAProxy configuraiton 9:20
- Test PSM Health Check with HAProxy 10:51
- Test PSM Load Balancer vi HTML5 Gateway 12:05
- Test PSMP Load Balancing via HAProxy 13:04