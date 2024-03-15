---
title: "#12 - CyberArk Privilege Cloud | Connector Updates 14.1"
date: 2024-03-15 10:10:10 +1100
categories: [Privilege Cloud,Connector Management,CPM,PSM,PSMP,]
tags: [cyberark,privilegecloud,connectormanagement,cpm,psm,psmp]     # TAG names should always be lowercase
---

This video covers the process to updating the windows CyberArk connector server via Connector Management and unix PSMP.

[![Video Preview](https://i.ytimg.com/vi/jY9J6xFUxoU/maxresdefault.jpg)](https://www.youtube.com/watch?v=jY9J6xFUxoU)

## Objectives
- Documentation Overview and CyberArk Marketplace
- Update Group Policy
- Update Connector Management Agent
- Update CPM via Connector Management
- Update PSM via Connector Management
- Update Secure Tunnel
- Update Identity Connector
- Update PSMP

The CyberArk documenation can be found [here.](https://docs.cyberark.com/privilege-cloud-shared-services/Latest/en/Content/Privilege%20Cloud/PrivCloud-upgrade-connector-12.7-later-CM.htm)

Backup Group Policy command
```
gpresult /h C:\CyberArk\PolicyBeforeUpgrade.html
```
You can find the latest psmpwiz scritps at this [GitHub page](https://github.com/pCloudServices/psmpwiz). Copy the RAW link for the relvant version.

In the video we used the follwoing script for version 14.1

```
https://raw.githubusercontent.com/pCloudServices/psmpwiz/main/psmpwiz1410.sh
```

# Timeline
- Intro 0:00
- Lab Overview and Documentation 0:42
- Upgrade Checks 1:42
- Group Policy Hardening Update 3:31
- Reset Installer User Password 5:46
- Connector Management Update and CPM Update 6:18
- PSM Update 9:32
- Secure Tunnel and Identity Connector Update 13:06
- PSMP Update 17:03