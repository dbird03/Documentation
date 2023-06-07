# Visual Studio Code (VS Code)
## General
### Open GitHub repository in Visual Studio Code within a web browser
1. Sign in to GitHub.
2. Go to a repository's main page (github.com/user/repository).
3. Hit the Period key (<kbd>.</kbd>) on your keyboard.
4. The repository should open within Visual Studio Code.
---
## Themes
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
---
## Snippets

### Examples of escapating special characters in the .json snippets file for PowerShell snippets
#### Example 1:
Notice how dollar signs ($) must be escaped with another dollar sign ($) and how the double-quote (") character must be escaped with the forward slash (/) character.

JSON Snippet
```json
"Parameter":{
	"prefix": "Parameter",
	"body": [
		"[Parameter(Mandatory=$$true)]"
		"[ValidateSet(\"Value1\",\"Value2\")]"
		"[ValidateNotNullOrEmpty()]"
		"[Alias(\"Alias1\",\"Alias2\")]"
		"[type]"
		"$$ParameterName"
	],
	"description": "Inserts a new PowerShell parameter"

} // End Parameter
```
PowerShell Result
```powershell
[Parameter(Mandatory=$true)]
[ValidateSet("Value1","Value2")]
[ValidateNotNullOrEmpty()]
[Alias("Alias1","Alias2")]
[type]
$ParameterName
```

#### Example 2
Notice how the tabs are kept in the result.

JSON Snippet:
```json
"Splat Get-GPResultantSetOfPolicyReport": {
	"prefix": "Splat Get-GPResultantSetOfPolicyReport",
	"body": [
		"$$Params = @{"
		"	User		= ''"
		"	Computer	= ''"
		"	Path		= $$ExportPath"
		"	Comment	    = ''"
		"}"
		"Get-GPResultantSetOfPolicyReport @Params"
	],
	"description": "Splat Get-GPResultantSetOfPolicyReport"
} // End ExportPath
```
PowerShell Result:
 
 (Note: The tab spacing in the PowerShell Result does not display correctly on GitHub.com)
```powershell
$Params = @{
    User		= ''
    Computer	= ''
    Path		= $ExportPath
    Comment	    = ''
}
Get-GPResultantSetOfPolicyReport @Params
```