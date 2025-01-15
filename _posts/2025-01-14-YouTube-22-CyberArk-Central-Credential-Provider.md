---
title: "#22 - CyberArk Central Credential Provider (CCP)"
date: 2025-01-14 14:10:10 +1100
categories: [CyberArk Central Credential Provider]
tags: [cyberark,cyberarkccp,privilegecloud,powershell,curl,tls,scripts]     # TAG names should always be lowercase
---

In this video, you'll learn how to install the CyberArk Central Credential Provider (CCP) and perform thorough testing using PowerShell and curl. Additionally, I'll guide you through configuring the Qualys Cloud Platform to securely retrieve secrets from the CyberArk CCP.

[![Video Preview](https://i.ytimg.com/vi/oqTVoaRn9Qo/maxresdefault.jpg)](https://www.youtube.com/watch?v=oqTVoaRn9Qo)

## CyberArk Marketplace
Download [CyberArk Central Credential Provider](https://community.cyberark.com/marketplace/s/#software-aK4Ht000000L675KAC-) found at the CyberArk Marketplace.

## CyberArk Central Credential Provider Docs
[Central Credential Provider (CCP)](https://docs.cyberark.com/credential-providers/latest/en/content/ccp/the-central%20-credential-provider.htm)

[Load balance the Central Credential Provider](https://docs.cyberark.com/credential-providers/latest/en/content/ccp/load-balancing-the-central-credential-provider.htm)

## Disable TLS 1.3 over TCP
Microsoft Blog - [Windows Server 2022 IIS web site TLS 1.3 does not work with client certificate authentication](https://techcommunity.microsoft.com/blog/iis-support-blog/windows-server-2022-iis-web-site-tls-1-3-does-not-work-with-client-certificate-a/4129738)

Microsoft A&A - [Requests to IIS with SSLCertNegotiate and TLS 1.3 is failing](https://learn.microsoft.com/en-us/answers/questions/1628012/requests-to-iis-with-sslcertnegotiate-and-tls-1-3)

Tennable Docs [Tennable Documentation](https://community.tenable.com/s/feed/0D5WP00000MkXmz0AF?language=en_US)


## PowerShell Script - No Certificate Authentication
```powershell
#Variable Definitions
$restresponse = $null
$object = "<AccountNameHere>"
$CCPAddress = "https://CCPURL.company.com"
$AppID = "ApplicationNameHere”
$URI = "$CCPAddress/AIMWebService/api/Accounts?AppID=$AppID&Object=$object"
#Creation of REST Web Request
$restresponse = invoke-restmethod -uri $URI -method GET
#REST Output
$restresponse
```
## PowerShell Command to get cert thumbprints
``` powershell
Get-ChildItem -Path Cert:\CurrentUser\My
```
## PowerShell Script - With Certificate Authentication
```powershell
#Variable Definitions
$restresponse = $null
$object = "<AccountNameHere>"
$CCPAddress = "https://CCPURL.company.com"
$AppID = "ApplicationNameHere”
$URI = "$CCPAddress/AIMWebService/api/Accounts?AppID=$AppID&Object=$object"
#Creation of REST Web Request
$restresponse = invoke-restmethod -uri $URI -method GET -CertificateThumbprint "1AC459C80C24245B05ECF4137FC4E80FB81D7EF6"
#REST Output
$restresponse
```
## OpenSSL Commands

This command will extract the full certificate chain of the public certificate.
```bash
openssl pkcs12 -in export.pfx -out cert.pem -nodes -nokeys
```
This command will extract the private key only and encrypt it with a passphrase.
```bash
openssl pkcs12 -in export.pfx -nocerts -out key.pem
```
This command will test the ccp
```bash
curl --http1.1 --cert cert.pem --key key.pem "https://CCPURL.company.com/AIMWebService/api/Accounts?AppID=INSERTAPPIDHERE&Object=INSERTOBJECTNAMEHERE" -v
```
# Timeline
- Intro 0:00
- Set Password and Download Files 2:26
- Install CCP on CCP1 3:08
- Install CCP on CCP2 12:42
- Add Applications 13:30
- Safe Permissions 15:53
- Add Account 18:29
- Test with PowerShell 21:13
- Setup certificate Template and Request Cert 22:06
- Update Application 23:39
- Test PowerShell with Cert auth 25:13
- Export certificate 27:23
- Convert certificate with OpenSSL 29:58
- Test CCP with Curl 32:31
- Qualys Setup and Testing