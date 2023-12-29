# WS1 - Start/Stop Services

Per PowerShell können die Workspace ONE Dienste im Bulk gestartet / gestoppt werden.

Dienste Starten:

```powershell
Start-Service -DisplayName AirWatch*
```

Dienste Stoppen:

```powershell
Stop-Service -DisplayName AirWatch*
```
