# WS1 - DB Update failed

Beim Upgrade der Workspace ONE DB scheitert das Setup mit dem Fehler

\[...] "**Procedure sp\_verify\_job\_identifiers"** \[...]

Der Fehler ist seitens VMware bekannt und hat mit den Berechtigungen des verwendeten Service-Accounts zu tun:

{% embed url="https://kb.vmware.com/s/article/84038" %}

Wenn Upgrades mit dem Service-User als SysAdmin oder mit einem anderen User durchgeführt worden sind, welcher erweiterte Recht hat, scheitert das Upgrade nachdem man die Berechtigungen (Rolle "Public") nach Vorgabe von VMware anpasst und der User anschließend weniger Berechtigungen hat.



**Lösung:**

Backup der Datenbank einspielen für konsistenten Stand der DB

\-> Anschließend manuell im SQL Server Management Studio unter "SQL Server Agent" -> "Jobs" -> Alle Jobs löschen, die zur Workspace ONE DB gehören. Die Jobs werden anschließend beim Upgrade vom Installer neu angelegt.
