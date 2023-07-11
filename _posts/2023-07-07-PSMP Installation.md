---
title: PSMP Installation
date: 2023-07-07 11:10:10 +1100
categories: [Privilege Cloud,PSMP]
tags: [psmp]     # TAG names should always be lowercase
---

# PSMP Installation

#### Download PSMP Installation Script

Use the follwoing command to downlaod the psmpwiz1xxx.sh script. Make sure it is placed in the same folder as PrivilegedSessionManagerSSHProxy-RHELinux8-Intel64-Rls-v1X.X.zip

```
cd /home/<username>/PSMP/
wget https://raw.githubusercontent.com/pCloudServices/psmpwiz/main/psmpwiz1310.sh
```

#### Test Network Connectivity

The follwoing commands will test outbound connectivity from the PSMP server to Privilege Cloud.

```sh
curl -v telnet://<subdomain>.privilegecloud.cyberark.cloud:443

curl -v telnet://<subdomain>.cyberark.cloud:443

curl -v telnet://<subdomain>.privilegecloud.cyberark.cloud:1858

curl -v telnet://<CyberArkIdentityID>.id.cyberark.cloud:443
```

#### Create proxymng User and proxymanagers Group

```
useradd proxymng
passwd proxymng
groupadd proxymanagers
usermod -a -G proxymanagers proxymng
```
#### Modify SSHD Config File
```
vi /etc/ssh/sshd_config
```

Add the following line to the bottom of /etc/ssh/sshd_config
```
AllowGroups proxymanagers PSMConnectUsers
```
Restart the SSH service
```
service sshd restart
service sshd status
```

#### Change Directory to /home/localadmin/PSMP/
```
cd /home/localadmin/PSMP/
```
#### Unzip CyberArk Files
```
ls
unzip PrivilegedSessionManagerSSHProxy-RHELinux8-Intel64-Rls-v1X.X.zip
unzip psmpwiz1XX.zip
```
#### Set the following files to executable
```
ls -ltr
chmod 755 CARKpsmp-1X.xx.xx.xx.rpm
chmod 755 CreateCredFile
chmod 755 psmpwiz1XX.sh
```

#### Run the PSMP install script
```
./psmpwiz1XX.sh
```
  



PSM for SSH requirements
https://docs.cyberark.com/Product-Doc/OnlineHelp/PrivCloud/Latest/en/Content/Privilege%20Cloud/PrivCloud-sys-req-PSM-SSH.htm?tocpath=Setup%7CSystem%20requirements%7C_____3
