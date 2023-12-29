# SEGv2 (Windows) Timeouts / Sporadische Fehler

## Symptome

In neueren Versionen des Secure Email Gateways gibt es Probleme mit der CRL-Validierung. Beim Zugriff auf den Device Server (API-Server), versucht das SEG die CRL des öffentlichen Zertifikates zu erreichen. Ist diese nicht erreichbar, weil HTTP Port 80 ins Internet nicht geöffnet ist, treten sporadisch Fehler auf. Diese können wie folgt bemerkt werden:

* Langsame Verbindungstest (SSLLabs / Testssl)
* Sporadisch Fehler beim Versenden / Empfang von Mails am Mobile Client
* Sporadische Fehlermeldungen in Mail-Apps bzgl. der Verbindung

Die Fehler treten in der Regel nicht immer, allerdings meist bei allen Clients auf. Die Anforderung zur Erreichbarkeit der CRL ist seitens VMware nicht dokumentiert, dennoch muss die CRL scheinbar erreichbar sein.



## Ursache / Validierung

Bei betroffenen Systemen lässt sich beobachten, das ausgehende Verbindungen zu öffentlichen IPs nicht erfolgreich aufgebaut werden können. Das lässt sich wie folgt auf dem SEG prüfen:

```powershell
Get-NetTCPConnection
```

Die gezeigte Tabelle, zeigt alle ausgehenden Netzwerk-Verbindungen. Entscheidend sind Einträge mit dem Status "SYN\_SENT". Diese sind nicht erfolgreich aufgebaut. Beispiel:





## Behebung

Öffnen der Firewall via HTTP Port 80 ins Internet oder zu den definierten Zielen der jeweiligen Zertifizierungsstelle. Kann im Zertifikat geprüft werden:

![](<../../../.gitbook/assets/image (2).png>)

Die genauen IPs sind in der Regel von der jeweiligen CA online dokumentiert.
