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
$psversiontable.
```
> in visual studio code, goto view option, command palette, select enable powershell mode. 
#
#
### Getting Ip address Information.

--> get all network card address info.
```ps
Get-NetIpAddress
```
--> get all network interface info (dhcp enabled or not).
```ps
Get-NetIpInterface
```
--> get full ip address info + dns.
```ps
Get-NetIpConfiguration
```
--> get summarized list of all network adapters.
```ps
Get-NetAdapter
```

Set New Ip Address:
New-NetIPAddress -InterfaceIndex [index number] -IPAddress [ip address] -DefaultGateway [gateway ip] -PrefixLength [subnet mask] --> set a new ip address and gateway.

Set Dns Address:
Set-DnsClientServerAddress -InterfaceIndex [index number] -ServerAddresses "8.8.8.8, 8.8.4.4" --> set a dns ip address.

Remove Ip address, Gateway and DNS:
Remove-NetIPAddress -IPAddress [ip address]
Remove-NetRoute -NextHop [default gateway ip address]
Set-DnsClientServerAddress -InterfaceIndex [index number] -ServerAddresses "dns address"