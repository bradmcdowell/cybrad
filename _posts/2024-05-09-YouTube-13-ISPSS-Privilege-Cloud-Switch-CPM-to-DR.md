---
title: "#13 - Switch CPM From Primary To DR Connector"
date: 2024-05-09 10:10:10 +1100
categories: [Privilege Cloud,Connector Management,CPM]
tags: [cyberark,privilegecloud,connectormanagement,cpm]     # TAG names should always be lowercase
---

This video covers the process of switching the CPM from Primary to DR.

[![Video Preview](https://i.ytimg.com/vi/U2v7-9X6vUQ/maxresdefault.jpg)](https://www.youtube.com/watch?v=U2v7-9X6vUQ)

## Objectives
- ApiKeyManager.exe removed from CPM 14.2+
- Upgrade CPM from 14.0 to 14.2
- New CPM Sync Component Utility
- Move CPM from CON1 to CON2
- Move CPM form CON2 to CON1


[CyberArk Documentation - SyncComponentUsers utility](https://docs.cyberark.com/privilege-cloud-shared-services/Latest/en/Content/PASIMP/SystemHealth.htm?Highlight=system%20health#RestoreconnectionforCPM)

[CyberArk Documentation - Switch between the primary and DR CPMs](https://docs.cyberark.com/privilege-cloud-shared-services/Latest/en/Content/Privilege%20Cloud/PrivCloud-CPM-DR-switch.htm)

Add to vault.ini

```
[API]
Addresses=https://subdomain.privilegecloud.cyberark.cloud/passwordvault
```

# Timeline
- Intro 0:00
- ApiKeyManager.exe 1:23
- CPM Upgrade 2:07
- Move CPM from CON1 to CON2 4:12
- Test CPM and Fix PluginManagerUser Issue 9:05
- Move CPM from CON2 to CON1 11:05