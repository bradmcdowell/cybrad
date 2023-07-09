---
# the default layout is 'page'
icon: fas fa-info-circle
order: 1
---
# CyberArk Privilege CloudDownload Links

[CyberArk Privilege Cloud Tools](https://cyberark-customers.force.com/mplace/s/#--Privilege+Cloud+Tools)

[CyberArk Privilege Cloud Software](https://cyberark-customers.force.com/mplace/s/#software#---Name-CyberArk%20Privilege%20Cloud)

[CyberArk PSM Health Check](https://cyberark-customers.force.com/mplace/s/#a352J000000ai0MQAQ-a392J000002QBA6QAO)

# Pre-Requisites CyberArk Privlege Cloud Connector

[Connector Hardware Requirements](https://docs.cyberark.com/Product-Doc/OnlineHelp/PrivCloud-SS/Latest/en/Content/Privilege%20Cloud/PrivCloud-sys-req-connector.htm#Hardwarerequirements)

[Outbound traffic network and port requirements](https://docs.cyberark.com/Product-Doc/OnlineHelp/PrivCloud-SS/Latest/en/Content/Privilege%20Cloud/PrivCloud-sys-req-networks.htm)


VAULT TCP 1858
```
vault-<subdomain>.privilegecloud.cyberark.cloud
```

https / TCP 443
```
connector-<subdomain>.privilegecloud.cyberark.cloud
<subdomain>.cyberark.cloud
<subdomain>.privilegecloud.cyberark.cloud
console.privilegecloud.cyberark.cloud
webaccess-<subdomain>.privilegecloud.cyberark.cloud
<Identity-tenant-id>.id.cyberark.cloud
cloudflare.com
```

Connector Management HTTPS TCP 443
```
<Subdomain>.connectormanagement.cyberark.cloud
connector-management-scripts-490081306957-ap-southeast-2.s3.amazonaws.com
connector-management-assets-490081306957-ap-southeast-2.s3.amazonaws.com
a3vvqcp8z371p3-ats.iot.ap-southeast-2.amazonaws.com
component-registry-store-490081306957.s3.amazonaws.com
```

Identity Connector HTTPS TCP 443
```
*.idaptive.app
*.id.cyberark.cloud
```

Identity Connector HTTP TCP 80
```
privacy-policy.truste.com
ocsp.verisign.com
ocsp.globalsign.com
crl.globalsign.com
secure.globalsign.com
```

### Connector Management

Handy command to watch the Connector Managment Logs

Connector Managment logging documentation [here](https://docs.cyberark.com/Product-Doc/OnlineHelp/PrivCloud-SS/Latest/en/Content/Setup/CM_Troubleshooting.htm).

``` powershell
cat -wait -tail 50 'C:\Program Files\CyberArk\Management Agent\Logs\client_log.txt'
```

## 03 - Secure Tunnel

Install Secure Tunnel
The credntials for the installeruser will be required.

## 04 - CyberArk Identity

Extract CyberArk-Identity-Management-Suite-win64.zip and run exe as administraotr.

The credntials for the installeruser will be required.
## 05 - PSM Certificate and HTML5 Gateway



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


## 08 - PSM Native Access RDP

https://docs.cyberark.com/Product-Doc/OnlineHelp/PrivCloud/Latest/en/Content/Privilege%20Cloud/privCloud-connect-using-RDP.htm?tocpath=End%20Users%7CConnect%20to%20a%20target%20device%7C_____3#RDPsettings

```
full address:s:PSMHOSTNAME
alternate shell:s:psm /u localadmin01 /a win1.cybr.com /c PSM-RDP
username:s:john
enablecredsspsupport:i:0
```
If using RDCMan
https://cyberark-customers.force.com/s/article/How-to-setup-Remote-Desktop-Connection-Manager


CPM Troubleshooting

https://cyberark-customers.force.com/s/article/How-does-CPM-manage-Windows-accounts-passwords

## 09 - App Locker Troubleshooting

This command is useful to determine what applications can’t run because of AppLocker.

``` powershell
Get-WinEvent -LogName "Microsoft-Windows-AppLocker/EXE and DLL" |Where-Object {$_.LevelDisplayName -ne "Information"} | Select-Object -First 200 | Format-Table
```

## 10 - Logs

CPM
``` powershell
cat -Wait -Tail 50 .\pm.log
```

PSM


Secure Tunnel

``` powershell
cat -Wait -Tail 50 "C:\Program Files\CyberArk\PrivilegeCloudSecureTunnel\logs\privilege-cloud-securetunnel-service.log"
```

## 16 - CyberArk Identity: How to configure or restrict attribute matching for the CyberArk Identity Connector

https://cyberark-customers.force.com/s/article/CyberArk-Identity-How-to-configure-or-restrict-attribute-matching-for-the-CyberArk-Identity-Connector