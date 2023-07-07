---
# the default layout is 'page'
icon: fas fa-info-circle
order: 5
---


CyberArk Privilege Cloud installation cheat sheet.

## 01 - Download Links

CyberArk Privilege Cloud Tools

https://cyberark-customers.force.com/mplace/s/#--Privilege+Cloud+Tools

CyberArk Privilege Cloud

https://cyberark-customers.force.com/mplace/s/#software#---Name-CyberArk%20Privilege%20Cloud

CyberArk Identity connector - See section 14 below


## 02 - Connector Install

Connector Hardware Requirements

https://docs.cyberark.com/Product-Doc/OnlineHelp/PrivCloud-SS/Latest/en/Content/Privilege%20Cloud/PrivCloud-sys-req-connector.htm#Hardwarerequirements

Outbound traffic network and port requirements

https://docs.cyberark.com/Product-Doc/OnlineHelp/PrivCloud-SS/Latest/en/Content/Privilege%20Cloud/PrivCloud-sys-req-networks.htm

### Connector Install Input Requirements

Customer ID (Used in Pre-Requisites Script)

```
XXXXX-XXXXXX-XXXXXX-XXXXX format
```

Privilege Cloud Web Portal
```
https://<subdomain>.cyberark.cloud/privilegecloud
```

Privilege Cloud Vault Address
```
vault-<subdomain>.privilegecloud.cyberark.cloud
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


## 06 - Loadbalancer - PSM Health Check

### Requirements
Downlaod the PSM Health Check Script
https://cyberark-customers.force.com/mplace/s/#--PSM+Health+check

Download .NET Core Runtime 6.0.X
https://dotnet.microsoft.com/en-us/download/dotnet/thank-you/runtime-aspnetcore-6.0.12-windows-hosting-bundle-installer

### Run the HealthCheck.ps1 script
``` powershell
.\HealthCheck.ps1 -installPath "E:\Program Files (x86)\Cyberark\PSM"
```
or
Use this command when CyberArk has been installed on a different drive letter.
``` powershell
.\HealthCheck.ps1 -installPath "E:\Program Files (x86)\Cyberark\PSM"
```


### Test Health Check
``` powershell
Invoke-WebRequest -uri https://fqdn/psm/api/health
```

## 07 - PSMP Installation

#### Download PSMP Installation Script

Use the follwoing command to downlaod the psmpwiz1xxx.sh script. Make sure it is placed in the same folder as PrivilegedSessionManagerSSHProxy-RHELinux8-Intel64-Rls-v1X.X.zip

```
cd /home/<username>/PSMP/
wget https://raw.githubusercontent.com/pCloudServices/psmpwiz/main/psmpwiz1310.sh
```

#### Test Network Connectivity

The follwoing commands will test outbound connectivity from the PSMP server to Privilege Cloud.

```sh
curl -v telnet://<subdomain>.privilegecloud.cyberark.cloud:443

curl -v telnet://<subdomain>.cyberark.cloud:443

curl -v telnet://<subdomain>.privilegecloud.cyberark.cloud:1858

curl -v telnet://<CyberArkIdentityID>.id.cyberark.cloud:443
```

#### Create proxymng User and proxymanagers Group

```
useradd proxymng
passwd proxymng
groupadd proxymanagers
usermod -a -G proxymanagers proxymng
```
#### Modify SSHD Config File
```
vi /etc/ssh/sshd_config
```

Add the following line to the bottom of /etc/ssh/sshd_config
```
AllowGroups proxymanagers PSMConnectUsers
```
Restart the SSH service
```
service sshd restart
service sshd status
```

#### Change Directory to /home/localadmin/PSMP/
```
cd /home/localadmin/PSMP/
```
#### Unzip CyberArk Files
```
ls
unzip PrivilegedSessionManagerSSHProxy-RHELinux8-Intel64-Rls-v1X.X.zip
unzip psmpwiz1XX.zip
```
#### Set the following files to executable
```
ls -ltr
chmod 755 CARKpsmp-1X.xx.xx.xx.rpm
chmod 755 CreateCredFile
chmod 755 psmpwiz1XX.sh
```

#### Run the PSMP install script
```
./psmpwiz1XX.sh
```
  



PSM for SSH requirements
https://docs.cyberark.com/Product-Doc/OnlineHelp/PrivCloud/Latest/en/Content/Privilege%20Cloud/PrivCloud-sys-req-PSM-SSH.htm?tocpath=Setup%7CSystem%20requirements%7C_____3

##

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

## 11 - RSAT Tools

The follwoing notes are based on the [How to use Add PSMApplication](https://cyberark-customers.force.com/s/article/How-to-use-Add-PSMApplication) documenation.

Step 1: [Download Privilege Cloud Tools](https://cyberark-customers.force.com/mplace/s/#a352J000000GWAZQA4-a392J000002tNgLQAU) and copy to C:\CyberArk folder on the connector soerver.

Step 2: Navigate and extract the Add-PSMApps

Step 3: Run a Powershell session as Administrator and run the follwoing sample command.

This command adds ADUC,GPMC and a GenericMMC (RSAT) console 
```Powershell
.\Add-PSMApps.ps1 -Application "ADUC","GPMC","GenericMMC" -MscPath "C:\PSMApps\RSAT.msc" -ComponentName "RSAT" -ComponentDisplayName "RSAT" -HTML5 "OffByDefault" -PortalUrl "https://subdomain.privilegecloud.cyberark.cloud"
```

This command adds ADUC,GPMC only

```Powershell
.\Add-PSMApps.ps1 -Application "ADUC","DNS" -HTML5 "OffByDefault" -PortalUrl "https://subdomain.privilegecloud.cyberark.cloud"
```

## 12 - CyberArk PSM with Microsoft Edge

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
BrowserPath - Value = C:\Program Files (x86)\Microsoft\Edge\Application\msedge.exe
RunValidations - Vaule = No
EnableTrace - Value = Yes or No


## 13 - Google Chrome Download and Install

This script downloads Google Chrome 64bit and executes the msi installer.
### 64bit Google Chrome
``` powershell
Start-BitsTransfer "https://dl.google.com/edgedl/chrome/install/GoogleChromeStandaloneEnterprise64.msi" $env:TEMP\GoogleChromeStandaloneEnterprise64.msi
$chromeinstaller = "$env:TEMP\GoogleChromeStandaloneEnterprise64.msi"
msiexec.exe /package $chromeinstaller
```
### 32bit Google Chrome
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
    <Application Name="GoogleChromeDriver" Type="Exe" Path="C:\Program Files (x86)\CyberArk\PSM\Components\chromedriver.exe" Method="Hash" />    
```


## 16 - CyberArk Identity: How to configure or restrict attribute matching for the CyberArk Identity Connector

https://cyberark-customers.force.com/s/article/CyberArk-Identity-How-to-configure-or-restrict-attribute-matching-for-the-CyberArk-Identity-Connector