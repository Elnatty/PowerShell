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



### Variables in Expressions
* if variable is enclosed in double quotation (""), then value of variable is returned.
* if variable is enclosed in single quotation (''), then name of variable is returned.
Example:
```ps
$l_num = 3
Write-Host 'value of var is $l_num' -> outputs value of var is $l_num
Write-Host "value of var is $l_num" -> outputs value of var is 3
```
====================================================================================================
--> Types of Variables:
1) User-defined Variables
2) Automatic Variables: eg;
	$$ -> returns last run cmd.
	$? -> returns result of last cmd execution either True or False.
	$_ -> current object in our pieline. eg; like tenary operator format in javascript
		1..10 | foreach{$_}
	$PID -> process id
	$PsCulture -> language
	$pwd -> present working director
3) Preference Variables:
	$MaximumHistoryCount -> changes the value of previous ran cmdlets for current powershell session. $MaximumHistoryCount = 10
	$ErrorView.
	

===================================Properties of a Variable in PowerShell===========================
--> $error | Get-Member | ft Name,MemberType -> getting the properties of the error variable.
--> $error[0].InvocationInfo -> you can access any of the properties for each error index.


====================directory/files manipulation - getting informations about files=================
exapmles: cd c:\testing -> has 1 file named hosts.txt.
	our script to manipulate:
	$ls = Get-ChildItem -> create a variable that list the items in the folder.
	$ls | Get-Member | ft Name,MemberType -> gets all properties for the variable $ls, in order to access these properties, you have to select items in the folder by indexing i.e; $ls[0].CreationTime e.t.c.
	


============================================Arrays=============================================
--> Declaring an array;
	$array1 = 10,20,30,40,50 -> declaring an array
	$fruits = @("Apple","Orange","Kiwi") -> declaring an array
--> Values are accessed using indexing; ie, $array1[0] -> outputs 10
--> $array1 += 60 or $array1 += 'net' -> adds an item to the end of the array.
--> $array.setvalue(5, 1) -> change the value at index 1 to 5.
--> $array1[-1] = 500 -> changes the last value from 50 to 500.
--> Clear-Variable -Name myName -> clears the myName variable from d variable list.
--> New-Variable -Name val1 -Value "This is value 1" -> creates a new variable (lengthy method)
--> [System.Collecction.ArrayList]$newArray = 1..5 -> defining a standard non-fixed array.
	$newArray.Remove(4) -> removes 4 from the newArray.
	$newArray.RemoveAt(1) -> removes value at index 1.
	$newArray.Add(10) -> adds 10 to the newArray.



====================================================================================================
=====================================Commands Piping===============================================
--> A way of connecting commands in powershell is called Piping.
--> We use | symbol to pipe two or more cmds,
	eg; dir | more
--> Piping provides a way for one cmd to pipe its output to another cmd as input,
	eg; Get-Service | export-csv c:\temp\services.csv
	    Get-service -Name "Windows update" | stop-service

=========================How Piping Works?====================================================
--> Powershell accepts inputs through "parameters".
--> Powershell uses a technique called "pipeline parameter binding"
--> Lets consider this example;
	-> Get-content c:\temp\services.txt | get-service
	-> get-content reads the content of textfile, which has service named listed.
	-> We are piping that information as input to get-service cmdlet.
	-> Now one of the parameter from get-service cmdlet should pickup this information as input and process it.


====Methods used by Powershell to understand Output produced by one cmd to the 2nd cmd====
1) Input 'ByValue' method:
	both data type of each output must be same. pipe the output to Get-Member cmdlet to get data type.
		example: Get-Content C:\testing\test.txt | Get-Member
			or
			(Get-Content C:\testing\test.txt | Get-Member).gettype()

2) Input 'ByPropertyName' method:
	Get-Process explorer | select -Property * --> this gets the process 	properties.
	Get-Service WSearch | Get-Member -MemberType Property,AliasProperty --> this exclude methods 		and events from  the MemberType column.

	Generic Piping example:
		Get-Process | Out-File .\folder\test.txt -->> outputs all processes into				the text.txt file.

	Get-Service | Export-Csv .\Test_playGround\222.csv -->> outputs all services as a csv file.

	Get-Service | ConvertTo-Html > .\222.html --> converts to html, then 	output into the 		.html file.



