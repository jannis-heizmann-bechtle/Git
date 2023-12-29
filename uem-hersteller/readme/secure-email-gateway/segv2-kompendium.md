# SEGv2 Kompendium

Kerberos Constrained Delegation funktioniert ab SEG 2.10 relativ staibl, SEG 2.10.1 sollte nicht verwendet werden ab besten empfiehlt sich Stand heute(23.05.19) SEG v2.11 Für KCD.

Das Root/Intermediate Zertifikat der Windows PKI welches für CBA genutzt wird kann über folgende Schritte auf dem PKI Server exportiert werden

1\. Requesting the Root Certification Authority Certificate by using command line:

a. Log into the Root Certification Authority server with Administrator Account.

b. Go to „**Start**“ -> „**Run**“ -> and write „**Cmd**“ and press on „**Enter**“ button.

c. To export the  Root Certification Authority server to a new file name „**ca\_name.cer**“

write:

„**certutil  -ca.cert ca\_name.cer**„.

Falls es zu problemen bei der CBA kommt sollte „Require Client Certificate“ in der Advanced SEG Config deaktiviert werden

## Crossrealm / Alternative Suffix Configuration

Wenn in der Kundenumgebung mehrere UPN Suffixe genutzt werden, wie Beispielseweise bei uns in der Demoumgebung (befrdemo.net , befrdemo.de) muss dies in der Konfiguration beachtet werden.

Im Domain – Domain Conroller Mapping in der SEG Config sollte nur die Domäne, nicht die alternativen UPN Suffixe aufgeführt werden.

Wichtig in diesem Fall ist das im Clientcertificate das User Principal Name SAN Feld nicht mit dem User Principal Name sondern mit SamAccountName@Realm befüllt wird.

Im Fall von Befrdemo.net/befrdemo.de sieht das folgendermaßen aus.

User Principal Name = {EnrollmentUser}@BEFRDEMO.NET

Wobei {EnrollmentUser} der SamAccountName ist und BEFRDEMO.NET die Domäne.

## Configuration for Multiple Certificates Trust

Although SEG v2 is capable of supporting multiple certificates to trust, the UEM Console only allows for a single certificate to be uploaded. If additional certificates are required, you must add them manually to the SEG configuration.

1.  Export the full chain of certificates for the required CAs.

    Note:

    Ensure that this full chain contains both the root and intermediate certificates.
2. Move the certificates to the /config/ssl-certs path within the install directory of the SEG.
3. Navigate to the config.json file within the config folder of the SEG directory.
4.  Modify the clientCertTrustStorePath file to include the certificate’s absolute paths as comma-separated values within quotes and save the file. For example:

    `"C:/SecureEmailGateway/config/ssl-certs/Example1.cer,C:/SecureEmailGateway/config/ssl-certs/Example2.cer"`
5. Restart the SEG service.

## **Troubelshooting SEGv2**

**Logging**

„C:\AirWatch\SecureEmailGateway-2.11\config\kerberos\kcd-client.conf“

In der kcd-client.conf gibt es 3 Stellen um das Logging zu erhöhen

\## 4 – Debug

log\_level 4

\## Flag to enable MIT kerberos DLL logging, when enabled MIT logs will be routed to pipe logs\
enable\_krb5\_logging 1

\## Enable time profiling for library calls (Enable to monitor performance)\
enable\_profiling 1

In der

„C:\AirWatch\SecureEmailGateway-2.11\config\logback.xml“

Datei lässt sich ebenfalls der Loglevel der einzelnen Komponenten erhöhen.

\<logger name=„com.airwatch.seg.handler.CertificateAuthHandler“ level=„DEBUG“ additivity=„false“ groupKey=„cert.auth.logger“ hideFromApi=„false“>

&#x20;

**Update der Config**

„C:\AirWatch\SecureEmailGateway-2.11\config\application.properties“

In Dieser Datei kann unter

vac.seg.config.settings.refresh.interval.in.minutes=30

Bestimmt den Takt in welchem der SEG bei der API nach einer neuen Konfiguration schaut

**Exchange 2010 / TSL 1.0**

Bei Java 1.8 ist standardmäßig TLSv1 deaktiviert da aber auf vielen Exchange 2010 Servern noch TLSv1 aktiviert ist kann es hier zu Problem kommen.

„X:\AirWatch\SecureEmailGateway-2.10\service\conf\segServiceWrapper.conf“

In dieser Zeile:

wrapper.java.additional.9=-Djdk.tls.disabledAlgorithms=MD5\\, RC4\\, TLSv1\\, SSLv2Hello\\, SSLv3\\, DSA\\, DESede\\, DES\\, 3DES\\, DES40\_CBC\\, RC4\_40\\, MD5withRSA\\, DH\\, 3DES\_EDE\_CBC\\, DHE\\, DH keySize < 1024\\, EC keySize < 224

muss TLSv1\\, entfernt werden

Danach muss der SEG Server neu gestartet werden.

**Kerberos / CBA**

„C:\AirWatch\SecureEmailGateway-2.11\config\application.properties“

\# This flag is used to enable or disable UPN (used for kerberos authentication) lookup from Subject Common Name when UPN is not present in SAN type extension.\
\# By default it is disabled since we do not want to fall back to CN value for every customer.\
enable.upn.lookup.from.subject.cn=false

Die Prüfung der CRL kann ebenfalls in der Config deaktiviert oder geändert werden

\# A switch to enable/disable Certificate revocation check using CRL. This flag will be used only when CBA is enabled.\
enable.cert.revocation.validation=false

\# Where CRL files will be stored in SEG. It will be relative path from SEG installation directory.\
cert.based.auth.crl.files.parent.dir.path=/config/crls

\# If SEG failed to download latest CRL from CA during server startup, shall we fail hard or just log the error.\
fail.hard.on.crl.download.failure.during.server.startup=true

\# CRL host separated by comma. It can be left empty if KCD is off or CRL is disabled.\
remote.crl.distribution.http.uris=

Um Debuggen von Kerberos kann auch die krb5.init Datei genutzt werden, hier können Realms / Domänen für das Erstellen der Tokens angepasst werden

„C:\AirWatch\SecureEmailGateway-2.11\config\kerberos\krb5.ini“
