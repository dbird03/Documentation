# Microsoft Outlook

## Email
#### Highlight emails from external senders using Conditional Formatting
1. Select your **Inbox** folder.
2. Select the **View** tab in the ribbon, and click **View Settings**.
3. Select Conditional Formatting.
4. Click **Add** and specify a **Name** for the rule (Ex: External Senders)
5. Click **Condition**
6. In the **From** field, type **@** _(Note: This only works if your email is hosted by Exchange)_
7. Click **OK**
8. Click **Font** and specify how you want the emails to appear
    ```
    Recommended Settings

    Font: Segoe UI
    Font Style: Regular
    Size: Normal
    Effects: Underline
    Color: Auto
    Script: Western
    ```

## Inbox Rules

#### Emails sent to a single email address with varying display names
This rule is useful for creating inbox rules when an email may be sent to the same email address but contain varying different display names. For example, "All Employees \<communications@contoso.com\>" or "Engineering Employees \<communications@contoso.com\>".

1. Create a new rule
2. During **Step 1: Select condition(s)**, select **sent to people or public group**.
3. Click the **people or public group** link, type the display name in the **To** field, and click **OK**.

    > [!NOTE]
    > This should not resolve to any contact in the address book. If you receive a pop up about the name not being recognized, click **Cancel** and continue with the rule creation.
4. Click next and continue with the rule creation wizard.

    > [!NOTE]
    > You need to create a new rule for each display name, even if you take the same action on them. For example, if you want emails from "All Employees" and "Engineering Employees" to go to the same folder, you need two separate rules.





## Miscellaneous
#### Forking email threads
Forking email threads is useful for continuing an existing email thread with a reduced or alternate list of recipients. Forked emails can either be displayed in the existing conversation thread or in a new conversation thread depending on the subject line used by the sender who forks the email.

Display forked email in the existing conversation thread:
Keep the "RE:" in the subject line.
```
    Original subject line:
    RE: Project meeting notes
    
    Forked subject line:
    RE: [Engineering Only] Project meeting notes
    -OR-
    [Engineering Only] RE: Project meeting notes
    
```

Display forked email in a new conversation thread:
Delete the "RE:" from the subject line.
```
    Original subject line:
    RE: Project meeting notes
    
    Forked subject line:
    [Engineering Only] Project meeting notes
```

#### Enable Query Builder
Query Builder is a helpful feature which enables advanced searching syntax in Views, Search folders, Advanced Find, and Automatic formatting or Conditional formatting.

https://www.slipstick.com/outlook/using-query-builder/

### Search Folders

#### Helpful search folders
##### Focused Inbox
This search finder finds any messages where the To or Cc field contains your name or the name of a team distribution list you are a part of.
```
Advanced:
In Folder is (exactly) Inbox

Query Builder:
OR (ANY clause may be true)
 |- To contains John Smith
 |- Cc contains John Smith
 |- To contains Contoso-Engineering
 |- Cc contains Contoso-Engineering
```
#### Unfocused Inbox
This search folder finds any messages where the To or Cc field doesn't contain your name (John Smith) or the name of a team distribution list you are a part of (Contoso-Engineering). This is especially useful for finding emails that can be deleted.
```
Advanced:
In Folder is (exactly) Inbox

Query Builder:
AND (ALL clauses must be true)
 |- To doesn't contain John Smith
 |- Cc doesn't contain John Smith
 |- To doesn't contain Contoso-Engineering
 |- Cc doesn't contain Contoso-Engineering
```

# Troubleshooting

## Inbox rules corrupted
#### Issue
When editing inbox rules, Outlook displays prompt "The rules on this computer do not match the rules on Microsoft Exchange. Only one set of rules can be kept. You will usually want to keep the rules on the server. Which rules do you want to keep?".

#### Resolution 1: Restore inbox rules from a backup

> [!CAUTION]
> Ensure you have a recent inbox rule backup file (.rwz) before proceeding!

1. Close Outlook
2. Delete the Outlook client rules using **Start > Run > outlook.exe /cleanrules**
3. Close Outlook
4. Delete the Exchange server rules using **Start > Run > outlook.exe /cleanserverrules**
5. Close Outlook
6. Open Outlook
7. Navigate to **File > Manage Rules & Alerts > Options > Import Rules** and select your backup file (.rwz)