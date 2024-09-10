---
title: "#18 - CyberArk EPM – Loosely Connected Devices"
date: 2024-09-09 9:10:10 +1100
categories: [EPM,CyberArk Privilege Cloud]
tags: [cyberark,privilegecloud,EPM,Identity]     # TAG names should always be lowercase
---

This video covers the process of configuring CyberArk EPM to support rotating local privileged accounts on loosely connected devices. We’ll explore both the manual and automated methods for installing the EPM agent and onboarding local privileged accounts into CyberArk Privilege Cloud.

[![Video Preview](https://i.ytimg.com/vi/V-Wh519vYFw/maxresdefault.jpg)](https://www.youtube.com/watch?v=V-Wh519vYFw)

## Objectives
- Configure EPM Credentials Rotation Policy
- Onboard EPM LCD Key
- Prepare Windows Loosely Device Platform
- Manually Install EPM Agent
- Manually Onboard and Rotate Local Privileged Accounts
- Automate EPM installation and Privileged Account Onboarding
- Confirm LCD Privileged Accounts Onboarding

## EPM LCD Documentaiton

[Cyberark Docs - Manage loosely connected devices](https://docs.cyberark.com/privilege-cloud-shared-services/latest/en/Content/PASIMP/LooselyConnectedDevices.htm)

[CyberArk Marketplace - EPM LCD Key Platform](https://community.cyberark.com/marketplace/s/#a35Ht000001phYDIAY-a39Ht000004Dfi4IAC)

[CyberArk Docs - Discover local accounts using EPM](https://docs.cyberark.com/privilege-cloud-shared-services/latest/en/Content/Privilege%20Cloud/PrivCloud-discover-accounts-EPM.htm)


## PVWA Server URL Format
This is the PVWA URL format that is required when creating the Credentials Rotation Policy in EPM.
```
https://<subdomain>.privilegecloud.cyberark.cloud/
```


# EPM LCD KEY Platform
This is the username and account name for the EPM LCD Key to be used in Privilege Cloud.
```
EPM_PAS_Gateway
```

# EPM Agent Commands
Use this command to monitor the EPM agent log.
``` powershell
cat -wait -tail 100 "C:\Program Files\CyberArk\Endpoint Privilege Manager\Agent\PASAgent\Trace\PASAgentLog.txt"
```
Use the fllowing commands to stop the EPM agent and force LCD account rotation.
```
cd 'C:\Program Files\CyberArk\Endpoint Privilege Manager\Agent\'
.\vf_agent.exe -UseToken <Generated_Secure_Token>
.\vf_agent -StopServ
.\vf_agent.exe -ImmediateLCDRotation
```
Start EPM agent via services.msc

## PowerShell EPM Agent Install Script
This is a sample script that can be used to install the EPM agent. There are many other ways to automate the EPM agent install.
Please find the more information here. [CyberArk Docs - Install EPM agents on Windows endpoints](https://docs.cyberark.com/epm/latest/en/Content/Installation/Windows-InstallAgents.htm#InstallEPMagentsonWindowsendpoints)



``` powershell
$filepath = "C:\Program Files\CyberArk\Endpoint Privilege Manager\Agent\vf_agent.exe"
$epminstallerdir = "\\<domain_name>\NETLOGON\EPM"

if (Test-Path $filepath) {
    # The EPM agent exists, so run your command
    Write-Output "The EPM Agent is present. Closing Script"
} else {
    Write-Output "The EPM agent does not exist. Installing EPM"
    MsiExec.exe /i "$epminstallerdir\vfagentsetupx64.msi" INSTALLATIONKEY="<INSTALLKEY_HERE>" CONFIGURATION="$epminstallerdir\CyberArkEPMAgentSetupWindows.config" /qn
}
```


# Timeline
- Intro 0:00
- Setup EPM Credentials Rotation Policy 2:10
- Setup EPM LCD Key Platform 4:33
- Setup Windows LCD Safe and Platform 6:11
- Manually Install EPM Agent 7:21
- Manually Onboard Localadmin Account 8:52
- Setup EPM Accounts Discovery 13:20
- Setup Discovery Rule 14:53
- Automate EPM Agent Install 16:16
- Test EPM Agent Install and Account Onboarding 17:05