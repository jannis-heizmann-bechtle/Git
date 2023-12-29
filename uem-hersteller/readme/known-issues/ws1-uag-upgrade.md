# WS1 - UAG Upgrade

## Settings Import Fehler nach Upgrade auf UAG 2303

Nach einem Upgrade auf UAG 2303 tritt der Fehler auf, dass beim Import der Konfiguration vom vorherigen UAG eine Fehlermeldung erscheint.

"JSON mapping exception while processing request"

Auf dem UAG wird im Log /opt/vmware/gateway/admin.log folgender Fehler gezeigt:

<figure><img src="../../../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

Der Fehler resultiert daraus, das VMware die Bezeichnung für den Tunnel-Dienst geändert hat.

Aus "TUNNEL\_GATEWAY\_AND\_TUNNEL\_PROXY" wurde "TUNNEL\_GATEWAY". Die Bezeichnung muss daher manuell auch im JSON angepasst werden.

Anschließend kann das JSON hochgeladen werden.
