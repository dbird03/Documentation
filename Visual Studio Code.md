# Visual Studio Code (VS Code)

### How to change the VS Code title bar to red
This theme customization is a helpful visual cue if you ever need to run VS Code as another user account, such as an admin account with elevated priviledges.
1. Open the `settings.json` file for the admin user
2. Add the following setting:
```json
{
    "workbench.colorCustomizations": {
        "titleBar.activeBackground": "#ad0000",
    }
}
```