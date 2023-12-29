# WS1 - AWCM: SSL Zertifikat tauschen

#### How to replace the SSL certificate on an AWCM server

* Log into the AWCM server in question.
* Open a command prompt, and navigate to the AWCM config directory (C:\AirWatch\AirWatch\<version>\AWCM\config by default) and run the following:

```
keytool -list -v -keystore awcm.keystore
```

* Type in the password when prompted (make a note of the password, the new awcm.keystore file needs to use the same password).
* Das Kennwort ist das des ursprünglich eingespielten Zertifikats
* Export the new SSL certificate from the appropriate 3rd party CA (or local CA for certain on-premise deployments), and make sure the full signing chain is exported.  Additionally, make sure that the password used to export is the same as the one used for the current awcm.keystore, otherwise the import will succeed but when AWCM starts an error message below will appear and the status page will refuse to load (as the pre-configured password will be incorrect and the AWCM app will not be able to open the keystore).
* Copy the certificate into the AWCM config directory (C:\AirWatch\AirWatch\<version>\AWCM\config by default).
* Run the following command to replace SSL cert on AWCM servers:

```
keytool -importkeystore -srckeystore <new-pfx-cert-name>.pfx -srcstoretype pkcs12 -destkeystore awcm.keystore.new -deststoretype JKS
```

* Once this has completed successfully, you will now see a new file named **awcm.keystore.new** in the config directory.
* Stop the AWCM service.
* Rename the **awcm.keystore** to **awcm.keystore.old**.
* Rename the **awcm.keystore.new** to **awcm.keystore**.
* Start the AWCM service.
* Using a valid AWCM url, try to hit the page (https://\<External AWCM URL>/awcm/statistics) and if the status page loads then check the certificate details. It should now display the values for the newly uploaded cert.
* If the status page fails to load, check the log files.
* If rollback is required, rename the **awcm.keystore** to **awcm.keystore.new** Then, rename **awcm.keystore.old** to **awcm.keystore** and restart AWCM. This will restore the previous settings.
