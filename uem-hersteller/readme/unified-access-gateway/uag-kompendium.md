# UAG – Kompendium

(R)- Relay\
(E)- Endpoint

### Features und Versionen

* 3.3.1
  * Admin Password Reset on Command Line
* &#x20;3.4
  * Support HA Cluster
* 3.5
  * Deployment on Amazon AWS EC2 / Microsoft Azure
  * SSH Configuration in OVF Template
* 3.6
  * Secure Email Gateway
* FIPS
  * Content Gateway not supported (3.5)

### Network

```
Source		Dest		Protocol  Port	 Description
------------------------------------------------------------------
Extern		UAG(R)		HTTPS	  443	 Content
Extern		UAG(R)		HTTPS	  2020	 Tunnel Proxy
Extern		UAG(R)		TCP	  8443	 Per-AppPN
Intern		UAG(R)(E)	HTTPS	  9443	 Admin UI
UAG(R)(E)	AWCM		HTTPS	  2001	 AirWatch Service
UAG(R)		UAG(E)		HTTPS	  2010,8443
UAG(R)(E)	AW API		HTTPS	  443	 AirWatch Service
AWConsole 	UAG(R)(E)	HTTPS	  2020
```

### Install

```
IPMODE		STATICV4
Forward Rules	-
NIC1 IP		Server IP
Routes		-
IPV6		-
DNS		space separate
IPV4 NM		NM
IPv6 NM		-
IPv4 GW		GW
IPv6 GW		-
Name		FQDN
CEIP		NO
PWD		kein symbol am Anfang
```

### Content Gateway

API User: System Administrator, API reicht nicht

Konfiguration: /opt/airwatch/content-gateway/\
– conf/\
– smb-connector/

**Problem mit NetApp FileShares**

(z.B. Ordner / Dateien sind sichtbar, Dateien lassen sich aber nicht öffnen)

UAG Endpoint: /opt/airwatch/content-gateway/conf/config.yml\
Modify the flag value for parameter `aw.fileshare.jcifs.active` to `true`. The default value is `false`.\
Restart content-gateway service

### Secure Email Gateway

Service:\
docker

Konfiguration:\
/opt/vmware/docker/seg/container/config/

Logs:\
/var/log/vmware/docker/seg/

### Network Test

```
netstat -tlpn | grep [Port]
curl -Ivv https://<AWCM URL>:<port>/awcm/status | HTTP 200 OK
curl -Ivv https://<API URL>/api/mdm/ping | HTTP 401–unauthorized
wget https://<AWCM URL>:<port>/awcm/status | HTTP 200
```

### Network Repair Script

```
/opt/vmware/share/vami/vami_config_net
```

### Configure Services Script

```
/opt/airwatch/appliance-agent/appliance-config.sh
```

### Services

```
service <x> start | stop | restart | status

proxy
vpnd
content-gateway
docker
```

### Network Configuration Script

Es gibt ab UAG 3.5 ein Tool um Netzwerk- und Host-Konfiguration auf der Appliance.

* Default Gateway
* Hostname
* DNS / Domain / search
* Proxy Konfiguration

```
cd /opt/vmware/share/vami
./vami_config_net
```

### JAVA Cert Store

```
AirWatch UAG CA Importieren

wget https://<device_service>/[crt]
cd /tmp
cd /usr/java/jre-vmware/lib/security
mv /tmp/[crt] .
keytool -import -keystore cacerts -trustcacerts - alias <Name> -file [crt] -storepass changeit -noprompt
```

### ContentLocker UAG ohne Domäne

Möglich wenn nur eine Domäne vorhanden

```
/opt/airwatch/content-gateway/smb-connector/smb.conf
workgroup = <domäne>

```

### Root User Password reset

Die Anleitung zum Zurücksetzen des Passwortes beim Root UAG User von VMware ist leider ein einigen stellen Fehlerhaft bzw. nicht vollständig. Mit folgenden Befehlen kann man das Root PW Änderung und den User unlocken.

1. Restart the appliance from vCenter and immediately connect to the console.
2. As soon as the Photon OS splash screen appears, press e to enter GNU GRUB edit menu
3. In the GNU GRUB edit menu, go to the end of the line that starts with `linux`, add a space and type `/$photon_linux root=$rootpartition rw init=/bin/bash`. After adding these values, GNU GRUB edit menu should look exactly like this:
4. Press the F10 key and at the bash command prompt
5. Enter `passwd` to change the root users password
6. Enter `pam_tally2 -u root` to see the root users status
7. Enter `pam_tally2 -u root -r` to unlock the root user
8. Enter `umount /` to unmount / directory
9. Enter `reboot -f` to force reboot

**umount / und reboot -f sind zwingend notwendig um eine Passwortänderung zu persistieren**

&#x20;

### .local Domains resolved auslösbar machen

&#x20;

Ab UAG 3.6 wird nicht mehr resolv sondern resolved als DNS Service auf dem UAG verwendet. resolved kann nicht mit .local Domains umgehen daher führen nslookup Abfragen gegen \*.\*.local führen zu einem „not found“. Als einfacher Workaround kann in er etc/resolv der DNS Server den Kunden anstatt 127.0.0.53 eingetragen werden. Diese Einstellung ist nicht persistent und wird nach einem Neustart überschrieben.

Persistent kann der DNS Server an folgender Stelle eingetragen werden.

Unter **/etc/systemd/network/** befindet sich ein File mit dem Namen des jeweiligen Networkinterfaces (meistens eth0).

In dieser Datei können die DNS Server leerzeichengetrennt eingetragen werden

**DNS=192.168.178.1 192.168.178.2**

Ab UAG 3.10 muss zusätzlich noch der Parameter **useDNS=False .**in der gleichen Datei auskommentiert oder auf True geändert werden.

Abschließend muss der DNS und der Networking Server neugestartet werden.

**systemctl restart systemd-resolved.service**

**systemctl restart systemd-network.service**
