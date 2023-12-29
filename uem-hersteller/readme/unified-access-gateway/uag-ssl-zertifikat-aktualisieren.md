# UAG - SSL Zertifikat aktualisieren

Wenn das (Wildcard) SSL Server Zertifikat für das UAG aktualisiert werden muss, dieses auf der Admin Console geändert werden und die Dienste auf dem UAG neugestartet werden.&#x20;

Das SSL-Zertifikat wird pro Service einzeln in der Console geändert. Das können sein:

* VMware Tunnel / VMware Tunnel Proxy
* VMware Content Gateway
* VMware Web-Proxy

**ACHTUNG:** Die Geräte können dann keine VPN Verbindung mehr aufbauen! Hierzu muss das VPN Profil neu an alle Geräte gepusht werden! (Add Version, neu abspeichern ohne Änderungen)

Ob das neue Zertifikat erfolgreich eingebunden wurde, kann via openssl überprüft werden.&#x20;
