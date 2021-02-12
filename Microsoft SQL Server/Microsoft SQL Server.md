# Microsoft SQL Server

## SQL Queries

### View active connections
```sql
EXEC sp_who
```

### View the properties of all databases
```sql
USE master
GO
SELECT *
FROM    sys.databases ;
GO
```

### View last start time of the SQL Server service
```sql
SELECT sqlserver_start_time FROM sys.dm_os_sys_info
```

### View open sessions and list databases and login name
```sql
SELECT db_name(dbid) AS DatabaseName, count(dbid) AS NoOfConnections,
loginame as LoginName
FROM sys.sysprocesses
GROUP BY dbid, loginame
```