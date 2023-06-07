# Log Analytics
## General
### Query a user's interactive and non-interactive sign-in logs.
```kql
union SigninLogs, AADNonInteractiveUserSignInLogs
| sort by TimeGenerated desc
```
Source: <https://isroot.nl/2022/03/23/parsing-interactive-and-non-interactive-sign-in-logs-with-microsoft-sentinel/>

---

## Time
Links: <https://learn.microsoft.com/en-us/azure/azure-monitor/logs/scope#time-range> 
```kql
\\ Filter where TimeGenerated between dates
| where TimeGenerated between (datetime(YYYY-MM-DD HH:MM:SS)..datetime(YYYY-MM-DD HH:MM:SS))
```