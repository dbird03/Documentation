# PowerShell - Active Directory
These commands should be used after importing a PSSession to the local Exchange server or Exchange Online.

***
## General Notes

#### Find an email address and what type of recipient it is (User Mailbox, Shared Mailbox, Mail Contact, etc.)
```powershell
Get-Recipient unknownemailaddress@contoso.com
```

***

## Running PowerShell Jobs in Exchange Online
[Running PowerShell cmdlets for large numbers of users in Office 365](https://techcommunity.microsoft.com/t5/Exchange-Team-Blog/Running-PowerShell-cmdlets-for-large-numbers-of-users-in-Office/ba-p/604280)

[Using variables with Invoke-Command in Remote PowerShell sessions in Exchange Online](https://www.michev.info/Blog/Post/1687/using-variables-with-invoke-command-in-remote-powershell-sessions-in-exchange-online)


***
## Mailboxes

#### Set an Out of Office on a mailbox
```powershell
Set-MailboxAutoReplyConfiguration -Identity user@contoso.com -AutoReplyState Enabled -InternalMessage "I am currently on vacation until September 1st. Please contact manager@contoso.com or call 111-111-1111." -ExternalMessage "I am currently on vacation until September 1st. Please contact manager@contoso.com or call 111-111-1111."
```

#### View the Spam/Junk mail settings on a mailbox
```powershell
Get-MailboxJunkEmailConfiguration -Identity "user@contoso.com"
```

#### View the Trusted Senders/Blocked Senders list
```powershell
Get-MailboxJunkEmailConfiguration -Identity "user@contoso.com" | Select-Object -ExpandProperty TrustedSendersandDomains

Get-MailboxJunkEmailConfiguration -Identity "user@contoso.com" | Select-Object -ExpandProperty BlockedSendersandDomains
```

#### View a mailbox's inbox rules
```powershell
Get-InboxRule -Mailbox "user@contoso.com"

Name                Enabled Priority RuleIdentity        
----                ------- -------- ------------        
Email from the boss True    1        4387192879655682049 
Forward emails      True    2        15346437689326960641
```

#### View a specific inbox rule's details
```powershell
Get-InboxRule -Mailbox "user@contoso.com" -Identity "Forward emails" | Select-Object -Property *
```

#### Apply retention policy to a mailbox
```powershell
Set-Mailbox -Identity "user@contoso.com" -RetentionPolicy "Three Year Retention Policy"
```

#### Enable archive on a mailbox
```powershell
Enable-Mailbox -Identity "user@contoso.com" -Archive
```

#### Grant Full Access permission to shared mailbox
```powershell
Add-MailboxPermission -Identity "sharedmailbox@contoso.com" -User "user@contoso.com" -AccessRights FullAccess
```

#### Grant Send As permission to shared mailbox
```powershell
Add-RecipientPermission -Identity "sharedmailbox@contoso.com" -Trustee "user@contoso.com" -AccessRights SendAs -Confirm:$false
```

#### View the permissions on a mailbox and filter out system accounts and Exchange roles
```powershell
Get-MailboxPermission -Identity "sharedmailbox@contoso.com" |
Where-Object {$_.user.tostring() -ne "NT AUTHORITY\SELF" -and $_.user.tostring() -notlike "S-1-5-21*" -and $_.IsInherited -eq $false}
```

#### Get all mailboxes a user has access to
```powershell
Get-Mailbox -ResultSize Unlimited | Get-MailboxPermission -User jsmith
```

***
## Calendars & Resource Rooms
#### View Permissions on Mailbox Calendar
```powershell
Get-MailboxFolderPermission -Identity "ConfRoom@contoso.com:\calendar"
```

#### Add Permissions on Mailbox Calendar
```powershell
Add-MailBoxFolderPermission -Identity "ConfRoom@contoso.com:\calendar" -User "jsmith@contoso.com" -AccessRights Owner 
```

#### View Users Who Have Auto Accept BookInPolicy
```powershell
Get-CalendarProcessing -Identity "ConfRoom@contoso.com" | Select-Object -ExpandProperty BookInPolicy 
```

#### Add User to Auto Accept Book In Policy
```powershell
Set-CalendarProcessing -Identity "ConfRoom@contoso.com" -BookInPolicy ((Get-CalendarProcessing -Identity "ConfRoom@contoso.com").BookInPolicy += "jsmith@contoso.com")
```

### Links
[Exchange: Configuring the Resource Booking Attendant with PowerShell](https://itpro.outsidesys.com/2017/11/06/exchange-configuring-the-resource-booking-attendant-with-powershell/) | Outside Sys

***
## Public Folders
#### Get all public folders and their permissions
```powershell
Get-PublicFolder -Recurse | Get-PublicFolderClientPermission | Select-Object -Property Identity,User,AccessRights
```