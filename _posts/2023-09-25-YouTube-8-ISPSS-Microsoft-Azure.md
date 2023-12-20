---
title: "#8 - CyberArk Privilege Cloud | Microsoft Azure"
date: 2023-09-25 10:10:10 +1100
categories: [Privilege Cloud,CPM,PSM,Microsoft Azure, Microsoft Entra ID]
tags: [cyberark,privilegecloud,cpm,psm,microsoft,azure,entraid]     # TAG names should always be lowercase
---

This video covers the Microsoft Azure platform.
<!---
[<img src="https://i.ytimg.com/vi/bjHjosRtQ7s/maxresdefault.jpg" width="50%">](https://www.youtube.com/watch?v=bjHjosRtQ7s)
--->
[![Video Preview](https://i.ytimg.com/vi/bjHjosRtQ7s/maxresdefault.jpg)](https://www.youtube.com/watch?v=bjHjosRtQ7s)

## Objectives
- Configure Azure Platform for Application Keys and Password Management
- Create CyberArk CPM Application in Azure
- Onboard and rotate the CyberArk CPM application key into CyberArk
- Onboard and rotate Azure Cloud accounts into CyberArk
- Establish PSM session to Azure Web Portal using cloud accounts
- Establish PSM session to Azure Web Portal using domain accounts
- Create a Connection Component for Microsoft MyApps Portal
- Establish PSM session to Microsoft My Apps portal using a domain account

### CyberArk Marketplace Links
[Microsoft Azure Application Keys - CPM](https://cyberark.my.site.com/mplace/s/#a3550000000EiBdAAK-a3950000000jjUDAAY)

[Microsoft Azure Password Management - CPM](https://cyberark.my.site.com/mplace/s/#a3550000000EiBeAAK-a3950000000jjUEAAY)

[Microsoft Azure Portal - PSM](https://cyberark.my.site.com/mplace/s/#a3550000000EiBfAAK-a3950000000jjUFAAY)

### Microsoft Azure Portal
Address
```
https://portal.azure.com
```

PSM-MS-AzurePortal -> Target Settings -> Web Form Settings -> WebFormFields
```
i0116 > {username}@cybrad.au
idSIButton9 > (Button)
i0118 > {password}
idSIButton9 > (Button)
idSIButton9 > (Button)
_weave_e_10 > (Validation)
```

### Microsoft My Apps Portal
Address
```
https://myapps.microsoft.com/
```

PSM-MS-AzurePortal -> Target Settings -> Web Form Settings -> WebFormFields
```
i0116 > {username}@cybrad.au
idSIButton9 > (Button)
i0118 > {password}
idSIButton9 > (Button)
idSIButton9 > (Button)
me-control-container > (Validation)
```
# Timeline:
- Intro 0:00
- Lab Overview 1:33
- Market Place Links 1:57
- Create Safes 2:30
- Azure Platform 2:56
- Create Azure CPM App Registration  3:26
- Onboard Azure CPM Account 5:55
- Onboard Cloud Accounts 7:57
- Configure Azure PSM Cloud Account 11:56
- Validation Error Fix 14:05
- Configure Azure PSM AD Account 17:28
- Configure MS MyApps PSM AD Account 19:03