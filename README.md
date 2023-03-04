# PowerShell Notes
#### 7:13pm 23/12/2022
### intro to powershell scripting

#### --> powershell commands.
#### -> comments/descriptions
#### --> ping or Test-Connection
``` ps
ping bbc.com
Test-connection bbc.com
```

## Basic Operations in Cmd/PowerShell:
#### copy -> copy file
#### cd -> change dir
#### move -> move file
#### ren -> rename file/folder
#### mkdir -> create folder
#### rmdir -> delete folder
#### del -> delete file
#### type -> just like 'cat' in linux.
#### dir > test.txt --> outputs the entire directory to the test.txt file.
#
#### New-Item -> create a new file. example;
``` ps
New-Item -Path . -Name "test_file1.txt" -ItemType "file" -Value "This is a text string."
New-Item -Path "c:\" -Name "log_files" -ItemType "directory"
```
#

-> __PowerShell Core:__ receives updates from microsoft.
	to download for windows, linux, macos goto the github 'pwsh' page and download.
	to check version: 
```ps 
$psversiontable.
```

-> in visual studio code, goto view option, command palette, select enable powershell mode. 