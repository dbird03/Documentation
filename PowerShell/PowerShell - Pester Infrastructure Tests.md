# Pester Infrastructure Tests

## BeforeAll Block
The BeforeAll block runs before all tests in a script, so it's usually a good idea to define your variables in this block such as the computer name of the remote computer and also to create a CIM session to the remote computer for use throughout the tests.
```powershell
BeforeAll {
    $ComputerName = 'SQLSERVER01'
    $CimSession = New-CimSession -ComputerName $ComputerName
}
```
## Example Tests
### Test if a Windows service status is running
```powershell
Describe "Services" {
    Context "SQL Server Service" {
        BeforeEach {
            $Params = @{
                ClassName = 'Win32_Service'
                ComputerName = 'SQLSERVER01'
                Filter = "Name like 'MSSQLSERVER' "
            }
            $Service = Get-CimInstance @Params
        }
        It "Should be running" {
            $Service.State | Should -Be 'Running'
        }
    }
}
```
### Test if a Windows service is running as a specified security principal
```powershell
Describe "Services" {
    Context "SQL Server Service" {
        BeforeEach {
            $Params = @{
                ClassName = 'Win32_Service'
                ComputerName = 'SQLSERVER01'
                Filter = "Name like 'MSSQLSERVER' "
            }
            $Service = Get-CimInstance @Params
        }
        It "Should be running as SQLServerServiceAccount@contoso.com" {
            $Service.StartName | Should -Be 'SQLServerServiceAccount@contoso.com'
        }
    }
}
```
### Test if a folder structure/path exists
```powershell
Describe "D: Drive Folder Structure" {
    Context "D:\SQL Backup folder exists" {
        It "Should be true" {
            $Path = Invoke-Command -ScriptBlock {Test-Path 'D:\SQL Backup'} -ComputerName 'SQLSERVER01'
            $Path | Should -Be 'True'
        }
    }
}
```
### Test if a backup file was created in the last 24 hours
```powershell
Describe "SQL Server Backup" {
    Context "Daily Backup" {
        It "Most recent backup should be less than 24 hours old" {
             # These commands come from the Help files for Get-TimeSpan (https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/new-timespan?view=powershell-5.1)
            $MostRecentBackup = Get-ChildItem -Path 'D:\SQL Backup' | Sort-Object -Property LastWriteTime -Descending | Select-Object -First 1
            $TimeSpan = $MostRecentBackup | New-TimeSpan
            $TimeSpan.TotalDays | Should -BeLessOrEqual 1
        }
    }
}
```