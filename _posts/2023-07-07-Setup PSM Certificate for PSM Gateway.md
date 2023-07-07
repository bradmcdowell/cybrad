---
title: Setup PSM Certificate for PSM Gateway
date: 2023-07-07 10:10:10 +1100
categories: [cyberark, pam]
tags: [cyberark,html5 gateway,ssl,certificate]     # TAG names should always be lowercase
---

## Setup PSM Certificate for PSM Gateway

### Optional Self-Signed Certificate

Us this script to generate a self-signed certificate if there in't an internal CA avalilable.
``` powershell
New-SelfSignedCertificate -DnsName "cyberarkpsm.domain.com", "psmserver1.domain.com", "psmserver2.domain.com" -NotAfter (Get-Date).AddYears(3) -TextExtension @("2.5.29.37={text}1.3.6.1.5.5.7.3.1") -KeyLength 4096 -KeyExportPolicy Exportable -CertStoreLocation "cert:\LocalMachine\My"
```

### PSM Server Change Certificate

``` powershell
#Copy the thumbprint for the certificate you have the Private Key of (usually the personal certificate of the machine unless the customer followed your instructions to build the CA from scratch)

Get-ChildItem "Cert:\LocalMachine\My"

#Set the path for where the thumbprint will be seen by RDS
$PATH = (Get-WmiObject -class "Win32_TSGeneralSetting" -Namespace root\cimv2\terminalservices)

#Execute the change
Set-WmiInstance -Path $PATH -argument @{SSLCertificateSHA1Hash="<INSERT-Thumbprint-HERE>"}

```

Configure the connection component to use the HTML5 GW

```
AllowSelectHTML5

HTML5 Gateway

CyberArk.TransparentConnection.BooleanUserParameter, CyberArk.PasswordVault.TransparentConnection
```

![image](https://user-images.githubusercontent.com/65890052/227809357-6edddc17-ae9f-4ed3-8a47-ae427045e556.png)

