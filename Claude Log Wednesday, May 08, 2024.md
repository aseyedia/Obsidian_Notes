---
aliases: []
title: Claude Log Wednesday, May 08, 2024
date created: Wednesday, May 8th 2024, 6:28:24 pm
date modified: Monday, May 13th 2024, 9:28:27 am
tags:
  - PCORnet_Projects_DSpace_Logs
  - PCORnet_Projects_DSpace
status: 
project: 
tech: 
---

Here's an updated report that includes your attempt to use the `NODE_EXTRA_CA_CERTS` environment variable:

**Date:** [[2024-05-08-Wednesday|2024-05-08-Wednesday]]

## Objective

The main objective of today's work was to set up HTTPS support for your DSpace application and troubleshoot issues related to the connection between the frontend and backend.

## Tasks and Progress

1. **Configuring HTTPS Support:**
   - You started by configuring Apache to use HTTPS (port 443) and proxy requests to Tomcat's AJP connector.
   - You created a new virtual host configuration file (`/etc/httpd/conf.d/pedsdspace01.conf`) for your domain (`pedsdspace01.research.chop.edu`).
   - In the virtual host configuration, you added the necessary directives for HTTPS support, including `SSLEngine on`, `SSLCertificateFile`, and `SSLCertificateKeyFile`.
   - You used a self-signed SSL certificate (`apache-selfsigned.crt` and `apache-selfsigned.key`) for testing purposes.

2. **Updating DSpace Configuration:**
   - After configuring HTTPS in Apache, you updated the `dspace.server.url` property in the DSpace `local.cfg` file to use the HTTPS URL.
   - You restarted Tomcat to apply the updated configuration.

3. **Testing REST API Connection:**
   - To test the connection between the frontend and backend, you used the `yarn test:rest` command.
   - However, the command failed with an SSL/TLS-related error, indicating that the frontend was unable to establish a secure connection to the backend API.

4. **Troubleshooting SSL/TLS Issues:**
   - You investigated the SSL/TLS issue by checking the DSpace backend's accessibility through port forwarding (`http://localhost:8080/server/api`).
   ~~- The backend API was accessible through the port-forwarded URL, and the response body contained the correct HTTPS URLs for various DSpace endpoints.~~
   - You verified that the frontend configuration (`config.prod.yml`) had the correct `rest` section settings, including `ssl: true`, `host`, `port`, and `nameSpace`.

5. **Attempting Solutions:**
   - You tried various approaches to resolve the SSL/TLS issue, including:
     - Modifying the `test-rest.ts` script to include the port number in the `restUrl` variable.
     - Setting the `NODE_TLS_REJECT_UNAUTHORIZED` environment variable to `0` to disable SSL verification.
     - Installing and using the `https-proxy-agent` package to send requests through the port-forwarded connection.
     - Modifying the `test-rest.ts` script to use the `http` protocol instead of `https`.
   - Despite these attempts, the `yarn test:rest` command continued to fail with SSL/TLS-related errors.

6. **Using `NODE_EXTRA_CA_CERTS` Environment Variable:**
   - You explored the possibility of using the `NODE_EXTRA_CA_CERTS` environment variable to make Node.js trust the self-signed SSL certificate.
   - Following the suggestion, you set the `NODE_EXTRA_CA_CERTS` environment variable to the path of your self-signed certificate file (export `NODE_EXTRA_CA_CERTS="/etc/pki/tls/apache-selfsigned.crt"`).
   - However, this approach did not resolve the issue, and the `yarn test:rest` command still failed with SSL/TLS-related errors.

7. **Next Steps:**
   - Due to the persistent issues with the SSL/TLS configuration and the inability to establish a secure connection between the frontend and backend, further investigation and troubleshooting are required.
   - Potential next steps include:
     - Reviewing the DSpace configuration files (`local.cfg` and `dspace.cfg`) to ensure proper REST API configuration.
     - Investigating the SSL/TLS configuration in the Apache or Tomcat server.
     - Testing the REST API connection manually using tools like Postman or cURL.
     - Seeking assistance from the DSpace community forums or mailing lists for further guidance and support.
     - Considering the option of obtaining a valid SSL certificate from a trusted Certificate Authority (e.g., Let's Encrypt) instead of using a self-signed certificate.
 - Since this is going to be hosted in the secure CHOP intranet, maybe I don't need an SSL certificate.

## Conclusion

Today's work focused on setting up HTTPS support for your DSpace application and troubleshooting issues related to the connection between the frontend and backend. Despite configuring HTTPS in Apache, updating the DSpace configuration, and attempting various solutions, including setting the `NODE_EXTRA_CA_CERTS` environment variable, the `yarn test:rest` command continued to fail with SSL/TLS-related errors. Further investigation and troubleshooting are necessary to resolve the issue and establish a secure connection between the frontend and backend.

Please let me know if you have any questions or if there's anything else I can assist you with.