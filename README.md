# PowerShell Notes
#### 7:13pm 23/12/2022
### intro to powershell scripting

##### ping or Test-Connection
``` ps
ping bbc.com
Test-connection bbc.com
```

## Basic Operations in Cmd/PowerShell:
  * copy -> copy file
  * cd -> change dir
  * move -> move file
  * ren -> rename file/folder
  * mkdir -> create folder
  * rmdir -> delete folder
  * del -> delete file
  * type -> just like 'cat' in linux.
  * dir > test.txt --> outputs the entire directory to the test.txt file.
#
#### New-Item -> create a new file. example;
``` ps
New-Item -Path . -Name "test_file1.txt" -ItemType "file" -Value "This is a text string."
New-Item -Path "c:\" -Name "log_files" -ItemType "directory"
```
#

__PowerShell Core:__ receives updates from microsoft.
	to download for windows, linux, macos goto the github 'pwsh' page and download.
	to check version: 
```ps 
$psversiontable
```
> in visual studio code, goto view option, command palette, select enable powershell mode. 
#
### Getting Ip address Information.

```ps
Get-NetIpAddress
```
get all network interface info (dhcp enabled or not).
```ps
Get-NetIpInterface
```
get full ip address info + dns.
```ps
Get-NetIpConfiguration
```
get summarized list of all network adapters.
```ps
Get-NetAdapter
```

### Set New Ip Address:
```ps
New-NetIPAddress -InterfaceIndex [index number] -IPAddress [ip address] -DefaultGateway [gateway ip] -PrefixLength [subnet mask]
```

### Set Dns Address:
```ps
Set-DnsClientServerAddress -InterfaceIndex [index number] -ServerAddresses "8.8.8.8, 8.8.4.4"
```

### Remove Ip address, Gateway and DNS:

```ps
Remove-NetIPAddress -IPAddress [ip address]
```
```ps
Remove-NetRoute -NextHop [default gateway ip address]
```
```ps
Set-DnsClientServerAddress -InterfaceIndex [index number] -ServerAddresses "dns address"
```
#
> __25/01/2023 7:16pm__
* Update powershell to latest: ```Get-Host```
* Update helper cmdlets: ```update-help```
* gets all the cmds in powershell: ```get-help get-command```
* get total number of cmdlets installed in pc: ```(Get-Command).count``` or ```Get-Command -parameterName "*computername*" | measure-object```
#
### How to Search in PowerShell
__searching for specific items:__ the `*` before and after `service` means that there might be words with `*` before and/or after d term `service`, so fetch all.
```ps
Get-Command *service*
```
to get total count of cmds available:
```ps
(Get-Command *servic*).count
```

you can further trim down ur search queries: cmds with -service in them
```ps
(Get-Command *-service*).count
```

searching using module names like; Microsoft Powershell Mgmt or Azure Modules; 
```ps
Get-Command -Module [module name]
```

or if you want to get a list of cmdlets with that particular module name.
```ps
Get-Command -Module Microsoft.PowerShell.Management
```

searching using parameterName or if you want to get list of all cmdlets that has a particular parameterName example; 
```ps
Get-Command [-ParameterName syntax] [the parameter name]
Get-Command -ParameterName ComputerName
```

you can further streamline your search query;
```ps
Get-Command *service* -ParameterName ComputerName
Get-Command -ParameterName NextHop
```