---
title: "#6 - CyberArk Privilege Cloud | PSM Web Sessions"
date: 2023-09-13 10:10:10 +1100
categories: [Privilege Cloud,PSM,Chrome,Edge]
tags: [cyberark,privilegecloud,psm,chrome,edge]     # TAG names should always be lowercase
---

This video covers enabling web browsers running in a PSM session for both Google Chrome and Microsoft Edge.
<!---
[<img src="https://i.ytimg.com/vi/VLo2TBgS0MM/maxresdefault.jpg" width="50%">](https://www.youtube.com/watch?v=VLo2TBgS0MM)
--->
[![Video Preview](https://i.ytimg.com/vi/VLo2TBgS0MM/maxresdefault.jpg)](https://www.youtube.com/watch?v=VLo2TBgS0MM)

## Objectives
- Use Add-PSMApps.ps1 script to install Chrome and Edge
- Configure AppLocker for Chrome
- Create a connection component for (Palo Alto Web) using Chrome
- Test PSM web session to a test target using Chrome
- Create a connection component for (Palo Alto Web) using Edge
- Test PSM web session to a test target using Edge
- Web Driver Updater

# PSM Web Sessions - Google Chrome
#### Install Google Chrome and Microsoft Edge using Add-PSMApps.ps1 script

Add-PSMApps.ps1 script is found [here](https://cyberark-customers.force.com/mplace/s/#a352J000000GWAZQA4-a392J000002tNgLQAU).

``` powershell
.\Add-PSMApps.ps1 -Application "GoogleChromeX64","MicrosoftEdgeX86"
```

#### Google Chrome AppLocker Requirements
Check the version of Google Chrome installed on the PSM server and download  the chrome driver from [here](https://googlechromelabs.github.io/chrome-for-testing/). Usually you will select the chromedriver win32 from the Stable section.

Extract the chromedriver.exe and place it in the following locations
```
"C:\Program Files (x86)\CyberArk\Password Manager\bin\chromedriver.exe"
"C:\Program Files (x86)\CyberArk\PSM\Components\chromedriver.exe"
```

Edit the PSMConfigureAppLocker.xml file located at "C:\Program Files (x86)\CyberArk\PSM\Hardening\PSMConfigureAppLocker.xml"

At about line 147 you will find the following text, leave it there and make a new line underneath.
``` xml
    <Application Name="GoogleChrome" Type="Exe" Path="C:\Program Files\Google\Chrome\Application\chrome.exe" Method="Publisher" />
```
And place the following line of text.
``` xml
    <Application Name="GoogleChromeDriver" Type="Exe" Path="C:\Program Files (x86)\CyberArk\PSM\Components\chromedriver.exe" Method="Path" />
```

Run PowerShell as Administrator and execute the PSMConfigureAppLocker.ps1 script.

#### PSM Components Options configuration (Google Chrome)

Administration  -> Configuration Options

Configuraitons -> Connection Components

Configuraitons -> Connection Components -> *Connection Component ID* -> Target Settings -> Client Specific
BrowserPath - Value = 
```
C:\Program Files (x86)\Microsoft\Edge\Application\msedge.exe
```
EnableTrace - Value = Yes or No

# PSM Web Sessions - Microsoft Edge
#### Microsoft Edge AppLocker Requirements
Check you version of Microsoft Edge installed on the PSM server and download the x86 edge driver version.
https://developer.microsoft.com/en-us/microsoft-edge/tools/webdriver/

```
"C:\Program Files (x86)\CyberArk\Password Manager\bin\msedgedriver.exe"
"C:\Program Files (x86)\CyberArk\PSM\Components\msedgedriver.exe"
```

#### App locker - Microsoft Edge

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

#### PSM Components Options configuration (Microsoft Edge)

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

See [Cyberark Documentation - Web applications for PSM](https://docs.cyberark.com/PrivCloud-SS/Latest/en/Content/PASIMP/psm_WebApplication.htm)

# Timeline:
- Intro 0:00
- Palo Alto Firewall Setup 0:59
- Install Chrome and Edge using Add-PSMApps.ps1 script 2:14
- Chrome Driver and PSMConfigureAppLocker.xml for Chrome 3:37
- Chrome PSM Connection Component example - Palo Alto 7:31
- Test Chrome PSM Session 11:26
- Edge Web Driver and PSMConfigureAppLocker.xml for Edge  12:07
- Edge PSM Connection Component example - Palo Alto 14:42
- Test Edge PSM Session 16:14
- WebDriverUpdater Overview 17:49
