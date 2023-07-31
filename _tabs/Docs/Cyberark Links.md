---
# the default layout is 'page'
icon: fas fa-link
order: 1
---
### CyberArk Privilege Cloud Software Download Links

[CyberArk Privilege Cloud Tools](https://cyberark-customers.force.com/mplace/s/#--Privilege+Cloud+Tools)

[CyberArk Privilege Cloud Software](https://cyberark-customers.force.com/mplace/s/#software#---Name-CyberArk%20Privilege%20Cloud)

[CyberArk PSM Health Check](https://cyberark-customers.force.com/mplace/s/#a352J000000ai0MQAQ-a392J000002QBA6QAO)

### Pre-Requisites CyberArk Privlege Cloud Connector - Hardware

[Connector Hardware Requirements](https://docs.cyberark.com/Product-Doc/OnlineHelp/PrivCloud-SS/Latest/en/Content/Privilege%20Cloud/PrivCloud-sys-req-connector.htm#Hardwarerequirements)

### Pre-Requisites CyberArk Privlege Cloud Connector - Network

[Outbound traffic network and port requirements](https://docs.cyberark.com/Product-Doc/OnlineHelp/PrivCloud-SS/Latest/en/Content/Privilege%20Cloud/PrivCloud-sys-req-networks.htm)


Allow connector server outbound connectivity to the follwing hostnames. 

There are 2 seperate variables in the hostnames below.
subdomain = The subdomain found in the URL ends with cyberark.cloud eg https://bradtest1.cyberark.cloud/
Identity-tenant-id = The subdomain found in the Identity Administration portal eg  https://aaw9999999.id.cyberark.cloud/admin

#### VAULT / TCP 1858
```
vault-<subdomain>.privilegecloud.cyberark.cloud
```

#### HTTPS / TCP 443
```
connector-<subdomain>.privilegecloud.cyberark.cloud
<subdomain>.cyberark.cloud
<subdomain>.privilegecloud.cyberark.cloud
console.privilegecloud.cyberark.cloud
webaccess-<subdomain>.privilegecloud.cyberark.cloud
<Identity-tenant-id>.id.cyberark.cloud
*.amazontrust.com
*.ss2.us
```

#### Connector Management HTTPS / TCP 443
```
<Subdomain>.connectormanagement.cyberark.cloud
connector-management-scripts-490081306957-ap-southeast-2.s3.amazonaws.com
connector-management-assets-490081306957-ap-southeast-2.s3.amazonaws.com
a3vvqcp8z371p3-ats.iot.ap-southeast-2.amazonaws.com
component-registry-store-490081306957.s3.amazonaws.com
```

#### Identity Connector HTTPS / TCP 443
```
*.idaptive.app
*.id.cyberark.cloud
```

#### Identity Connector HTTP / TCP 80
```
privacy-policy.truste.com
ocsp.verisign.com
ocsp.globalsign.com
crl.globalsign.com
secure.globalsign.com
```

### Connector Management

Connector Managment is installed vi the https://subdomain.cyberark.cloud/connectormanagement URL and the installation instructions can be found [here](https://docs.cyberark.com/Product-Doc/OnlineHelp/PrivCloud-SS/Latest/en/Content/Privilege%20Cloud/PrivCloud-ConnectorInstall-CM.htm#RuntheConnectorManagementConnectorinstaller).

Handy command to watch the Connector Managment Logs

Connector Managment logging documentation [here](https://docs.cyberark.com/Product-Doc/OnlineHelp/PrivCloud-SS/Latest/en/Content/Setup/CM_Troubleshooting.htm).

``` powershell
cat -wait -tail 50 'C:\Program Files\CyberArk\Management Agent\Logs\client_log.txt'
```

## App Locker Troubleshooting

This command is useful to determine what applications can’t run because of AppLocker.

``` powershell
Get-WinEvent -LogName "Microsoft-Windows-AppLocker/EXE and DLL" |Where-Object {$_.LevelDisplayName -ne "Information"} | Select-Object -First 200 | Format-Table
```

## Logs

CPM
``` powershell
cat -Wait -Tail 50 .\pm.log
```

PSM


#### Secure Tunnel Logs

``` powershell
cat -Wait -Tail 50 "C:\Program Files\CyberArk\PrivilegeCloudSecureTunnel\logs\privilege-cloud-securetunnel-service.log"
```

## CyberArk Identity: How to configure or restrict attribute matching for the CyberArk Identity Connector

https://cyberark-customers.force.com/s/article/CyberArk-Identity-How-to-configure-or-restrict-attribute-matching-for-the-CyberArk-Identity-Connector