=====================================================================================================
=====================Creating a Script to silently install apps in windows=========================
--> if multiple apps to install, you can put them in a share folder or any folder then create a .csv file with their fullname and silent install value (search how to install that file silently with powershell). example: in test folder, we have a pkgs.csv file, inside the csv file (ChromeStandaloneSetup64.exe,/silent /install).

--> you can then import to  powershell with (specify a delimiter -> "," and header for the csv file 		-> "install","value"):
	Import-Csv \\path_to_file\\ -Delimiter "," -Header "install","value" --> outputs the apps in 		a nicely formatted view in powershell.

--> 
to be continued...



====================================================================================================
===========================================PowerShell Objects=======================================
--> view all parameter of a cmdlet.

--> (Get-Command Get-Service).Parameters.Values | select name -> outputs all parameters for the Get-Service cmdlet.
	or
	Help Get-NetIPAddress -Parameter * | select name
	or,
	you can view by -> Get-Service and press 'tab key' to iterate through them.
	or,
	you can view in a formatted list view by -> (Get-Command Get-Service).Parameters.Values | select name

--> Get-Service  WSearch | select * -> outputs all the parameters for 'wsearch'
	or,
	Get-Service  WSearch | select -Property *
	or,
	Get-Service | Select-Object DisplayName

--> Get-Service | select -Property * -> outputs full properties of an object.


============================================Real life usecase=======================================
====================================================================================================
How to find suffs you dont know exact name of in powershell......................
use the powershell ise environment:
example;

--> accessing properties of an object using variable declaration:
$service = Get-Service WSearch
$service.Name -> outputs Windows Search
or,
Get-Service WSearch | Select-Object -Property Name


-> You can stop a service many waye: example;
Stop-Service -Name $service

-> you can also get object by using the 'Where-Object' cmdlet
example;
-> 'Where-Object' based on "matching" keywords.
Get-Service | Where-Object -Property Name -Match 'anydesk' -> outputs the 'anydest' service
or,
Get-Service | Where-Object {$_.DisplayName -match 'backup'}
Get-Process | Where-Object {$_.Name -match 'search'}

-> 'Where-Object' based on "exact" keywords.
Get-Service | Where-Object -Property Name -EQ 'wsearch' -> use this if you know the actual name.
or,
Get-Process | Where-Object {$_.Name -eq 'searchapp'}



=======================================User Defined Objects=========================================
You can also define/create your own user defined objects............................................
example:
$obj = New-Object -TypeName psobject -> to create
$obj | Add-Member -Type NoteProperty -Name Name -Value Nathan -> to define its objects
$obj | Add-Member -Type NoteProperty -Name Age -Value 24 -> another object.
$obj.name -> to access the name method.



===========================================PowerShell Operators=====================================
====================================================================================================
--> The term operator refers to one or more characters in a command or expression that powershell interprets in a specific way.
--> 	1) Arithmetic (+, -, *, /, modulus %)
	2) Logical: -and, -or, -not or !, xor - (only one should be true)
		$number
		($number -eq 100) -or ($number -ne 20) -> outputs True. 
		Equality -> -eq, -ne, -lt, -ge, -le. example: 100 -eq 19 -> outputs false.
		Match -> -like, -notlike, -match, -notmatch. example: 'anonymous' -match 'anon' -> outputs true.
	3) Comparison: -contains, -notcontains example:
		$a = 'one','two','three','four'
		$a -contains 'one' -> outputs True.
-replace example:
	"My name is Nathan" -replace 'Nathan',"Hannah Alabi"
	"Test value" -replace "Te","Be"
	1..5 | foreach {New-item -Name $("file" + $_ + ".txt")} -> creates 5 .txt files (file1.txt-file5.txt).
	Get-children *.txt | foreach {Rename-Item $_.Name -NewName $($_.name -replace '.txt','.log')} -> rename all files to (file1.log - file5.log).

	4) SPlit and Join: -split, -join example;
		-split -> convert string to array. $name = 'My name is not Natty'
			$name.split(' ') -> returns an array.
			$name -join " " -> returns a string
	5) Type Operators: -is, -isnot example;
		$a = 2
		$a -is [int] -> outputs True
	6) Redirection: > -> write an output to a file
			>> -> append an output to a file
		Get-Children > .\path 
	7) Assignment / unary: =,+=,-=,*=,/=,++,--,%=
		$num = 100
		$num++ -> outputs 101

