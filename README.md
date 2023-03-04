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
#
### How to use the Get-Help cmdlet
__parameters for the Get-Help cmmdlet;__
* -ShowWindow	--display help in a window format, allowing for selecting views.
* -Examples	--gives some examples about the cmdlet.
* -Detailed	--detailed usecase.
* -Full	--to get full usecase/exapmles.
*	-Online --to get online help.

To view an example or usecase of a cmdlet;  
```ps
Get-Help [-Examples syntax] [any cmdlet u want to know how to use]
```
example;
```ps
Get-Help -Examples Get-ComputerInfo
```
-property is a parameter of the cmd.
```ps
Get-ComputerInfo -Property "*version"
```
search for all services with -DisplayName parameter with space before server keyword and displays them.
```ps
Get-Service -DisplayName "* server*"
```
#
### Display powershell results in full
displays results in full without cutouts in powershell windows:
```ps
Get-service | ft -AutoSize
```

## Powershell Integrated Scripting Environment (ISC)

### Types of Commands in PowerShell:
1. __Functions:__ is a script which has been written in powershell language using the cmdlets provided in powershell
```ps
function myService{
    Get-Service
}
myService
```
you can use the `Import-Module` to import the function to your current terminal, then the `myService` function will become available just like any other cmdlets.


2. __Cmdlets:__ is a program which has been written and compiled using .NET libraries, and integrated into powershell module eg; microsoft powershell environment. Cant be modified.

### Types of Parameters in PowerShell

1. __Optional:__ parameters and its values are surrounded by square brackets example;
```ps
Get-Service [-ComputerName <Str[]>]
```
2. __Mandatory:__ parameters not surrounded by square brackets example;
```ps
Get-Service -DisplayName <Str[]>
```
3. __Common:__ parameters which are common across all cmdlets example;
```ps
Get-EventLog [<CommonParameters]
```
4. __Positional:__ commonly used parameters are often positional, meaning that you can provide a value without typing the parameter's name, provided you put that value in the correct position example;
```ps
Get-EventLog [-LogName] <Str
Get-EventLog -LogName System or Get-EventLog System
```
5. __Switch Parameters:__ A cmdlet in PowerShell may have switch parameters.
    * Some parameters are referred to as switches, like in cmd line switches ex;
    ```ps
		ipconfig /all (here /all is a switch).
    ```
	* They don't accept any value as input
		eg; 
      ```ps
      Get-Service [-DependentServices]
      ```
#
### Getting List of a CmdLet Parameters
To get the entire list of a cmdlet parameters.
```ps
(Get-Command Get-NetIPAddress).Parameters.Values | select name
```

### Alias
When using some cmdlets frequently you can call them by their shortname or alias instead of using their filenames, example; 
```ps
ps instead of Get-Process
```
to get the alias for a particular cmdlet;
```ps
Get-Alias -Definition Get-Process
```
to get full name using the alias name;
```ps
Get-Alias -Name ps
```
### PSDrives
When you run a cmdlet you ultimately manipulate some data in the form of configuration on your computer, right?
All 'Get' cmdlets will retrieve data and all 'Set' cmdlets will change/modify configuration (data).
Powershell comes with different drives by default like our regular data drives C:\, D:\ etc. and additionally you can see Registry, Certificate Store and Environmental Variables etc. also as drives.

```Get-PSDrive``` is the cmdlet to explore them.

Some of these PSDrives will not be visible in Explorer.
You can access registry as a drive (file system).
Drive letters for these drives can be more than one character for ex: HKCU (HKEY_CURRENT_USER), HKLM (HKEY_LOCAL_MACHINE)
#
### Errors
An error occurs when something isn't right with the cmdlet or script execution.
* Errors looks scary, but they are not really.
* errors tells us where exactly the script went wrong with line and character numbers.
* errors are different types in powershell, technically called as Exceptions.


### PowerShell Variables Intro
* A variable name starts with a ```$``` (dollar sign)
* Should not include/use name from pre-defined system variables like;
	```$true, $false, $null, $error```
* To assign a value to variable = will be used
	example;
```ps
$hostname = "Natty"
$hostname --> to print "Natty" to console.
```

### TYPES of variable in POWERSHELL
1. __System Variables:__ Pre-defined variables in powershell.
	we can use the; Get-Variable or ls variable:* --> to view these variables.

2. __User-defined Varaibles:__ defined by a user and can be overwritten.
	example; ```$myName = "Natty", $age = 30```
3. __Environmental Variables:__ Stores info about the OS environment.
	example; ```cd env:\``` then;
			```ls``` to view the environment
		or view them individually;
		```ls temp, ls home, ls os, ls windir```


to exclude those items from the variable list
```ps
Get-Variable -Exclude profile, home
```
you can call any variable listed in 
```ps
Get-Variable $error, $home, $profile
```

to get the data type you can access the data type methods using the tab key
```ps
$myName | Get-Member or $age.GetType()
```
#

### Variable Scope
1. __Local:__ local to the script, mostly user-defined variables.
	example: use the ```Invoke-Command``` to declare a local variable.
```ps	
Invoke-Command -ScriptBlock {$str = 'This is a local String'; "String value: $str"}
```
2. __Global:__ available to all cmdlets, scripts and functions
	you can make a local variable in a script file globally accessible outside that script by; ```$global:myName = "DKing"```
	when the script file is executed, the ```myName``` variable becomes globally accessible in that powershell terminal/session.
#
### Changing variable scope using Scope Modifier
```ps
$global:myName = "DKing"
Invoke-Command -ScriptBlock {$global:str = 'This is a local String'; "String value: $str"}
```
#
### How to get input from users in PowerShell
By using 'Read-Host' cmdlet example; write script in ise:
> as strings:
* ```$f_name = Read-Host 'enter first name'```
* ```$l_name = Read-Host 'enter last name'```
* ```$sp = ' '```
* ```$result = $f_name + $sp + $l_name```
* ```$result```
> as integers:
* ```[int] $f_num = Read-Host 'enter first number'```
* ```[int] $l_num = Read-Host 'enter last number'```
* ```$result = $f_num + $l_num```
* ```$result```