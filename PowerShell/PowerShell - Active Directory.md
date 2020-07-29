# PowerShell - Active Directory

### Copy members from one group to another group
##### Example 1:
```powershell
# This copies members from 'Old Group' to 'New Group'
Add-ADGroupMember -Identity 'New Group' -Members (Get-ADGroupMember -Identity 'Old Group' -Recursive)
```