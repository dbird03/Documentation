# PowerShell - Visual Studio Code

##  Configuring Visual Studio Code to replace PowerShell ISE

#### Install Microsoft's PowerShell Extension
1. Go to *View > Extensions* ( <kbd>Ctrl</kbd>+<kbd>Shift</kbd>+<kbd>X</kbd> )
2. Search for "PowerShell" in the Extensions Marketplace
3. Click the green *Install* button


#### Set PowerShell as the default language
##### Using the GUI
1. Go to *File > Preferences > Settings* ( <kbd>Ctrl</kbd>+<kbd>,</kbd> )
2. Under *Text Editor > Files*, specify "powershell" as the Default Language (without quotes, all lowercase)

##### Using Settings.json
```
"files.defaultLanguage": "powershell"
```

#### Enable tab completion
##### Using the GUI
1. Go to *File > Preferences > Settings* ( <kbd>Ctrl</kbd>+<kbd>,</kbd> )
2. Under *Text Editor > Editor*, set *Editor: Tab Completion* to on

##### Using Settings.json
```
"editor.tabCompletion": "on"
```

### Additional Recommendations
#### Install a File Icon Theme
1. Go to *File > Preferences > File Icon Theme* which will open the Command Palette
2. Select *Install Additional File Icon Themes* which will open the Extensions Marketplace and list extensions tagged as icon-theme.
3. Install an icon theme you like (I recommended Material Icon Theme)

## Visual Studio Code Tips
#### Open the output of a command in a new editor file
Pipe the command to New-EditorFile
```powershell
Get-Process | New-EditorFile
```

## Keyboard Shortcuts
|Visual Studio Code Action|Keyboard Shortcut|PowerShell ISE Action|Keyboard Shortcut
|---|---|---|---|
|Open Command Palette|<kbd>F1</kbd> or <kbd>Ctrl</kbd>+<kbd>Shift</kbd>+<kbd>P</kbd>|
|Trigger suggestion|<kbd>Ctrl</kbd>+<kbd>Space</kbd>|Start Snippet|<kbd>Ctrl</kbd>+<kbd>J</kbd>
|Open Extension Snippets|<kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>J</kbd>  |
|Run Selected Text|<kbd>F8</kbd>|Run Selection|<kbd>F8</kbd>
|Start Debugging|<kbd>F5</kbd><br>I recommend to set as <kbd>Ctrl</kbd>+<kbd>F5</kbd> to <br>replace the Start Without Debugging shortcut.|Run Script|<kbd>F5</kbd>
|Start Without Debugging|By default, no shortcut is set.<br>I recommend to set as <kbd>F5</kbd> to <br>replace the Start Debugging shortcut.||




## References
[Make Visual Studio Code look and behave like PowerShell ISE](https://4sysops.com/archives/make-visual-studio-code-look-and-behave-like-powershell-ise/) | 4sysops

[Setup VS Code for PowerShell](https://www.learnpwsh.com/setup-vs-code-for-powershell/) | Learn PowerShell

[How to replicate the ISE experience in Visual Studio Code](https://docs.microsoft.com/en-us/powershell/scripting/components/vscode/how-to-replicate-the-ise-experience-in-vscode?view=powershell-6) | Microsoft Docs 

Video: [How to install Visual Studio Code and configure it as a replacement for the PowerShell ISE](https://www.youtube.com/watch?v=1sD-RktG_dM) | Mike F Robbins

Video: [Using Visual Studio Code as Your Default PowerShell Editor by Tyler Leonhardt ](https://www.youtube.com/watch?v=bGn45vIeAMM) | PowerShell.org on YouTube