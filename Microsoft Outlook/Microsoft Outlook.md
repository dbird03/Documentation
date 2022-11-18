# Microsoft Outlook

### Highlight emails from external senders using Conditional Formatting
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

### Forking email threads
Forking email threads is useful for continuing an existing email thread with a reduced or alternate list of recipients. Forked emails can either be displayed in the existing conversation thread or in a new conversation thread depending on the subject line used by the sender who forks the email.

Display forked email in the existing conversation thead:
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
