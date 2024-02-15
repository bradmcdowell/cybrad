---
title: CyberArk PSM Health Check
date: 2023-07-07 10:10:10 +1100
categories: [Privilege Cloud,PSM]
tags: [cyberark,PSM Health Check]     # TAG names should always be lowercase
---

### Requirements
Downlaod the PSM Health Check Script
https://cyberark-customers.force.com/mplace/s/#--PSM+Health+check

Download .NET Core Runtime 6.0.X
https://dotnet.microsoft.com/en-us/download/dotnet/6.0

### Run the HealthCheck.ps1 script
``` powershell
.\HealthCheck.ps1 -installPath "E:\Program Files (x86)\Cyberark\PSM"
```
or
Use this command when CyberArk has been installed on a different drive letter.
``` powershell
.\HealthCheck.ps1 -installPath "E:\Program Files (x86)\Cyberark\PSM"
```

### Test Health Check
``` powershell
Invoke-WebRequest -uri https://fqdn/psm/api/health
```
