---
title: Install Cyberark Identity Connector
date: 2023-07-06 10:10:10 +1100
categories: [Privilege Cloud,Identity]
tags: [cyberark,identity connector]     # TAG names should always be lowercase
---

## Script to install CyberArk Identity Connector
``` powershell
Start-BitsTransfer "https://edge.idaptive.app/ProxyDownload/CyberArk-Identity-Management-Suite-win64.zip" $env:TEMP\CyberArk-Identity-Management-Suite-win64.zip
$IdentityZipPackage = "$env:TEMP\CyberArk-Identity-Management-Suite-win64.zip"
Expand-Archive $IdentityZipPackage -DestinationPath $env:TEMP\CyberArk-Identity-Management-Suite-win64
$InstallerName = Get-ChildItem -Path $env:TEMP\CyberArk-Identity-Management-Suite-win64\CyberArk-Identity*.exe
$InstallEXE = $InstallerName.Name
Start-Process $env:TEMP\CyberArk-Identity-Management-Suite-win64\$InstallEXE
Write-Host "Finished"
```