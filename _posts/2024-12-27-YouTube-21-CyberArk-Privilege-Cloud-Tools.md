---
title: "#21 - CyberArk Privilege Cloud Tools"
date: 2024-12-27 9:10:10 +1100
categories: [CyberArk Privilege Cloud]
tags: [cyberark,cyberarkmarketplace,privilegecloud,powershell,scripts]     # TAG names should always be lowercase
---

This video explores handy scripts bundled with the CyberArk Privilege Cloud Tools package found in the CyberArk Marketplace.

[![Video Preview](https://i.ytimg.com/vi/v_rMFGGP3Jk/maxresdefault.jpg)](https://www.youtube.com/watch?v=v_rMFGGP3Jk)

## CyberArk Marketplace
Download [CyberArk Privilege Cloud Tools](https://community.cyberark.com/marketplace/s/#a352J000000GWAZQA4-a392J000002tNgLQAU) found at the CyberArk Marketplace.

## Orphaned Safes
```powershell
Get-OrphanedSafes.ps1 -PortalURL https://<subdomain>.cyberark.cloud
```
## User Types and Login Activity
```powershell
Get-UserTypesAndUsersLoginActivity.ps1 -PortalURL https://<subdomain>.cyberark.cloud
```
## Sync Identiity Roles With Vault Users
```powershell
SyncIdentityRolesWithVaultUsers.ps1 -PortalURL https://<subdomain>.cyberark.cloud
```
## Delete Users
```powershell
DeleteUsersFromVault.ps1 -PortalURL https://<subdomain>.cyberark.cloud
```
# Timeline
- Intro 0:00
