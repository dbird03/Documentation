# PowerShell - Pester Infrastructure Tests

## BeforeAll Block
The BeforeAll block runs before all tests in a script, so it's usually a good idea to define your variables in this block such as the computer name of the remote computer and also to create a CIM session to the remote computer for use throughout the tests.
```powershell
BeforeAll {
    $ComputerName = 'SQLSERVER01'
    $CimSession = New-CimSession -ComputerName $ComputerName
}
```



# Example Tests

## Windows Services

### Test the status/state of a Windows service
```powershell
Describe "Windows Services" {
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
Describe "Windows Services" {
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

## File System

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

## Local Groups

### Test if a local group exists and contains specified security principals
The following example tests if the local group _AppWidget Users_ exists on the computer _AppWidget01_ and contains the group _CONTOSO\SG AppWidget Users_:
```powershell
Describe "Local Groups" {
    Context "AppWidget Users" {
        It "Should exist" {
            $LocalGroups = Invoke-Command -ScriptBlock {Get-LocalGroup} -ComputerName 'AppWidget01'
            $LocalGroups.Name | Should -Contain 'AppWidget Users'
        }
        It "Should contain CONTOSO\SG AppWidget Users" {
            $AppWidgetUsersMembers = Invoke-Command -ScriptBlock {Get-LocalGroupMember -Name 'AppWidget Users'} -ComputerName 'AppWidget01'
            $AppWidgetUsersMembers.Name | Should -Contain 'CONTOSO\SG AppWidget Users'
        }
    }
}
```

## Network

### Test the IP configuration of a network adapter
The following example describes the Ethernet01 network adapter on the AppWidget01 computer. Its IPv4 address should be _172.16.1.120_.
```powershell
Describe "Ethernet0" {
    BeforeEach {
        $ComputerName = 'AppWidget01'
        $CimSession = New-CimSession -ComputerName $ComputerName
    }
    Context "IPv4 Address" {   
        It "Should be 172.16.1.120" {
            $IPAddresses = Get-NetIPAddress -CimSession $CimSession | Where-Object -FilterScript {$_.AddressFamily -eq 'IPv4' -and $_.IPAddress -ne '127.0.0.1'}
            $IPAddresses.IPAddress | Should -Be '172.16.1.120'
        }
    }
}
```

### Test network connectivity to another host
The following example tests if the _AppWidget01_ computer can successfully connect to _172.16.2.30_ on port _80_:
```powershell
Describe "Network Connectivity" {
    Context "172.16.2.30 Port 80" {
        It "Should be successful" {
            $Result = Invoke-Command -ComputerName 'AppWidget01' -ScriptBlock {Test-NetConnection -ComputerName '172.16.2.30' -Port '80'}
            $Result.TcpTestSucceeded | Should -Be 'True'
        }
    }
}
```

## Windows Task Scheduler

### Test if a scheduled task in Windows Task Scheduler is running as a specified security principal
The following example tests if all scheduled tasks created by _CONTOSO\jsmith.admin_ are configured to run as the _CONOTOSO\AppWidgetSvc_ security principal.
```powershell
Describe "AppWidget Scheduled Tasks" {
    BeforeEach {
        $AppWidgetScheduledTasks = Invoke-Command -ComputerName 'AppWidget01' -ScriptBlock {Get-ScheduledTask | Select-Object -Property *,@{name='UserId';expression={$_.Principal.UserId}} | Where-Object {$_.Author -eq 'CONTOSO\jsmith.admin'  -and $_.TaskName -notlike 'User_Feed*'} }
    }
        It "Should run as CONTOSO\AppWidgetSvc" {
            foreach ($AppWidgetScheduledTask in $AppWidgetScheduledTasks) {
                $AppWidgetScheduledTask.UserId | Should -Be 'CONTOSO\AppWidgetSvc'
            }
        }
}
```