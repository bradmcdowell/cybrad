---
title: Add-PSMApps
date: 2023-07-04 10:10:10 +1100
categories: [Privilege Cloud,PSM]
tags: [cyberark,chrome,rsat]     # TAG names should always be lowercase
---

The follwoing notes are based on the [How to use Add PSMApplication](https://cyberark-customers.force.com/s/article/How-to-use-Add-PSMApplication) documenation.

Step 1: [Download Privilege Cloud Tools](https://cyberark-customers.force.com/mplace/s/#a352J000000GWAZQA4-a392J000002tNgLQAU) and copy to C:\CyberArk folder on the connector soerver.

Step 2: Navigate and extract the Add-PSMApps

Step 3: Run a Powershell session as Administrator and run the follwoing sample command.

This command adds ADUC,GPMC and a GenericMMC (RSAT) console 
``` powershell
.\Add-PSMApps.ps1 -Application "ADUC","GPMC","GenericMMC" -MscPath "C:\PSMApps\RSAT.msc" -ComponentName "RSAT" -ComponentDisplayName "PSM-RSAT" -HTML5 "OffByDefault" -PortalUrl "https://subdomain.privilegecloud.cyberark.cloud"
```

This command adds ADUC,GPMC only

``` powershell
.\Add-PSMApps.ps1 -Application "ADUC","DNS" -HTML5 "OffByDefault" -PortalUrl "https://subdomain.privilegecloud.cyberark.cloud"
```

This command adds GoogleChromeX64,ADUC,DNS,DHCP,GPMC and a GenericMMC (RSAT) console 

``` powershell
.\Add-PSMApps.ps1 -Application "GoogleChromeX64","ADUC","DNS","DHCP","GPMC","GenericMMC" -MscPath "C:\PSMApps\RSAT.msc" -ComponentName "PSM-RSAT" -ComponentDisplayName "RSAT" -HTML5 "OffByDefault" -PortalUrl "https://subdomain.privilegecloud.cyberark.cloud"
```

I want it all!!! This command adds GoogleChromeX64,MicrosoftEdgeX86,ADUC,DNS,DHCP,GPMC and a GenericMMC (RSAT) console 
``` powershell
.\Add-PSMApps.ps1 -Application "GoogleChromeX64","MicrosoftEdgeX86","ADUC","DNS","DHCP","GPMC","GenericMMC" -MscPath "C:\PSMApps\RSAT.msc" -ComponentName "PSM-RSAT" -ComponentDisplayName "RSAT" -HTML5 "OffByDefault" -PortalUrl "https://subdomain.privilegecloud.cyberark.cloud"
```