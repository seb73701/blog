## [ PowerShell ] - Template de script
Code :

```powershell
<#
.Synopsis
 
.DESCRIPTION
   Check the ...
   ...
   ...
 
   Pre-requisites:
    * Nothing
 
.NOTES
   Created by: Firstname Name
   Modified: 2019-09-01 
 
   Version 1.00
 
   Changelog:
    * Fixed for ...
    * ...
    * Initial script
 
.EXAMPLE
   .\mon_script.ps1
   Checks ...
.EXAMPLE
   .\mon_script.ps1 -Arg1 value1
   Check ...
.EXAMPLE
   .\mon_script.ps1 -Arg1 value1 -Arg2 value2
   Check ...
.LINK
    https://
#>
#Requires -Version 2.0
[CmdletBinding()]
Param
(
    # Name of the server, defaults to local
    [string]$Arg1,
    [int]$Arg2
    #...
)
 
Begin
{
    #...
}
 
Process
{
    try {
        #...
    }
    # Catch all other exceptions
    catch {
        #...
    }
}
 
End
{
    #...
}
```