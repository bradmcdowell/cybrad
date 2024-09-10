---
title: "#18 - CyberArk EPM – Loosely Connected Devices"
date: 2024-09-09 9:10:10 +1100
categories: [EPM,CyberArk Identity]
tags: [cyberark,privilegecloud,EPM,Identity]     # TAG names should always be lowercase
---

This video covers the process of Installing the EPM agent and onborading local privileged accounts.

[![Video Preview](https://i.ytimg.com/vi/ArovTA4SyWY/maxresdefault.jpg)](https://www.youtube.com/watch?v=ArovTA4SyWY)

## Objectives
- TBC
# EPM commands to force rotation

``` powershell
cat -wait -tail 100 "C:\Program Files\CyberArk\Endpoint Privilege Manager\Agent\PASAgent\Trace\PASAgentLog.txt"
```

```
cd 'C:\Program Files\CyberArk\Endpoint Privilege Manager\Agent\'
.\vf_agent.exe -UseToken <Generated_Secure_Token>
.\vf_agent -StopServ
```

## Install EPM Agent Script

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
