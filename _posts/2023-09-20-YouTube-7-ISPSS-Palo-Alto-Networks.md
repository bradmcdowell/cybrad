---
title: "#7 - CyberArk Privilege Cloud | Palo Alto Networks PAN-OS"
date: 2023-09-20 10:10:10 +1100
categories: [Privilege Cloud,CPM,PSM,Palo Alto]
tags: [cyberark,privilegecloud,cpm,psm,paloalto,panos]     # TAG names should always be lowercase
---

This video covers the Palo Alto PAN-OS platform.
<!---
[<img src="https://i.ytimg.com/vi/Si8MTsSMoTg/maxresdefault.jpg" width="50%">](https://www.youtube.com/watch?v=Si8MTsSMoTg)
--->
[![Video Preview](https://i.ytimg.com/vi/Si8MTsSMoTg/maxresdefault.jpg)](https://www.youtube.com/watch?v=Si8MTsSMoTg)

## Objectives
- Import PAN-OS CPM Platform 
- Import PAN-OS PSM Connection Component
- CPM: Manage Palo Alto PAN-OS local accounts
- PSM: Connect to PAN-OS using local accounts for both SSH and Web
- PSM: Connect to PAN-OS using Active-Directory managed accounts for both SSH and Web
- PSMP: Connect to PAN-OS using local accounts via SSH

### CyberArk Marketplace Links
[Palo Alto Networks PAN-OS - CPM](https://cyberark.my.site.com/mplace/s/#a352J000000WUKgQAO-a392J0000013eW1QAI)

[Palo Alto Networks PAN-OS - PSM](https://cyberark.my.site.com/mplace/s/#a352J000000WUOOQA4-a392J0000013eXJQAY)
### PSM-PaloAltoWeb
#### WebFormFields
Target Settings -> Web Form Settings -> WebFormFields
```
user>{Username}
passwd>{Password}
submit>(Button)
ext-gen60 > (Validation)
```
### PSM-SSH-PaloAlto-Domain

#### PSMRemoteMachine
User Parameters
-> Create Parameter
Name: 
```
PSMRemoteMachine
```
DisplayName: 
```
Firewall Hostname
```
Type:
```
CyberArk.PasswordVault.Web.TransparentConnection.RemoteMachineUserParameter, CyberArk.PasswordVault.Web
```

#### ClientApp
Target Settings -> Client Specific
-> Create Parameter
Name:
```
ClientApp
```
Value:
```
"putty.exe" -ssh "{UserName}"@"{PSMRemoteMachine}" -pw "{Password}"
```

### PSMP Connection String

```
ssh brad@cybrad.au@ca.superuser@firewall.cybrad.au@psmp1cybrad.au -i "C:\Users\Brad\Downloads\key.openssh"
```
[PSM for SSH Syntax Cheat Sheet](https://cyberark.my.site.com/s/article/PSM-for-SSH-Syntax-Cheat-Sheet)
# Timeline:
- Intro 0:00
- Download Plugins form Cyberark Marketplace 00:52
- Clean Up Palo Alto Connection Components from last video 01:11
- Create Safes for Palo Alto local accounts 1:49
- Import Palo Alto PAN-OS CPM Plugin 2:34
- Duplicate Palo Alto PAN-OS CPM Platform 2:49
- Onboard Palo Alto PAN-OS local accoutns 3:19
- Configure Palo Alto PAN-OS settings4:09
- Change local PAN-OS passwords 5:48
- Link reconcile accounts for local PAN-OS accounts 7:36
- Test PSM-SSH for local PAN-OS accounts 9:40
- Import Palo Alto PAN-OS PSM Connection Component  10:08
- Test local Palo Alto PAN-OS web sessions 13:00
- Validation Issue Fix 13:15
- Enable Trace 16:05
- PSM: Using Active-Directory accounts via PSM web session 17:18
- PSM: Using Active-Directory accounts via PSM SSH session 20:30
- PSMP: Using local PAN-OS Account 23:11