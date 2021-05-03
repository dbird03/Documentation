# GitHub

### Create and configure a new GitHub repository with a GitHub private email address
1. Create a new repository on GitHub.com.
1. Copy the URL of the repo (click the green **Code** button).
1. In VS Code, open the Command Palette (<kbd>Ctrl</kbd> + <kbd>Shift</kbd> + <kbd>P</kbd>), search for the **Git: Clone** command, and hit <kbd>Enter</kbd>.
1. Paste in the URL of the repo, and hit <kbd>Enter</kbd>
1. Specify the local directory where you want to clone the repo to, and click **Save**.
1. In GitHub in your web browser, go to your **profile > Settings > Emails**.
1. In the **Primary email address** section, note down your unique GitHub email address (ex: _12345678+username@users.noreply.github.com_).
1. In VS Code, navigate to the repo in the terminal.
1. Use the `git config` command to set your name and email:
   ```
   git config --local user.name "John Smith"
   git config --local user.email "12345678-jsmith@users.noreply.github.com"
   ```
1. Confirm your git config by using the `git config --list` command. Hit <kbd>Enter</kbd> to scroll through the lines of the config and type `:wq` to quit when finished.