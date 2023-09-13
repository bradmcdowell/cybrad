---
title: 6 - CyberArk Privilege Cloud - Shared Services | PSM Web Sessions
date: 2023-09-13 10:10:10 +1100
categories: [Privilege Cloud,PSM,Chrome,Edge]
tags: [cyberark,privilegecloud,psm,chrome,edge]     # TAG names should always be lowercase
---

This video covers enabling web browsers running in a PSM session for both Google Chrome and Microsoft Edge.

[<img src="https://i.ytimg.com/vi/sxT60oX7bYQ/maxresdefault.jpg" width="50%">](https://www.youtube.com/watch?v=sxT60oX7bYQ)

# #6 - CyberArk Privilege Cloud - Shared Services | PSM Web Sessions Using Chrome and Edge

## Objectives
- Use Add-PSMApps.ps1 script to install Chrome and Edge
- Configure AppLocker for Chrome
- Create a connection component for (Palo Alto Web) using Chrome
- Test PSM web session to a test target using Chrome
- Create a connection component for (Palo Alto Web) using Edge
- Test PSM web session to a test target using Edge
- Web Driver Updater

## Install Google Chrome and Microsoft Edge using Add-PSMApps.ps1 script

``` powershell
.\Add-PSMApps.ps1 -Application "GoogleChromeX64","MicrosoftEdgeX86"
```

## Google Chrome AppLocker Requirements
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

Run PowerShell as Administrator and execute the PSMConfigureAppLocker.xml script.

Update the connection component broweser path if required.


## Microsoft Edge AppLocker Requirements
Check you version of Microsoft Edge installed on the PSM server and download the x86 edge driver version.
https://developer.microsoft.com/en-us/microsoft-edge/tools/webdriver/

```
"C:\Program Files (x86)\CyberArk\Password Manager\bin\msedgedriver.exe"
"C:\Program Files (x86)\CyberArk\PSM\Components\msedgedriver.exe"
```

Timeline:
- Intro 0:00
- Goal PaloAlto firewall
-
