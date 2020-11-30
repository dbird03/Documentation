# PowerShell - General

### Easily convert block of text into an array
Reference: (http://pleasework.robbievance.net/howto-easily-convert-block-of-text-into-an-array-in-powershell/)
```powershell
$Array = @" 
item 1
item 2
item 3
item 4
"@ -split "`n" | ForEach-Object { $_.trim() }
```
##### Example:
```powershell
$Processes = @" 
explorer
Right Click Tools Desktop
spoolsv
OneDrive
"@ -split "`n" | % { $_.trim() }

$Processes | % {Get-Process $_}

Handles  NPM(K)    PM(K)      WS(K)     CPU(s)     Id  SI ProcessName                                                                                                                                                
-------  ------    -----      -----     ------     --  -- -----------                                                                                                                                                
   3726     125   183340     153284     551.09   8680   1 explorer
    754      59   142072     141372       5.16   7632   1 Right Click Tools Desktop                                                                                                                                                   
    830      50    22568      27060               388   0 spoolsv                                                                                                                                                    
    742      53    20900      16980      41.81   8144   1 OneDrive
```


***

### Use an array in an Active Directory cmdlet -Filter parameter
```powershell
# Create the array
$Array = @"
Finance
Human Resources
Sales
"@ -split "`n" | ForEach-Object { $_.trim() }

# Create the filter string. Note the property and operators in parenthesis match the property and operator outside.
$FilterString = "Department -like '$($Array -join "' -or Department -like '")'"

# Use the filter string
Get-ADUser -Filter $FilterString
```

### Progress bar in a foreach loop
```powershell
$Counter = 1
foreach ($item in $collection)
{
    Write-Progress -Activity "Querying through $($collection)" -Status "Querying $($item)" -PercentComplete (($Counter / $collection.Length) * 100)
    # Put your code for each $item here
    $Counter += 1
}
```
##### Example:
```powershell
$Result = @()
Write-Host "Querying Active Directory security groups..."
$Counter = 1
foreach ($Group in $Groups)
{
    Write-Progress -Activity "Querying Active Directory security groups..." -Status "Querying $($Group.Name)" -PercentComplete     (($Counter / $Groups.Length) * 100) 
    $Result += Get-ADGroupMember -Identity $Group -Recursive | Get-ADUser -Properties Name | Select @{Label="Active Directory Security Group Name";Expression={$Group.Name}}, Name, Enabled
    $Counter += 1
}
```
***

### Get Windows Updates/Patches
```powershell
Get-Hotfix -ComputerName SEA-SRV-01 | Select-Object HotfixID, Description, InstalledOn | Sort-Object InstalledOn
```

### Dynamically set the $ExportPath variable to the user's desktop
```powershell
$ExportPath = [System.Environment]::GetFolderPath([System.Environment+SpecialFolder]::Desktop) # This sets the export path to the user's desktop
```

### Test if a module is installed and available for import
```powershell
$Module = "Microsoft.Online.SharePoint.PowerShell"

if (Get-Module -ListAvailable -Name $Module)
{
    Write-Host "Module $($Module) is installed"
}
else
{
    throw "Module $($Module) is NOT installed" # Throw an error message and do not continue the rest of the code
}
```
***

### Select only the first object instead of all objects
```powershell
Get-Process | Select-Object -First 1
```
***


### Why $null should be placed on the left side of equality comparisons.

Reference: [PossibleIncorrectComparisonWithNull](https://github.com/PowerShell/PSScriptAnalyzer/blob/development/RuleDocumentation/PossibleIncorrectComparisonWithNull.md) | PSScriptAnalyzer on GitHub

> $null is a scalar. When the input (left side) to an operator is a scalar value, comparison operators return a Boolean value. When the input is a collection of values, the comparison operators return any matching values, or an empty array if there are no matches in the collection. The only way to reliably check if a value is $null is to place $null on the left side of the operator so that a scalar comparison is performed.

> PowerShell will perform type casting left to right, resulting in incorrect comparisons when $null is cast to other scalar types.

Example:
```powershell
PS 14:15:15 PM C:\> if (@() -eq $null) { 'true' }else { 'false' }
false
PS 14:15:26 PM C:\> if (@() -ne $null) { 'true' }else { 'false' }
false
PS 14:15:30 PM C:\> if ($null -eq @()) { 'true' }else { 'false' }
false
PS 14:15:31 PM C:\> if ($null -ne @()) { 'true' }else { 'false' }
true
```

### Find certificates

Reference: [Find certificates using PowerShell](http://www.herlitz.nu/2017/11/09/find-certificates-using-powershell/) | Eric Herlitz

```powershell
# List any certificates with a FriendlyName containing DigiCert
dir cert: -Recurse | Where-Object { $_.FriendlyName -like "*DigiCert*" }

# List any certificates with a thumbprint containing 0563B8630D62D75ABBC8AB1E4B
dir cert: -Recurse | Where-Object { $_.Thumbprint -like "*0563B8630D62D75ABBC8AB1E4B*" }

# List any certificates that isn't valid after the 31 Dec 2018
dir cert: -Recurse | Where-Object { $_.NotAfter -lt (Get-Date 2018-12-31) }

# List any certificates that will expire the upcomming year, from now and one year ahead
dir cert: -Recurse | Where-Object { $_.NotAfter -gt (Get-Date) -and $_.NotAfter -lt (Get-Date).AddYears(1) }
```

### Combine multiple CSVs with the same header

Reference: [Combine CSV Files with Windows 10 PowerShell](https://sites.pstcc.edu/elearn/instructional-technology/combine-csv-files-with-windows-10-powershell/)

```powershell
Get-ChildItem -Filter *.csv | Select-Object -ExpandProperty FullName | Import-Csv | Export-Csv .\combinedcsvs.csv -NoTypeInformation -Append
```