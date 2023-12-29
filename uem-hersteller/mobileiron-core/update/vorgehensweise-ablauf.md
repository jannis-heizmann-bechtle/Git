# Vorgehensweise / Ablauf

Generell gilt folgende Vorgehensweise beim Upgrade. Dokumentiert werden die Updates über die Healthcheck Dokumente (Excel-Tabelle) im OneDrive.



## Vor dem Update

* Prüfung der Funktionen in der Admin-Oberfläche
* Prüfen der Geräte-Funktionen die kritisch sind (Mail, Docs@Work, Web@Work, VPN etc.)

### Snapshots erstellen

* Core herunterfahren -> Dann Snapshot via vSphere / Hyper-V
* Sentry kann im laufenden Betrieb gesnapshottet werden
* Core wieder hochfahren um mit dem Update zu beginnen



## Update durchführen

System Manager der Core -> Maintenance -> Update -> Download

Nach erfolgreichem Update -> "Validate" prüft den aktuellen Status

Dann "Stage for install" -> Wenn Status auf "Reboot to install" wechselt -> Core neustarten (rechts im Menü den Punkt "Reboot" nutzen)

Sentry kann parallel aktualisiert werden, wenn zeitlich notwendig, ansonsten parallel updaten.



## Nach dem Update

* Prüfung der Funktionen in der Admin-Oberfläche
* Prüfen der Geräte-Funktionen die kritisch sind (Mail, Docs@Work, Web@Work, VPN etc.)
* Ggfls. weitere Anpassungen wie:
  * Zertifikats-Tausch
  * Erneuerung Apple-Token
  * Anpassungen unabhängig vom Update selbst

