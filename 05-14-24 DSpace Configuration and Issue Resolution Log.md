---
aliases: []
title: 05-14-24 DSpace Configuration and Issue Resolution Log
date created: Tuesday, May 14th 2024, 11:04:38 am
date modified: Tuesday, May 14th 2024, 11:14:22 am
tags:
  - PCORnet_Projects_DSpace_Logs
  - PCORnet_Projects_DSpace
status: 
project: 
tech: 
exampleProperty: 68
---

## Checkpoint Log for May 14, 2024

### Issue Resolution Steps

1. **Identified Missing AJP Secret:**
   - Encountered an error indicating that the AJP Connector required a secret but none was provided.
   - Log:

```
 SEVERE [main] org.apache.catalina.util.LifecycleBase.handleSubClassException Failed to start component [Connector["ajp-nio-127.0.0.1-8009"]]
 Caused by: java.lang.IllegalArgumentException: The AJP Connector is configured with secretRequired="true" but the secret attribute is either null or "". This combination is not valid.
```

1. **Updated Tomcat Configuration:**
   - Modified the AJP Connector configuration in Tomcat's `server.xml` to disable the secret requirement.
   - Added the following line to `server.xml`:

```xml
<Connector protocol="AJP/1.3" port="8009" redirectPort="8443" URIEncoding="UTF-8" secretRequired="false" />
```

1. **Restarted Tomcat and Apache:**
   - [[Internal DSpace Instance Setup Log#How to Start/Stop Apache Tomcat|Restarted both Tomcat]] and Apache services to apply the changes.
   - Commands:

```sh
sudo systemctl restart httpd
```

1. **Tested REST API Connection:**
   - Ran `yarn test:rest` to verify the REST API connection.
   - Output indicated successful connection and resolution of previous issues.

2. **Verified AJP Connector Functionality:**
   - Confirmed that the AJP connector was properly configured and working.
   - Checked logs to ensure there were no errors related to the AJP connector.
   - Relevant log snippets:

```
14-May-2024 10:47:20.054 INFO [main] org.apache.catalina.startup.Catalina.start Server startup in [22539] milliseconds
```

1. **Checked SSL and HTTP Requests:**
   - Verified SSL configuration by checking the response from the REST API over HTTPS.
   - Ensured the frontend can communicate with the backend using the configured ports.

## Summary of Changes and Configurations

1. **Tomcat Configuration (`server.xml`):**

```xml
<Connector port="8080"
	minSpareThreads="25"
	enableLookups="false"
	redirectPort="8443"
	connectionTimeout="20000"
	disableUploadTimeout="true"
	URIEncoding="UTF-8"/>
...
	<Connector protocol="AJP/1.3" port="8009" redirectPort="8443" URIEncoding="UTF-8" secretRequired="false" />

```

1. **Apache Configuration (`ssl.conf`):**

   ```apache
   <VirtualHost _default_:443>
       ServerName pedsdspace01.research.chop.edu

       ErrorLog logs/ssl_error_log
       TransferLog logs/ssl_access_log
       LogLevel warn

       ProxyPreserveHost On
       RequestHeader set X-Forwarded-Proto https

       SSLEngine on
       SSLCertificateFile /etc/pki/tls/certs/apache-selfsigned.pem
       SSLCertificateKeyFile /etc/pki/tls/private/apache-selfsigned.pem

       ProxyPass /server ajp://localhost:8009/server
       ProxyPassReverse /server ajp://localhost:8009/server

       SSLHonorCipherOrder on
       SSLCipherSuite PROFILE=SYSTEM
       SSLProxyCipherSuite PROFILE=SYSTEM

       CustomLog logs/ssl_request_log "%t %h %{SSL_PROTOCOL}x %{SSL_CIPHER}x \"%r\" %b"
   </VirtualHost>
   ```

2. **Environment Configuration:**
   - Set environment variable to bypass SSL certificate verification for testing:

```sh
export NODE_TLS_REJECT_UNAUTHORIZED=0
```