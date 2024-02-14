---
title: CyberArk PSM Sessions Using Microsoft Edge
date: 2023-07-03 10:10:10 +1100
categories: [Privilege Cloud,PSM]
tags: [cyberark,edge]     # TAG names should always be lowercase
---

CyberArk PSM with Microsoft Edge

#### Install Microsoft Edge
Download and install microsoft edge. Ensure you select the correct operating system
https://www.microsoft.com/en-us/edge?form=MA13FJ#evergreen

#### Install Edge Driver
Check you version of Microsoft Edge. 1XX.X.XXXX.XX and download the x86 driver version.
https://developer.microsoft.com/en-us/microsoft-edge/tools/webdriver/

Extract the zip file and copy the msedgedriver.exe to the follwoing location:
"C:\Program Files (x86)\Cyberark\PSM\Components\msedgedriver.exe"

#### Secure Web Application Connectors Framework

https://cyberark-customers.force.com/mplace/s/#a3550000000EiCMAA0-a3950000000jjUwAAI

Extract zip file in a temporary location.
Copy all contents in the folder WebAppDispatcher-v13.0.0.87\Components
And paste in C:\Program Files (x86)\Cyberark\PSM\Components 
This process will overwrite many files.

#### App locker

In the"-- Allowed DLLs --" section add the follwoing line

```xml
    <Libraries Name="NATIVEIMAGES" Type="Dll" Path="%WINDIR%\ASSEMBLY\NATIVEIMAGES_V4.0.30319_32\*" Method="Path" SessionType="*" />
```

In the "-- Microsoft Edge process --" section, uncomment the Edge application and add the EdgeDrive line.
```xml
    <Application Name="Edge" Type="Exe" Path="C:\Program Files (x86)\Microsoft\Edge\Application\msedge.exe" Method="Publisher" />
    <Application Name="EdgeDriver" Type="Exe" Path="C:\Program Files (x86)\Cyberark\PSM\Components\msedgedriver.exe" Method="Publisher" />

```

In the "-- Allowed DLLs --" section add NATIVEIMAGES.
NOTE: this is different to the NATIVEIMAGES above.

```xml
    <Libraries Name="NATIVEIMAGES" Type="Dll" Path="%WINDIR%\ASSEMBLY\NATIVEIMAGES_V4.0.30319_32\*" Method="Path" />
```

#### PSM Components Options configuration

Administration  -> Configuration Options

Configuraitons -> Connection Components

Duplicate an existing Chrome Connecton Component

Configuraitons -> Connection Components -> *Connection Component ID* -> Target Settings
Change the value for "ClientApp" form Chrome to Edge

Configuraitons -> Connection Components -> *Connection Component ID* -> Target Settings -> Client Specific
BrowserPath - Value = 

```
C:\Program Files (x86)\Microsoft\Edge\Application\msedge.exe
```
EnableTrace - Value = Yes or No
