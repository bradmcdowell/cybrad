---
title: Reconcile Account
date: 2023-07-06 10:10:10 +1100
categories: [Privilege Cloud,CPM]
tags: [cyberark,reconcile]     # TAG names should always be lowercase
---

Script set permissions for Reconcile Group

Instructions below are based on [this kb article.](https://cyberark-customers.force.com/s/article/How-do-I-reconcile-a-domain-admin-account-without-granting-domain-admin-membership-to-the-reconcile-account)

To give the Reconcile Group the minimum permissions to change user passwords. 

From Active Directory (You must have the Advanced Features view enabled) to proceed Right click on the domain > Properties

Goto the Security Tab

Click Advanced

Select Add

Click "Select a principal" at the top and search fro the Reconcile Group

Applies to: Drop Down menu > Select "Descendant User objects" at the bottom.

Scroll to the very bottom and slect "Clear All" to clear all permissions

Scroll to the top and select the follwoing permissions.


```
Reset Password
Read permissions
Read account restrictions
Read general information
Read group membership
Read logon information
```

Once complete slect OK and OK again


Domain Admin accounts, along with a list of other groups, are protected.  If you change the ACL on a member of the Domain Admins group, Active Directory will eventually change the ACL back based on a secure template.  This template is AdminSDHolder and is always found in the System container.

In order to grant your reconcile account the ability to change the password for members of protected groups, you must add that permission to the AdminSDHolder template

The following script modifies the AdminSDHolder template.

``` powershell
# This script will set the minimum permissions for the reconcile account

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