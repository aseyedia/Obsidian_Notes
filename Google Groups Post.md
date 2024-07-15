---
aliases: []
title: Google Groups Post
date created: Wednesday, May 15th 2024, 10:16:50 am
date modified: Monday, May 20th 2024, 2:02:25 pm
tags:
  - PCORnet_Projects_DSpace_Logs
  - PCORnet_Projects_DSpace
status: 
project: 
tech: 
---

[Link to post.](https://groups.google.com/g/dspace-tech/c/Kfkk0Zwyb9w)

## Reply

Forgive me, that was a typo. My UI is pointing towards my `localhost`. Not sure why it says otherwise in the text I copied and pasted.

```yaml
ui:
  ssl: false
  host: localhost
  port: 443
  nameSpace: /
  basePath: /
  rateLimiter:
    windowMs: 60000
    max: 500
  useProxies: true

rest:
  ssl: true
  host: pedsdspace01.research.chop.edu
  port: 443
  nameSpace: /server
```

Note that I get the same result whether I use `config.prod.yml` or `config.dev.yml`, and that I have been testing my setup with `yarn start:dev`.

I suspect that it might have something to do with using RHEL; some of the instructions around installing Apache HTTPD are not fully compatible with RHEL so I had to install it with `sudo yum install httpd` - I have also confirmed that the appropriate modules are installed and are being loaded by default.

```sh
sudo yum install httpd
sudo systemctl enable httpd
sudo systemctl start httpd
```

Others have suggested that I edit `angular.json`. I tried inserting this snippet:

```json
"architect": {
  "serve": {
    "options": {
      "allowedHosts": ["pedsdspace01.research.chop.edu"]
    }
  }
}
```

And was given the "cookies consent" form and nothing else. I rolled back this change because I felt as though there was something else I was doing wrong, and that I shouldn't have to edit `angular.json` to get it to work.

Someone else has responded to me separately and suggested the following change within `angular.json` as well:

> Search the section with `"serve": { … "options": {  `
> and add after

```
"port": 4000,
"disableHostCheck": true
```

Though I have yet to experiment with it.

My colleague is also working on this issue and reports that he's now getting the frontend to appear but is experiencing some CORS-related issue that is preventing the backend from working. I know there's information about CORS-related issues in the troubleshooting guide but I haven't had time to get back to this problem since making this post.

Anyway, thank you for your response and recommendation. I will continue tinkering with this problem and see where we end up. Worst-case-scenario, we will just try to use a Docker container for the frontend and see where that leads us.

All the best,
Arta

## DSpace 7.6.1 Configuration Issues with Apache HTTPD and SSL

Hello everyone,

I'm currently setting up a DSpace 7.6.1 instance for an internally-hosted and accessed metadata database and have encountered several issues that I'm struggling to resolve.

Below, I have my relevant config files listed out. But first, I will address the issue I'm encountering.

The results of `yarn test:rest` are exactly what you would expect from a working setup:

```sh
[dspace@pedsdspace01 dspace-angular-dspace-7.6.1]$ yarn test:rest
yarn run v1.22.22
$ ts-node --project ./tsconfig.ts-node.json scripts/test-rest.ts
Building production app config
Overriding app config with /home/dspace/dspace-angular-dspace-7.6.1/config/config.yml
Overriding app config with /home/dspace/dspace-angular-dspace-7.6.1/config/config.prod.yml
...Testing connection to REST API at https://pedsdspace01.research.chop.edu/server/api...

(node:2078877) Warning: Setting the NODE_TLS_REJECT_UNAUTHORIZED environment variable to '0' makes TLS connections and HTTPS requests insecure by disabling certificate verification.
(Use `node --trace-warnings ...` to show where the warning was created)
RESPONSE: 200 200 

Checking JSON returned for validity...
	"dspaceVersion" = DSpace 7.6.1
	"dspaceUI" = https://pedsdspace01.research.chop.edu
	"dspaceServer" = https://pedsdspace01.research.chop.edu/server
	"dspaceServer" property matches UI's "rest" config? true
	Does "/api" endpoint have HAL links ("_links" section)? true
Done in 2.11s.
```

You might have noticed a warning message about `NODE_TLS_REJECT_UNAUTHORIZED`. That's because I set `NODE_TLS_REJECT_UNAUTHORIZED` to `0` in my `~/.bashrc`. I was still encountering problems when I would just set `NODE_EXTRA_CA_CERTS`. These are the environmental variables I have set w/r/t Node in my `~/.bashrc`:

```sh
export NODE_EXTRA_CA_CERTS="/etc/pki/tls/certs/apache-selfsigned.pem"
export NODE_TLS_REJECT_UNAUTHORIZED=0
export NODE_OPTIONS="--max-old-space-size=4096"
```

I am trying to test my setup through `yarn start:dev`. My config details are below, but for now, it should be useful to know that `ui.ssl: false` and `rest.ssl: true`.

When I forward port 4000 to my machine and go to <http://localhost:4000>, I get DSpace's 500 page, which, believe it or not, is a huge achievement:

![[./docs/assets/img/Pasted image 20240515102904.png|Pasted image 20240515102904.png]]

Visiting the URL (which can only be accessed internally) via its URL <https://pedsdspace01.research.chop.edu/> does not produce the same thing:

![[./docs/assets/img/Pasted image 20240515171834.png|Pasted image 20240515171834.png]]

I am told "Invalid Host header." Nothing really illuminating in the DevTools.

The backend works fine. I am totally able to access <https://pedsdspace01.research.chop.edu/server/#/server/api> and see The HAL Browser:

![[./docs/assets/img/Pasted image 20240515113231.png|Pasted image 20240515113231.png]]

So the issue seems to be with connecting the frontend to the backend.

I have valid certifications issued by my IT department:

```
/etc/pki/tls/certs/pedsdspace01.research.chop.edu.crt
/etc/pki/tls/private/pedsdspace01.research.chop.edu.pem
```

### Environment Setup

- **Backend**: DSpace REST API running on Tomcat with HTTP on port 8080 and AJP on port 8009.
- **Frontend**: DSpace Angular UI running on Node.js with HTTP on port 4000.
- **Proxy**: Apache HTTPD acting as a reverse proxy, handling SSL termination and forwarding requests to Tomcat and the Angular UI.

### Configuration Files

**1. `config.dev.yml`**

```yaml
ui:
  ssl: false
  host: localhost
  port: 4000
  nameSpace: /
  rateLimiter:
    windowMs: 60000 
    max: 500 
  useProxies: true

rest:
  ssl: true
  host: pedsdspace01.research.chop.edu
  port: 443
  nameSpace: /server
```

**2. `config.prod.yml`**

```yaml
ui:
  ssl: false
  host: localhost
  port: 443
  nameSpace: /
  basePath: /
  rateLimiter:
    windowMs: 60000
    max: 500
  useProxies: true
  
rest:
  ssl: true
  host: pedsdspace01.research.chop.edu
  port: 443
  nameSpace: /server
```

**3. `local.cfg`**

```
dspace.ui.url = https://pedsdspace01.research.chop.edu
dspace.server.url = https://pedsdspace01.research.chop.edu/server

solr.server = http://localhost:8983/solr

db.url = jdbc:postgresql://localhost:5432/dspace
db.driver = org.postgresql.Driver
db.dialect = org.hibernate.dialect.PostgreSQL94Dialect
db.username = dspace
db.password = dspace
db.schema = public
```

**4. `server.xml`**

```xml
<Connector port="8080"  
                minSpareThreads="25"
                enableLookups="false"
                redirectPort="8443"
                connectionTimeout="20000"
                disableUploadTimeout="true"
                URIEncoding="UTF-8"/>

<Connector 
		   protocol="AJP/1.3" 
		   port="8009" 
		   redirectPort="8443" 
		   URIEncoding="UTF-8" 
		   secretRequired="false" />
```

Here, I inserted `secretRequired` because I was noticed the same type of error in my `catalina.err` file as in [this StackOverflow post](https://stackoverflow.com/questions/60501470/the-ajp-connector-is-configured-with-secretrequired-true-but-the-secret-attrib).

**6. `ssl.conf`**

```apache
Listen 443 https

<VirtualHost *:443>
    ServerName pedsdspace01.research.chop.edu

    # Add your desired log settings
    LogLevel trace6
    ErrorLog /var/log/httpd/pedsdspace01.research.chop.edu.error.log
    CustomLog /var/log/httpd/pedsdspace01.research.chop.edu.access.log combined
    # SSL logging for requests
    CustomLog logs/ssl_request_log "%t %h %{SSL_PROTOCOL}x %{SSL_CIPHER}x \"%r\" %b"

    # Ensure the correct headers and host settings
    ProxyPreserveHost On
    RequestHeader set X-Forwarded-Proto https

    # SSL Configuration
    SSLEngine on
    SSLCertificateFile /etc/pki/tls/certs/pedsdspace01.research.chop.edu.crt
    SSLCertificateKeyFile /etc/pki/tls/private/pedsdspace01.research.chop.edu.pem

    # Proxy requests to the Tomcat server (backend)
    ProxyPass /server ajp://localhost:8009/server
    ProxyPassReverse /server ajp://localhost:8009/server

    # Proxy requests to the Angular UI server (frontend)
    ProxyPass / http://localhost:4000/
    ProxyPassReverse / http://localhost:4000/

</VirtualHost>
```