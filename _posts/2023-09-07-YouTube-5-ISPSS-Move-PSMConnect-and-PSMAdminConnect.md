---
title: "#5 - CyberArk Privilege Cloud | Move PSM Application Users To The Domain Level"
date: 2023-09-07 10:10:10 +1100
categories: [Privilege Cloud,PSM]
tags: [cyberark,privilegecloud,psm,psmconnect,psmadminconnect]     # TAG names should always be lowercase
---

This video covers moving the PSM application users to the domain level.
<!---
[<img src="https://i.ytimg.com/vi/sxT60oX7bYQ/maxresdefault.jpg" width="50%">](https://www.youtube.com/watch?v=sxT60oX7bYQ)
--->
[![Video Preview](https://i.ytimg.com/vi/sxT60oX7bYQ/maxresdefault.jpg)](https://www.youtube.com/watch?v=sxT60oX7bYQ)

## Objectives
- Create the PSMConnect and PSMAdminConnect users in your domain
- Modify the domain users in Active Directory
- Update Group Policy
- Run the Set-DomainUser script
- Test PSM Session / Monitoring

Set-DomainUser.ps1 script is found [here](https://cyberark-customers.force.com/mplace/s/#--CyberArk+Privilege+Cloud+Tools).

Timeline:
- Intro 0:00
- Documentation 0:48
- Testing PSM and Monitoring before Change 1:56
- Requirements 3:08
- Create PSMConnect and PSMAdminConnect in Active-Directory 3:41
- Group Policy hardening changes 5:48
- Run Set-DomainUser.ps1 script 6:41
- Update PSM safe 8:43
- Update PSM Application Platform 9:08
- Rotate PSMConnect and PSMAdminConnect passwords 9:54
- Gpupdate / restart 12:05
- Test PSM Sessions and PSM session monitoring 12:38