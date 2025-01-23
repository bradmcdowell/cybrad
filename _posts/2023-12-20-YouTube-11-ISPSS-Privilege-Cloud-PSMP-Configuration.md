---
title: "#11 - CyberArk Privilege Cloud | PSMP Proxy Configuration"
date: 2023-12-20 10:10:10 +1100
categories: [Privilege Cloud,PSMP,squid proxy]
tags: [cyberark,privilegecloud,psmp,squidproxy]     # TAG names should always be lowercase
---

> ##### WARNING
>
> Be Aware That This Is Currently Under Controlled Availability. For This To Be Supported You Need To Get Written Approval From PM, Contact Your Account Executive To Start This Process.
> If You Do Not, CyberArk Can't Guarantee Support.
{: .block-warning }

This video covers the PSMP connector install with a proxy for internet access. If you are not using a proxy server, please refer to [this post](https://cybrad.au/posts/YouTube-4-ISPSS-PSMP) for more details.

[CyberArk KB - Proxy Configuration in Privilege Cloud](https://community.cyberark.com/s/article/Proxy-Configuration-in-Privilege-Cloud)

[![Video Preview](https://i.ytimg.com/vi/6ML-PBRgHas/maxresdefault.jpg)](https://www.youtube.com/watch?v=6ML-PBRgHas)

## Objectives
- LAB Overview
- Install PSMP v13.2 using web proxy
- Test PSMP

### PSMP Install commands

##### Copy PrivilegedSessionManagerSSHProxy-RHELinux8-Intel64-Rls-vXX.X.zip file to PSMP server

```
scp "C:\Install\CyberArk Privilege Cloud_14.0_1700448236427\Privileged Cloud\Privileged Session Manager for SSH\PrivilegedSessionManagerSSHProxy-RHELinux8-Intel64-Rls-v14.0.zip" localadmin@psmp10:~
```

### SSH to PSMP

### Edit file /etc/dnf/dnf.conf using vi

```
sudo vi /etc/dnf/dnf.conf
```
Add the following line to the bottom.

```
proxy=http://webproxy.cybrad.au:3128
```

### Install packages wget unzip and nano

```
sudo dnf install wget unzip nano
```

### Edit environment variables.

```
sudo nano /etc/environment
```
Add the follwoing lines

```
http_proxy=http://webproxy.cybrad.au:3128
https_proxy=http://webproxy.cybrad.au:3128
```

```
sudo reboot
```

### Tidy up files

```
mkdir PSMP
mv PrivilegedSessionManagerSSHProxy-RHELinux8-Intel64-Rls-v14.0.zip ./PSMP/
cd PSMP/
```

### Download psmpwiz script
```
wget https://raw.githubusercontent.com/pCloudServices/psmpwiz/main/psmpwiz1320.sh
```

### Change file permissions
```
chmod 755 CreateCredFile CARKpsmp-13.2.0.15.x86_64.rpm psmpwiz1320.sh
ls -ltr
```

### Edit vault.ini
```
nano vault.ini
```
Upddate vault address on line 2
```
ADDRESS=vault-subdomain.privilegecloud.cyberark.cloud
```
Add in proxy details
```
PROXYTYPE=HTTPS
PROXYADDRESS=webproxy.cybrad.au
PROXYPORT=3128
```

### Run psmpwiz script
```
sudo ./psmpwiz1320.sh
```

### Edit psmpsrv-psmpserver.service file
```
sudo nano /usr/lib/systemd/system/psmpsrv-psmpserver.service
```
Append the follwoing on the line that starts wint Environment=
```
"HTTPS_PROXY=webproxy.cybrad.au:3128"
```
Daemon reload and restart service
```
sudo systemctl daemon-reload
sudo service psmpsrv restart
```

#### Troubleshooting

This command dispalys a live monitor of the PSMTrace log
```
sudo tail -f /var/opt/CARKpsmp/logs/PSMPTrace.log
```

# Timeline
- Intro 0:00
- Lab Overview 0:26
- Set installer user password 0:44
- PSMP Install 1:08
- Troubleshoot PSMP 6:41
- Test PSMP 8:15