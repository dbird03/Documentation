# PowerShell - Active Directory

### Copy members from one group to another group
##### Example:
```powershell
# This copies members from 'Old Group' to 'New Group'
Add-ADGroupMember -Identity 'New Group' -Members (Get-ADGroupMember -Identity 'Old Group' -Recursive)
```

### Using the -Filter parameter with wildcards and variables
In the following example, there is a collection called $DocuSignUser and objects have a UserEmail property formatted like "jsmith@contoso.com". We want to use the Get-ADUser cmdlet to lookup AD users whose proxyAddresses attribute is like this email. Since the proxyAddresses attribute is formatted like SMTP:jsmith@contoso.com", we need to use a wildcard.
##### Example:
```powershell
Get-ADUser -Properties proxyAddresses -Filter "proxyAddresses -like '*$($DocuSignUser.UserEmail)'"
```