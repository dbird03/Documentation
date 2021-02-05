# PowerShell - Windows


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