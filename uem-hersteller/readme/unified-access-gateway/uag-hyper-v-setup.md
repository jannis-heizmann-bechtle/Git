# UAG - Hyper-V Setup

Für die Installation des UAGs auf Hyper-V sind eine Punkte besonders wichtig.

Die Installation muss über ein Powershell Script auf dem Hyper-V Server druchgeführt werden. Wichtig hierbei ist das der Server auf Englisch installiert sein muss da ansonsten das Script fehlschlägt. Falls der Server auf Deutsch installiert ist empfiehlt es sich temporär einen neuen Hyper-V Server als virtuelle Maschine auf Englisch zu installieren und hier die UAG Installation durchzuführen. Nach Erfolgreicher installation muss die Maschine einmalig gestartet werden, danach kann die vhdx auf den Produktiven Hyper-V verschoben werden.

Die relevanten Dateien hierbei sind uag9-awhv.ini und uagdeployhv.ps1

In der Ini Datei muss der Server konfiguriert werden, zwingend sind hier nur Hostname und Netzwerk Konfiguration, die Passwörter für Root und Admin fragt das Powershellscript während der Ausführung ab.

Folgende Einstellungen können in der Ini Konfiguriert werden.

https://communities.vmware.com/docs/DOC-30835
