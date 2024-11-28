---
title: "#19 - CyberArk Migrate Accounts via REST API"
date: 2024-11-07 9:10:10 +1100
categories: [CyberArk Privilege Cloud]
tags: [cyberark,privilegecloud,API]     # TAG names should always be lowercase
---

This video covers the process of migrating Platforms, Safes and Accounts using the API.

[![Video Preview](https://i.ytimg.com/vi/tPucVKyBqGY/maxresdefault.jpg)](https://www.youtube.com/watch?v=tPucVKyBqGY)

## Objectives
- Migrate Platforms via REST API
- Migrate Safes via REST API
- Migrate Accounts via REST API

## CyberArk GitHub Documenation

[Cyberark epv-api-scripts](https://github.com/cyberark/epv-api-scripts/tree/main/Migration/Migration%20via%20REST)

## PowerShell Commnads Used

```powershell
# Download GitHub repo https://github.com/cyberark/epv-api-scripts
# Unbock Files
dir -r | Unblock-File
# Import Modules
cd "C:\Migration\epv-api-scripts-main\Migration\Migration via REST\"
Import-Module ".\Migrate.psm1" -force
Import-Module "C:\Migration\epv-api-scripts-main\Identity Authentication\IdentityAuth.psm1"

# Export Platforms 
cat ./platformlist.txt
#$SourceUPCred = get-credential
mkdir ./PLATFORMZIPS/
# If auth is CyberArk use this to export platforms
C:\Migration\epv-api-scripts-main\Platforms\Export-Import-Platform.ps1 -PVWAURL https://<SourcePVWA>/PasswordVault/ -ExportFile -ListFile ./platformlist.txt -PlatformZipPath ./PLATFORMZIPS/
# If auth is LDAP use this to export platforms
C:\Migration\epv-api-scripts-main\Platforms\Export-Import-Platform.ps1 -PVWAURL https://<SourcePVWA>/PasswordVault/ -ExportFile -ListFile ./platformlist.txt -PlatformZipPath ./PLATFORMZIPS/ -AuthType ldap

# Import Platforms
$DestUPCred = get-credential
$header = Get-IdentityHeader -IdentityTenantURL <Your_Tenant_Identity_ID>.id.cyberark.cloud -UPCreds $DestUPCred
C:\Migration\epv-api-scripts-main\Platforms\Export-Import-Platform.ps1 -ImportFile -PVWAURL https://<subdomain>.privilegecloud.cyberark.cloud/PasswordVault -ListFile ./PLATFORMZIPS/_Exported.txt -LogonToken $header

# Source PVWA Auth
$SourceUPCred = get-credential
New-SourceSession -srcPVWAURL https://<SourcePVWA>/PasswordVault/ -srcAuthType LDAP -srcPVWACredentials $SourceUPCred

# Export Accounts from Source
Export-Accounts -exportCSV ./export.csv

# Use this to open the CSV file and adjust what needs to be migrated
Start ./export.csv

cat ./Import.csv

# Import Accounts
Import-Accounts -importCSV ./Import.csv

# Command to import Failed accounts
Import-Accounts -importCSV ./FailedAccounts.csv

# Destination PVWA Auth
$DestUPCred = get-credential
$header = Get-IdentityHeader -IdentityTenantURL <identityID>.id.cyberark.cloud -UPCreds $DestUPCred
New-DestinationSession -dstPVWAURL https://<subdomain>.privilegecloud.cyberark.cloud/PasswordVault -dstLogonToken $header

# Sync Safes
Sync-Safes -CreateSafes -CPMOverride "PROD-CPM"

# Sync Accounts
Sync-Accounts -ProgressDetails -verbose

# Handy Command
Get-AccountList
```

# Timeline
- Intro 0:00
- Migrate Platforms 1:17
- Migrate Safes 5:39
- Migrate Accounts 9:55