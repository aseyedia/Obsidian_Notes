---
aliases: []
tags:
  - PCORnet_Projects_DSpace_Logs
  - PCORnet_Projects_DSpace
status: 
title: How I Overcame the SSL Issue
date created: Wednesday, May 8th 2024, 3:36:44 pm
date modified: Wednesday, May 8th 2024, 3:40:36 pm
---

- [Source](https://claude.ai/chat/ef3fa774-be1f-4f7e-9be3-c0c24ece5752)

## Summary

1. Problem:
   - You were trying to obtain an SSL certificate using Certbot with the Apache plugin (`sudo certbot --apache`).
   - The Certbot command failed because it couldn't find a virtual host listening on port 80, which is required for Let's Encrypt to validate your domain.

2. Investigation:
   - You checked the Apache configuration files and found that there was no virtual host configuration for your domain (`pedsdspace01.research.chop.edu`).
   - You also realized that your DSpace application is intended for internal use within your enterprise network and is not accessible from the internet.

3. Solution:
   - Since your application is not publicly accessible, you decided to use a self-signed SSL certificate as a temporary solution.
   - You generated a self-signed certificate using the `openssl` command:

```sh
sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/pki/tls/private/apache-selfsigned.key -out /etc/pki/tls/certs/apache-selfsigned.crt
```

   - You configured Apache to use the self-signed certificate by updating the SSL configuration file (`/etc/httpd/conf.d/ssl.conf`) with the appropriate certificate and key file paths.

1. Virtual Host Configuration:
   - You created a new virtual host configuration file (`/etc/httpd/conf.d/pedsdspace01.conf`) for your domain.
   - In this file, you added a virtual host configuration for port 80 (HTTP) to allow future SSL certificate validation and HTTP to HTTPS redirection.
   - You also updated the existing virtual host configuration for port 443 (HTTPS) in the `ssl.conf` file to proxy requests to Tomcat's AJP connector.

2. DSpace Configuration Update:
   - After configuring Apache to use HTTPS, you updated the `dspace.server.url` and `dspace.ui.url` property in the `local.cfg` file to use the HTTPS URL.
   - You restarted Tomcat to apply the updated configuration.

Current Status:
- Your DSpace application is now accessible over HTTPS using the self-signed certificate.
- Users within your enterprise network can access the application, but they will see a warning message about the self-signed certificate being untrusted.
- You plan to work with your IT department to obtain a properly signed SSL certificate from a trusted Certificate Authority in the future.

Next Steps:
- Distribute the self-signed certificate to users within your enterprise network and provide instructions on how to import and trust the certificate in their browsers.
- Test your DSpace application to ensure that the frontend and backend are communicating correctly over HTTPS.
- Collaborate with your IT department to obtain a valid SSL certificate and update your Apache configuration accordingly.

That's a high-level summary of the SSL certificate issue, the steps you took to resolve it, and the current status of your DSpace application. Let me know if you have any further questions or if there's anything else I can assist you with.

