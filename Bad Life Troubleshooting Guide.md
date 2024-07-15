---
aliases: []
tags:
  - PCORnet_Projects_DSpace_Logs
  - PCORnet_Projects_DSpace
status: 
title: Bad Life Troubleshooting Guide
date created: Tuesday, May 14th 2024, 7:15:20 pm
date modified: Tuesday, May 14th 2024, 7:16:40 pm
---

To troubleshoot and ensure that everything is configured correctly, let's go through a checklist of things to verify:

## 1. **Apache Configuration**

- Ensure your Apache configuration for SSL and proxying is correct. Check `/etc/httpd/conf.d/ssl.conf` or the relevant Apache configuration file:

```xml
<VirtualHost _default_:443>
    ServerName pedsdspace01.research.chop.edu

    # Add your desired log settings
    ErrorLog logs/ssl_error_log
    TransferLog logs/ssl_access_log
    LogLevel warn

    ProxyPreserveHost On
    RequestHeader set X-Forwarded-Proto https

    SSLEngine on
    SSLCertificateFile /etc/pki/tls/certs/apache-selfsigned.pem
    SSLCertificateKeyFile /etc/pki/tls/private/apache-selfsigned.pem

    # Proxy all HTTPS requests to "/server" from Apache to Tomcat via AJP connector
    ProxyPass /server ajp://localhost:8009/server
    ProxyPassReverse /server ajp://localhost:8009/server

    # SSL Protocol Adjustments
    SSLHonorCipherOrder on
    SSLCipherSuite PROFILE=SYSTEM
    SSLProxyCipherSuite PROFILE=SYSTEM

    # Logging for SSL requests
    CustomLog logs/ssl_request_log "%t %h %{SSL_PROTOCOL}x %{SSL_CIPHER}x \"%r\" %b"
</VirtualHost>
```

## 2. **Tomcat Configuration**

- Ensure Tomcat's AJP connector is enabled in `server.xml`:

```xml
<Connector protocol="AJP/1.3" 
           port="8009" 
           redirectPort="8443" 
           secretRequired="false" />
```

## 3. **Restart Services**

- Restart both Apache and Tomcat to apply the configuration changes:

```sh
sudo systemctl restart httpd
sudo systemctl restart tomcat
```

## 4. **Check Logs**

- Check Apache logs for any errors:

```sh
sudo tail -f /var/log/httpd/ssl_error_log
sudo tail -f /var/log/httpd/error_log
```

- Check Tomcat logs for any errors:

```sh
sudo tail -f /opt/apache-tomcat-9.0.88/logs/catalina.out
sudo tail -f /opt/apache-tomcat-9.0.88/logs/catalina.err
```

## 5. **Verify Port Bindings**

- Ensure the ports are correctly bound and listening:

```sh
sudo netstat -tuln | grep 8009
sudo netstat -tuln | grep 443
```

## 6. **Test Connectivity**

- Test if you can reach the Tomcat server directly:

```sh
curl -v http://localhost:8080/server/api
```

- Test the AJP connection via Apache:

```sh
curl -v -k https://localhost/server/api
```

## 7. **Verify DSpace Configuration**

- Ensure `local.cfg` and `config.prod.yml` are correctly set up:

**`local.cfg`**

```properties
dspace.server.url = https://pedsdspace01.research.chop.edu/server
```

**`config.prod.yml`**

```yaml
rest:
  ssl: true
  host: pedsdspace01.research.chop.edu
  port: 443
  nameSpace: /server
```

## 8. **Review SSL Certificate**

- Ensure your SSL certificate and key files are correctly set up and not expired:

```sh
openssl x509 -in /etc/pki/tls/certs/apache-selfsigned.pem -text -noout
```

## 9. **Check Permissions**

- Ensure the SSL certificate and key files have the correct permissions:

```sh
ls -l /etc/pki/tls/certs/apache-selfsigned.pem
ls -l /etc/pki/tls/private/apache-selfsigned.pem
```

## 10. **Run DSpace Test**

- After ensuring all the above configurations are correct and services are restarted, run the DSpace REST API test again:

```sh
yarn test:rest
```

By following these steps, you should be able to identify where the configuration might be incorrect or where the issue lies. If you encounter any specific errors in the logs, please share them, and I can help you further diagnose the problem.