--> example: '+'
	powershell uses this for addition of integers or to join strings.
	$a = 1; $b = 2; $c = $a + $b -> outputs 3
	"Nathan" + "Alabi" -> Nathan Alabi

	8) Tenary Operators:
		2 -eq 3 ? "True" : "False"


===========================================Loops in PowerShell=====================================
====================================================================================================
foreach, for, while,do-while, do-until loops.

--> foreach loop:
$number = 1,2,3,4,5
foreach($num in $number) {
    $n = $num + 10
    Write-Host $n
}

--> for loop:
$numbers = 10,20,30,40,50
for ($i=0; $i -le $numbers.Count; $i++){
    $numbers[$i] * 2
}

$count = 0
for ($i=0; $i -lt 8; $i++){
    "$count Apple"; $count++
}

$count = 0
for (($i=0),($m=10); $i -lt 10 -and $m -ge 10; $i++, $m++){
    "$i is $m"
}



===============================================================================================================================Invoke Command in PowerShell============================================
--> Is used to execute script on a local or remote computer. example;
	Invoke-Command -ScriptBlock {$str = 'This is a local String'; "String value: $str"}



====================================================================================================================================While Loops========================================================
-->
$list = 15..18
$i = 0
while ($i -le $list.count){
    $list[$i]; $i++
} 

============================================================================================================================================Error Handling in PS========================================

--> $error -> the $error variable holds all errors encountered in powershell for that current session. Which is an array.
--> $error[0].gettype() -> shows the error type for the 1st error (using indexing).
--> $error.exception -> shows the description of each all errors in the array.
--> $error[0].Exception.GetType().name or .fullname -> gives the name of the 1st error in d array.
example:
	Test-Connection AZTECMFB -> throws an error
	$error[0].Exception.GetType().fullname -> fetches the error fullname ie; System.Net.NetworkInformation.PingException
	or
	$error[0].Exception.GetType().name -> fetches the error name ie; PingException.
--> $error.Clear() -> clears all errors for current powershell session.


===================================reallife usecase of try and catch================================
--> exapmle 1;
Try {
	ls -Name gg -ErrorAction Stop
}
catch{
	Write-Host "`nError Message:" $_.exception.message
} -> outputs: Error Message: Cannot find path 'C:\testing\gg' because it does not exist.

--> example 2;
try {
    Test-Connection AZTECMFB -ErrorAction Stop
} catch {
    Write-Host "`nError Message:" $_.exception.message
}



=============================================================================================================================================PowerShell Remoting========================================
--> Terminologies & Components of PS Remoting:
	-> Web Services for mgmt (WS-MAN) -> HTTP-5985 and 5986
	-> Firewall & Security
	-> Windows Remote Mgmt (WinRM)
	-> Authentication
	-> Endpoint
	-> Listener


=================================Preparing Client PC's for PowerShell Remoting======================
====================================================================================================
--> Hostname -> get name of client pc workstation.
1)--> Enable-PSRemoting -> enable powershell remoting on PC's

2)--> Get-PSSessionConfiguration | select name -> checking enabled endpoints, or verifying ps remoting is enabled

-->  Get-ItemProperty -Path 'HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System' -Name "LocalAccountTokenFilterPolicy" -> check if the registry exists.

3)--> new-ItemProperty -Path 'HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System' -Name "LocalAccountTokenFilterPolicy" -value '00000001' -propertytype Dword -Force -> adding the key into the registry (do this on client PC).

4)--> set-Item WSMan:\localhost\Client\TrustedHosts -Value "ip_add_of_client_pc" -Force -Concatenate -> add the client PC to the domain controller trusted host lists.

5)--> Get-Item WSMan:\localhost\Client\TrustedHosts -> checking if new client has been added to DC trusted host lists.

====================================================================================================
=========================================One-One PS Remoting========================================
--> helpful when working with single remote machine.
--> commands;
	Enter-PsSession eg; Enter-PSSession -ComputerName "ip_add_of_client"
	Exit-PsSession