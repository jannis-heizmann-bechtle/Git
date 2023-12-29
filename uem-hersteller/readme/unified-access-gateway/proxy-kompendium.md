# Proxy - Kompendium

##

A reverse proxy is a device or a proxy server placed in front of a web server. When a single or multiple web server(s) is installed with reverse-proxy functionality, it acts as a single point of access or a gateway in the server farm. It is used to take some load off web servers and to provide an additional protection layer. Incoming requests are handled by the proxy server, which retrieves information from the Web server and then forwards it to the user.

A reverse proxy server reduces the workload on the internal servers and is used for various reasons:

* To hide the existence and characteristics of the origin server(s).
* To set up application firewalls to protect against common web-based attacks.
* To offload Secure Socket Layer (SSL)  to a reverse proxy server that may be equipped with SSL acceleration hardware.
* To distribute the load from incoming requests to several servers. The reverse proxy server may have to rewrite the URL in each incoming request in order to match the relevant internal location of the requested resource.

The below image is a demonstration of how a reverse proxy server works.

![Rev\_Proxy\_server\_workflow.png](https://kb.vmware.com/servlet/rtaImage?eid=ka0f4000000OPEF\&feoid=00Nf400000Tyi5C\&refid=0EMf4000001jG2N)

A reverse proxy server takes an HTTP request from the Internet and forwards it to servers in an internal network. Those users making requests connect to the reverse proxy server and most of the time may not even be aware of the internal network.



**Device Services**

Reverse proxy servers can be used with the device services server, which in an On-Premises deployment resides on the internal network. This prevents Internet traffic from accessing the internal network and masks its existence.

* Reverse proxy servers may be used as an SSL termination point and the communication from the reverse proxy server to the actual servers can either be over plain HTTP or re-encrypted with a different certificate.
* If the reverse proxy server is being used to load balance between multiple device services servers, then it needs to be set up with persistence of 20 minutes based on the source IP or SSL session token.

An overview of network connections between a reverse proxy server and device services server is shown below.

![Configuring\_ReverseProxyServers.png](https://kb.vmware.com/servlet/rtaImage?eid=ka0f4000000OPEF\&feoid=00Nf400000Tyi5C\&refid=0EMf4000001jG2I)

**Web Console**

A reverse proxy server can be used with the web console servers, which are on the internal network and have direct access to the internal network. In this case, reverse proxy server prevents the direct connectivity path from Internet to the internal network.

* SSL offloading has an impact on the console URL set in the system settings. Under Site URLs (**Groups & Settings** > **All Settings** > **System** > **Advanced** > **Site URLs**), you define the schema (http/https) that the back-end servers accept. The console application does not respond to requests arriving with the wrong schema. So make sure you set the console URL to the right http/https schema based on whether traffic from the reverse proxy server to the actual console server is encrypted or not. In other words, if the SSL session is being terminated on the reverse proxy server and is not being re-encrypted before sending to the console, you should set the site URL schema as http, even though https is used to access the console from a browser.
* Another method to make the console accessible externally is by using SSL VPN. If SSL VPN is being used, then make sure you encapsulate the entire packet along with the complete headers while sending it over and not just doing a reverse proxy and altering the header information.

**Secure Email Gateway (SEG)**

Please refer to the following section in the VMware Workspace ONE Secure Email Gateway Guide, available via Workspace ONE Resources: Appendix: Configuring with Reverse Proxy Server.

**API**

API servers can also be used with reverse proxy servers.

* API servers can be load balanced using a reverse proxy.
* In certain cases (such as dedicated  SaaS environment), you may integrate API servers with reverse proxy servers with/without SSL (certificates).
* To enable support for SSL offloading in the API, ensure the line corresponding to the endpoint in the `<serviceHosts>;` section in the Workspace ONE Services (API) web.config has the `bindingMode` attribute in the binding configuration set as `bindingMode=SSLOffload` instead of `bindingMode=Default`. See example below.
* `<add name="EisConfigurationServiceEndpoint" contract="AW.Services.ServiceDefinition.Internal.IEisConfigurationService, AW.Services.ServiceDefinition" useUserNameAndPassword="true" serverCertificate="EB430C76D0DCD3C792548452EDC93047586A013C" bindingMode="SslOffload" messageOptimization="Mtom" payloadProfile="Large"/>`

&#x20;

**Tunnel Proxy**

The below image is a demonstration of how a reverse proxy server configuration with Tunnel Proxy works:

![saas\_integrated\_cloudproxymag.png](https://kb.vmware.com/servlet/rtaImage?eid=ka0f4000000OPEF\&feoid=00Nf400000Tyi5C\&refid=0EMf4000001jG2K)

The reverse proxy server receives the SSL certificate and then routes it to the Tunnel Proxy server through HTTP protocol. Note the following:

* If the reverse proxy server is used as a termination for the SSL Session, and if the traffic is not being re-encrypted, make sure the SSL Offloading check box is selected in the Tunnel Proxy installation.
* In addition, you need to export the Tunnel Proxy certificate you are using for authentication and upload it to your reverse proxy server. If you are using Tunnel Proxy to access internal content over HTTPS (port 443), you need to export the SSL certificate used to bind port 443 and load it to the reverse proxy server as well.
* If the Tunnel Proxy is being deployed behind a reverse proxy, then a new rule has to be set on the reverse proxy server for allowing all traffic on Tunnel Proxy ports to be passed on to the Tunnel Proxy. For a list of these ports, see the Workspace ONE Tunnel Proxy documentation, available via Workspace ONE Resources.

If configuring a reverse proxy server with Tunnel Proxy, it must support CONNECT requests natively.

**VMware AirWatch Cloud Messaging (AWCM)**

* The primary goal of AWCM is to support a large number of simultaneous connections (mostly from devices) with a low sized thread pool. If a reverse proxy is set up in front of AWCM (for load balancing), the proxy gets jammed while processing the large number of connections hitting it. Therefore, configuring AWCM with a reverse proxy is not a recommended approach.
* If SSL offloading needs to be made to a proxy, then it should ensure the asynchronous http processing capabilities (lower sized thread-pool handling high number of connections).
* If High Availability (HA) is needed, an active-passive configuration must be set up for AWCM.
* The underlying data store for AWCM supports shared storage architecture (HornetQ journals). If Disaster Recovery (DR) set up is needed, it is required to set up the storage in a fail-safe SAN environment. Both the active and passive nodes of AWCM should point to the same storage location.

