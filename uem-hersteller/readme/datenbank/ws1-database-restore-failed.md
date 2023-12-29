# WS1 – Database Restore failed

Beim Zurückspielen des Datenbank-Backups kann es passieren, dass die Datenbank „noch verwendet wird“ obwohl der Installer bereits geschlossen und der Prozess dahinter bereits beendet ist.

Alle Verbindungen zur Datenbank schließen löst das Problem:

Datenbank -> Rechtsklick – Offline nehmen

Sollte das ebenfalls nicht tun (Timeout -> Fehler) kann die Datenbank per SQL-Statement manuell Offline genommen werden:

Für SQL Server 2012 und aufwärts:

```sql
USE [master];

DECLARE @kill varchar(8000) = '';  
SELECT @kill = @kill + 'kill ' + CONVERT(varchar(5), session_id) + ';'  
FROM sys.dm_exec_sessions
WHERE database_id  = db_id('MyDB')

EXEC(@kill);
```

Für SQL Server 2008 und früher:

```sql
USE master;

DECLARE @kill varchar(8000); SET @kill = '';  
SELECT @kill = @kill + 'kill ' + CONVERT(varchar(5), spid) + ';'  
FROM master..sysprocesses  
WHERE dbid = db_id('MyDB')

EXEC(@kill); 
```

Statt ‚MyDB‘ einfach den Namen der Datenbank eintragen bspw. „Airwatch“.

&#x20;

Befehle auch nochmal Online zu finden:

https://stackoverflow.com/questions/7197574/script-to-kill-all-connections-to-a-database-more-than-restricted-user-rollback
