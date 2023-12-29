# iOS - Enrollment: Redirect für iPadOS

Bei Workspace ONE OnPrem werden Androids und iOS Geräte beim Aufruf der externen Device Server Adresse für gewöhnlich auf die Enrollment Seite verwiesen, Windows Clients und Macs auf den Console Server.

Seit iOS 13 mit iPadOS werden iPads nun auch auf die Console weitergeleitet, die Benutzer müssten \<FQDN>/enroll für die Registrierungsseite angeben.

Ein Workaround ist die Anpassung der default.aspx\
Device Server: z.B. C:\AirWatch\Default Website\default.aspx

hier müssen 2 Zeilen hinzugefügt werden.

Unter die Konstanten (const string xxx = „xxx“;):

```
const string airwatch = "AIRWATCH";
...
const string awbrowser = "AWBROWSER";
const string ipadnew = "MACINTOSH";
string redirect_iphone = "https://mdm.asklepios.com/DeviceManagement/Enrollment/";

```

Bei den Redirects

```
Response.Headers.Remove("Server");
Response.Redirect(
agent.Contains(airwatch) ? redirect_iphone :
...
agent.Contains(awbrowser) ? redirect_awbrowser :
agent.Contains(ipadnew) ? redirect_iphone :
redirect_console
);
```

HINWEISE:

* Vor der Änderung eine Kopie der alten Datei machen.
* Ebenfalls eine Kopie von der neuen Version anlegen, da diese bei Updates überschrieben wird
* Nach der Änderung im IIS Manager ein restart ausführen\
  (cmd iisreset ist nicht erfolderlich)
