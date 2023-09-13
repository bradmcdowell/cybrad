---
title: Download and Install Google Chrome for PSM
date: 2023-06-02 10:10:10 +1100
categories: [Privilege Cloud,PSM]
tags: [cyberark,chrome]     # TAG names should always be lowercase
---


### 64bit Google Chrome
This script downloads Google Chrome 64bit and executes the msi installer.
``` powershell
Start-BitsTransfer "https://dl.google.com/edgedl/chrome/install/GoogleChromeStandaloneEnterprise64.msi" $env:TEMP\GoogleChromeStandaloneEnterprise64.msi
$chromeinstaller = "$env:TEMP\GoogleChromeStandaloneEnterprise64.msi"
msiexec.exe /package $chromeinstaller
```


### 32bit Google Chrome
This script downloads Google Chrome 32bit and executes the msi installer.
``` powershell
Start-BitsTransfer "https://dl.google.com/edgedl/chrome/install/GoogleChromeStandaloneEnterprise.msi" $env:TEMP\GoogleChromeStandaloneEnterprise64.msi
$chromeinstaller = "$env:TEMP\GoogleChromeStandaloneEnterprise.msi"
msiexec.exe /package $chromeinstaller
```
### Google Chrome Driver
Downlaod the [Google Chrome Driver](https://chromedriver.chromium.org/downloads) and place it in the "C:\Program Files (x86)\CyberArk\PSM\Components\" folder

### Applocker Configuration

Edit file "C:\Program Files (x86)\CyberArk\PSM\Hardening\PSMConfigureAppLocker.xml"

``` xml
    <!-- Google Chrome process -->
    <!-- If relevant, uncomment this part to allow Google Chrome webform based connection clients
    End of Google Chrome process comment -->
    <Application Name="GoogleChrome" Type="Exe" Path="C:\Program Files\Google\Chrome\Application\chrome.exe" Method="Publisher" />
    <Application Name="GoogleChromeDriver" Type="Exe" Path="C:\Program Files (x86)\CyberArk\PSM\Components\chromedriver.exe" Method="Path" />    
```
