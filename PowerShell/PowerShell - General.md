# PowerShell - General

### Splatting a script block with Invoke-Command
```powershell
# Store the script block in a variable
$ScriptBlock = {Test-Path 'C:\'}

# Use the variable in the parameter hash table when splatting
$Params = @{
    ScriptBlock     = $ScriptBlock
    ComputerName    = 'SRV01'
}
$Result = Invoke-Command @Params
```

### Force PowerShell to use TLS 1.2
```powershell
[System.Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12
```

***

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

***

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

***

### Dynamically set the $ExportPath variable to the user's desktop
```powershell
$ExportPath = [System.Environment]::GetFolderPath([System.Environment+SpecialFolder]::Desktop) # This sets the export path to the user's desktop
```

***

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

***

### Combine multiple CSVs with the same header

Reference: [Combine CSV Files with Windows 10 PowerShell](https://sites.pstcc.edu/elearn/instructional-technology/combine-csv-files-with-windows-10-powershell/)

```powershell
Get-ChildItem -Filter *.csv | Select-Object -ExpandProperty FullName | Import-Csv | Export-Csv .\combinedcsvs.csv -NoTypeInformation -Append
```

***

### Use calculated properties to rename column names when importing a csv file
Using this trick is useful when you do not have the ability to rename the column names in a csv.
```powershell
# Csv file contains three columns: User, O365License, ServicePlan
$Path = "C:\Temp\MicrosoftTeamsUsers.csv"

# Create a hash table of calculated properties to rename the column name in the csv file (Expression) to the new property name (Name)
$CalculatedProperties = @(
    @{ Name = 'UserPrincipalName'   ; Expression = { $_.'User' } }
    @{ Name = 'AccountSkuId'        ; Expression = { $_.'O365License'  } }
    @{ Name = 'ServicePlanName'     ; Expression = { $_.'ServicePlan'  } }
)

# Import the csv and use Select-Object with the hash table to rename the property names
$TeamsUsers = Import-Csv -Path $Path | Select-Object -Property $CalculatedProperties
```

### Line break URL for easy readability
This technique is useful for troubleshooting OAuth flows with Azure AD. This will start a new line for each & character for easy readability.
```powershell
$URL = 'https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize?client_id=6731de76-14a6-49ae-97bc-6eba6914391e&response_type=code&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F&response_mode=query&scope=https%3A%2F%2Fgraph.microsoft.com%2Fmail.read%20api%3A%2F%2F&state=12345&code_challenge=YTFjNjI1OWYzMzA3MTI4ZDY2Njg5M2RkNmVjNDE5YmEyZGRhOGYyM2IzNjdmZWFhMTQ1ODg3NDcxY2Nl&code_challenge_method=S256'

$URL -split '(?=&)'
```
#### Example Output:
```powershell
https://login.microsoftonline.com/{tenant}/oauth2/v2.0/authorize?client_id=6731de76-14a6-49ae-97bc-6eba6914391e
&response_type=code
&redirect_uri=http%3A%2F%2Flocalhost%2Fmyapp%2F
&response_mode=query
&scope=https%3A%2F%2Fgraph.microsoft.com%2Fmail.read%20api%3A%2F%2F
&state=12345
&code_challenge=YTFjNjI1OWYzMzA3MTI4ZDY2Njg5M2RkNmVjNDE5YmEyZGRhOGYyM2IzNjdmZWFhMTQ1ODg3NDcxY2Nl
&code_challenge_method=S256
```