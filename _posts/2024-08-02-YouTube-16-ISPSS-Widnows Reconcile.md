---
title: "#16 - CyberArk Windows Reconcile"
date: 2024-08-02 10:10:10 +1100
categories: [Privilege Cloud,CPM]
tags: [cyberark,privilegecloud,CPM]     # TAG names should always be lowercase
---

This video covers the process setting up reconcile accounts for CyberArk.

[![Video Preview](https://i.ytimg.com/vi/ITA8Wza_Y98/maxresdefault.jpg)](https://www.youtube.com/watch?v=ITA8Wza_Y98)

## Objectives
- Create Windows Domain Reconcile Accounts and Set Permissions.
- Configure Windows Domain Platforms in Privilege Cloud.
- Onboard Windows Domain Admin/Enterprise and Server Admin.
- Create Windows Server Reconcile Account.
- Onboard Server localadmin account.
- Additional Information – What to look out for and what not to do.

[CyberArk KB - How to Reconcile a Domain Admin Account Without Domain Admin Membership for the Reconcile Account](https://community.cyberark.com/s/article/How-to-Reconcile-a-Domain-Admin-Account-Without-Domain-Admin-Membership-for-the-Reconcile-Account)

## Active-Directory CyberArk Reconcile Permissions Script

``` powershell
# This script will set the minimum permissions for the reconcile account.

# Set the AD account that will be used fo Reconcile
$ReconcileAccount = "<Domain>\<ReconcileUser>"

# This gets the AD domain in DN format
$DistinguishedName = (Get-ADDomain).DistinguishedName
Write-host "Domain OU is =" $DistinguishedName

# Domain Permissions
dsacls.exe $DistinguishedName /i:S /G $ReconcileAccount":CA;Reset Password;user"
dsacls.exe $DistinguishedName /i:S /G $ReconcileAccount":WD"
dsacls.exe $DistinguishedName /i:S /G $ReconcileAccount":WPRP;LockoutTime;user"
dsacls.exe $DistinguishedName /i:S /G $ReconcileAccount":WPRP;account restrictions;user"

# This gets the AdminSDHolder DN
$AdminSDHolerDN = "CN=AdminSDHolder,CN=System,"+$DistinguishedName
Write-host "AdminSDHolder DN is =" $AdminSDHolerDN

# AdminSDHolder Template
dsacls.exe $AdminSDHolerDN /G $ReconcileAccount":CA;Reset Password"
dsacls.exe $AdminSDHolerDN /G $ReconcileAccount":WD"
dsacls.exe $AdminSDHolerDN /G $ReconcileAccount":RP;LockoutTime"
dsacls.exe $AdminSDHolerDN /G $ReconcileAccount":WP;LockoutTime"
dsacls.exe $AdminSDHolerDN /G $ReconcileAccount":RP;account restrictions"
dsacls.exe $AdminSDHolerDN /G $ReconcileAccount":WP;account restrictions"
```

# Extra Information on permission inheritance
Clear adminCount and Enable Inheritance on User
https://notesbytom.wordpress.com/2017/12/01/clear-admincount-and-enable-inheritance-on-user/

# Timeline
- Intro 0:00
- Create Domain Reconcile Acccounts 1:25
- Set Domain Reconcile Permissions 3:25
- Group Policy Consideratioins 4:44
- Cyberark Reconcile Safe and Platform Setup 6:20
- Onboard Widnows Domain Reconcile Accounts 9:58
- Test Reconcile Accounts 12:35
- Create Windows Server Reconcile Account 14:52
- Setup Windows Server Reconcile Account 16:14
- Test Onborad Windows Server Local Admin 19:51
- What to look out for 21:50