---
title: "#7 - CyberArk Privilege Cloud - Shared Services | Palo Alto Networks PAN-OS"
date: 2023-09-20 10:10:10 +1100
categories: [Privilege Cloud,CPM,PSM,Palo Alto]
tags: [cyberark,privilegecloud,cpm,psm,paloalto,panos]     # TAG names should always be lowercase
---

This video covers the Palo Alto platform.

[<img src="https://i.ytimg.com/vi/Si8MTsSMoTg/maxresdefault.jpg" width="50%">](https://www.youtube.com/watch?v=Si8MTsSMoTg)
## Objectives
- Import PAN-OS CPM Platform 
- CPM: PAN-OS PSM Connection Component
- CPM: Manage Palo Alto PAN-OS local accounts
- PSM: Connect to PAN-OS using local accounts for both SSH and Web
- PSM: Connect to PAN-OS using Active-Directory managed accounts for both SSH and Web
- PSMP: Connect to PAN-OS using local accounts via SSH

## 

ClientApp

```
"putty.exe" -ssh "{UserName}"@"{PSMRemoteMachine}" -pw "{Password}"
```

### PSM-SSH-PaloAlto-Domain

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

# Timeline:
- Intro 0:00
- Test 1:00
