---
title: "#17 - CyberArk EPM and CyberArk Identity SAML Authentication"
date: 2024-09-05 9:10:10 +1100
categories: [EPM,CyberArk Identity]
tags: [cyberark,privilegecloud,EPM,Identity]     # TAG names should always be lowercase
---

This video covers the process setting up CyberArk Identity SAML auth with CyberArk EPM.

[![Video Preview](https://i.ytimg.com/vi/ArovTA4SyWY/maxresdefault.jpg)](https://www.youtube.com/watch?v=ArovTA4SyWY)

## Objectives
- Configure EPM SAML Authentication via CyberArk Identity
- Add Step Up Authentication Profile
- Configure Automatic User Provisioning.

# Automatic Provisioning
The [Cyberark EPM Configure authentication via SAML documentation.](https://docs.cyberark.com/epm/latest/en/Content/Admin/SAMLIntegration.htm#AutomaticSAMLuserprovisioning)

Link to [EPM-User-Binding script](https://community.cyberark.com/s/question/0D5Ht00009pF6RwKAK/cyberark-identity-epm-configuration)

## Example SAML Attribute

```
setAttribute('EPM-User-Binding','[{"R":"00000000-0000-0000-0000-000000000005","S":["All"]}]');
```

# Timeline
- Intro 0:00
- EPM Overview 0:36
- Identity SAML Config 0:55
- Test SAML Authentication 7:14
- Step Up Authentication 9:06
- Test Step Up Auth 10:44
- User Provisioning 11:32
- Test User Provisioning 15:00