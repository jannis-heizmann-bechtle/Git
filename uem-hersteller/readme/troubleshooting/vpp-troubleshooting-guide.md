# VPP Troubleshooting Guide

Wenn VPP nicht richtig synchronisiert oder einen Timeout bringt lässt sich hiermit Prüfen ob der VPP Server erreichbar ist und ob der VPP Account /sToken gültig ist und die Lizenzen vorhanden sind.

Zum Testen wir ein Rest Client benötigt, der Test sollte am besten auf der Airwatch Console durchgeführt werden

* Download [Advanced REST API client](https://chrome.google.com/webstore/detail/advanced-rest-client/hgmloofddffdnphfgcellkdfbfbjeloo?hl=en-US\&utm\_source=chrome-ntp-launcher) to Google Chrome

Folgenden Parameter müssen für den REST Reqeust verwendet werden:

HTTP-Method: POST

URL: [https://vpp.itunes.apple.com/WebObjects/MZFinance.woa/wa/getVPPLicensesSrv](https://vpp.itunes.apple.com/WebObjects/MZFinance.woa/wa/getVPPLicensesSrv)&#x20;

Header: Content-Type : application/json

Body: {„sToken“: „Inhalt aus der VPP Tokendatei“, „adminID“ : „ID einer über VPP gekauften App“}

Die Admin ID einer App lässt sich über den iTunes Store Link herrausfinden https://itunes.apple.com/ca/app/onedrive-cloud-storage-for/id**477537958**?mt=8

<figure><img src="../../../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>

[Apple Doku](https://developer.apple.com/library/archive/documentation/Miscellaneous/Reference/MobileDeviceManagementProtocolRef/5-Web\_Service\_Protocol\_VPP/webservice.html)
