# PowerShell - WMI & CIM

### Get CIM Methods and CIM parameters
```powershell
# Get the CIM class
$class = Get-CimClass -ClassName Win32_Service
# Get the methods of the CIM class
$class.CimClassMethods
# Get the parameters of a specific method (ex: Change method)
$class.CimClassMethods[“Change”].Parameters
```