# IIS Ciphers und SSL Einstellungen durchführen

Der IIS wird standardmäßig mit unsicheren SSL Einstellungen ausgeliefert. Das einfachste Tool dies zu optimieren ist IISCrypto.

Die aktuelle Version ist immer hier zu finden: [https://www.nartac.com/Products/IISCrypt](https://www.nartac.com/Products/IISCrypto)

<figure><img src="../../../.gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>

Mit dem klick auf „Best Practise“ werden alle unsicheren Ciphers deaktiviert. Danach muss der Server noch neugestartet werden, da die Änderungen in der Registry nur beim Neustart ausgelesen werden.

Als Tool für den SSL Scan / Test kann dann folgendes Tool verwendet werden:

[https://www.ssllabs.com/ssltest/](https://www.ssllabs.com/ssltest/)
