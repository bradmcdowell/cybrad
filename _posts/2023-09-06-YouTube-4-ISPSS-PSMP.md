---
title: "#4 - CyberArk Privilege Cloud | PSM for SSH (PSMP)"
date: 2023-09-06 10:10:10 +1100
categories: [Privilege Cloud,PSMP]
tags: [cyberark,privilegecloud,psmp,psmforssh]     # TAG names should always be lowercase
---

This video covers setting up the *NIX platform and installing PSM for SSH (PSMP).

<!---
[<img src="https://i.ytimg.com/vi/IA68mw4eqRs/maxresdefault.jpg" width="50%">](https://www.youtube.com/watch?v=IA68mw4eqRs)
--->
[![Video Preview](https://i.ytimg.com/vi/IA68mw4eqRs/maxresdefault.jpg)](https://www.youtube.com/watch?v=IA68mw4eqRs)

## Objectives
- Setup Unix Platform, Configuration Options and Create Safes
- Onboard Linux Account
- Test CPM and PSM
- PSMP Requirements
- Install PSMP
- Test PSMP
- Configure and Test PSM For SSH MFA Caching

## Commands used to install PSMP

Download [PrivilegedSessionManagerSSHProxy-RHELinux8-Intel64-Rls-v1X.X.zip from the CyberArk Marketplace](https://cyberark-customers.force.com/mplace/s/#software#---Name-CyberArk%20Privilege%20Cloud)

Copy package from windows machine to PSMP server

```
scp "C:\<Path_To_File>\PrivilegedSessionManagerSSHProxy-RHELinux8-Intel64-Rls-v13.2.zip" <USERNAME>@<PSMP_HOSTNAME>:~/
```

Update the system and install wget and unzip
``` bash
sudo dnf update
sudo dnf install wget unzip
```

``` bash
ls
mkdir PSMP
mv PrivilegedSessionManagerSSHProxy-RHELinux8-Intel64-Rls-v13.2.zip ./PSMP/
cd ./PSMP/
ls
```
``` bash
unzip PrivilegedSessionManagerSSHProxy-RHELinux8-Intel64-Rls-v13.2.zip
```

PSMPWIZ scripts are found [here](https://github.com/pCloudServices/psmpwiz). You will need to update this url depending on the version of PSMP being installed.
``` bash
wget https://raw.githubusercontent.com/pCloudServices/psmpwiz/main/psmpwiz1320.sh
```

Confirm you have the files
``` bash
ls -ltr
```

Add the exectue permission to the follwoing files:
``` bash
chmod 755 CreateCredFile
chmod 755 CARKpsmp-13.2.0.15.x86_64.rpm
chmod 755 psmpwiz1320.sh
```

If you are not running as the root users, switch to the root user.
``` bash
sudo -i
```

Change Directory to where the PSMP files are located
``` bash
cd /home/<USERNAME>/PSMP/
```

Confirm if the files are executable
``` bash
ls -ltr
```

Run the PSMP wizard
``` bash
./psmpwiz1320.sh
```

## Links

[PSM for SSH Syntax Cheat Sheet](https://cyberark.my.site.com/s/article/PSM-for-SSH-Syntax-Cheat-Sheet)