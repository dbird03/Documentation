# GitHub

### Create and configure a new GitHub repo with a GitHub private email address
1. Create a new repo on GitHub.com
2. Copy the URL of the repo (click the green Code button)
3. In VS Code, open the Command Palette (<kbd>Ctrl</kbd> + <kbd>Shift</kbd> + <kbd>P</kbd>), search for the **Git: Clone** command, and hit <kbd>Enter</kbd>.
4. Paste in the URL, and hit <kbd>Enter</kbd>
5. Specify the directory where you want to clone the repo to and click Save
6. Navigate to the repo in VS Code, and open the GitLens extension
7. Expand the Contributors section, and note your private email for the GitHub repo
8. In the repo, use the git config --local to set the name and email
   ```
   git config --local user.name "John Smith"
   git config --local user.email 12345678-jsmith@users.noreply.github.com
   ```
9.  Add files to the repo, stage the changes, commit, and push repo to the GitHub