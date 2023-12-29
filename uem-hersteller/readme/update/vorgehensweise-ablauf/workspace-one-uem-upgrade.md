# Workspace ONE UEM Upgrade

Grundsätzlich gelten beim Upgrade von Workspace ONE UEM folgende Vorgehensweisen.



## **Testing (vor Upgrade):**

* Geräte-Tests durchführen
  * Geräte-Registrierung
  * Häufig verwendeten Use Cases
  *   Kritische Funktionalitäten

      * Email
        * VPN
        * Content
        * App-Deployment
        * etc.


*   Console-Tests durchführen (Healthcheck-Dokument)

    * Prüfen der Anbindung von 3rd Party Services
    * Prüfen der Site-URLs
    * Test Connections


*   Dokumentation der Test-Ergebnisse

    * Nutzung der Healthcheck-Vorlage



## Vorbereitung des Upgrades

* Prüfen der Voraussetzungen
  * Minimale SQL-Server Version
  * Windows Server Versionen von Console/ Device Server
  * Downtime kommuniziert?
    * User informiert?
    * DB-Berechtigungen / Service-Account
* Backup
  * [Herunterfahren aller Dienste](../../console-device-server/ws1-start-stop-services.md) (AirWatch\*) auf Console & Device Server
  * Backup der SQL-Datenbank via SQL Server Management Studio
  * Erstellen von Snapshots für Console & Device Server



## **Durchführung des Upgrades:**

Generell gilt beim Upgrade immer zu beachten, dass immer ein Full-Installer und anschließend der jeweils aktuellste Patch ausgeführt werden sollte. Für Full-Installer und Patch gilt unabhängig von einander immer, dass **zuerst die Datenbank aktualisiert** werden muss. Anschließend die Application Server in Form von Console und Device Server.

Die Updates werden also in folgender Reihenfolge installiert:

1. Full-Installer (Datenbank)
2. Full-Installer (Application Server)
3. Patch-Skript (Datenbank)
4. Patch-Installer (Application Server)

Bei der Reihenfolge spielt es keine Rolle ob zuerst Console oder zuerst Device Server aktualisiert werden. Der Upgrade-Prozess im Detail:



* Prüfen der Vorab-Tests



* Upgrade von Datenbank, Console Server, Device Server
  * Datenbank
    * Ausführen des Full-Installer für Datenbank auf **Console Server nicht auf SQL-Server**
    * Prüfen der [Datenbank-Version](../../datenbank/ws1-check-db-version.md) bei erfolgreichem Upgrade
  * Console Server
    * Ausführen des Full-Installer für Application Server auf Console Server
    * Prüfen der WS1-Version bei erfolgreichem Upgrade (Programm-Liste bspw.)
  *   Device Server

      * Ausführen des Full-Installer für Application Server auf Device Server
      * Prüfen der WS1-Version bei erfolgreichem Upgrade (Programm-Liste bspw.)


* Patch von Datenbank, Console Server und Device Server
  * Datenbank
    * Ausführen des Patch-Skripts gegen die Workspace ONE Datenbank
    * Prüfen der [Datenbank-Version](../../datenbank/ws1-check-db-version.md) bei erfolgreichem Upgrade
  * Console Server
    * Ausführen des Patch-Installer für Application Server auf Console Server
    * Prüfen der WS1-Version bei erfolgreichem Upgrade (Programm-Liste bspw.)
    * Neustart des Console-Server durchführen
  * Device Server
    * Ausführen des Patch-Installer für Application Server auf Device Server
    * Prüfen der WS1-Version bei erfolgreichem Upgrade (Programm-Liste bspw.)
    * Neustart des Device Servers durchführen



## Testing (nach Upgrade)

* Geräte-Tests durchführen
  * Geräte-Registrierung
  * Häufig verwendeten Use Cases
  *   Kritische Funktionalitäten

      * Email
      * VPN
      * Content
      * App-Deployment
      * etc.


* SSL-Scans über alle extern erreichbare Server laufen lassen
* Dokumentation der Test-Ergebnisse
  * Nutzung der Healthcheck-Vorlage
