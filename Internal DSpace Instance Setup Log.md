---
editor-width: 
status: 
aliases: []
tags:
  - "#PCORnet_Projects_DSpace"
  - PCORnet_Projects_DSpace
priority: ✅ - Complete
title: Internal DSpace Instance Setup Log
date created: Monday, April 29th 2024, 4:38:25 pm
date modified: Thursday, June 13th 2024, 1:52:13 pm
---

> [!todo] Incomplete Tasks
>
> ```tasks
> # this query is inside of a callout. Learn more about callouts [here.](https://help.obsidian.md/Editing+and+formatting/Callouts)
> path includes {{query.file.path}}
> hide backlink
> not done
> ```

## Resources

_(Link any notes that may be relevant to the task at hand.)_

- [[2024-04-23 SSQ and DSpace|2024-04-23 SSQ and DSpace]]

## Task Details

This assignment focuses on the setup and implementation of [DSpace](https://dspace.lyrasis.org/), a comprehensive library management system designed to manage and contain metadata effectively. The project aims to establish a DSpace instance specifically for PEDSnet internally, transitioning from the current use of a GitHub io page for variable dictionary documentation to a more centralized and robust system. ==The initiative is driven by the need to consolidate a vast array of information, studies, and other data into a singular, accessible platform.== DSpace, known for its prebuilt metadata library capabilities, utilizes technologies such as Apache Maven, Ant, Solr, and Tomcat to facilitate this process.

Key objectives include understanding the application's layout and functionality in depth, addressing the limitations imposed by predefined datasets and metadata fields, and exploring the potential for adding new fields to enhance metadata accessibility. A significant part of the project involves setting up a local VM to serve as an internal development and testing environment, laying the groundwork for future front-end improvements by a contractor.

This assignment is not only a technical endeavor but also an opportunity to deepen knowledge about DSpace and its application within the context of PEDSnet and PCORnet. It represents a critical step towards streamlining the accessibility and interface of metadata, serving dual purposes as both a deliverable for PCORnet and an internal library for PEDSnet's variable dictionary, centered around OMOP standards.

## Additional Notes

### OS Info

```sh
[seyediana1@pedsdspace01 ~]$ cat /etc/os-release 
NAME="Red Hat Enterprise Linux"
VERSION="9.3 (Plow)"
ID="rhel"
ID_LIKE="fedora"
VERSION_ID="9.3"
PLATFORM_ID="platform:el9"
PRETTY_NAME="Red Hat Enterprise Linux 9.3 (Plow)"
ANSI_COLOR="0;31"
LOGO="fedora-logo-icon"
CPE_NAME="cpe:/o:redhat:enterprise_linux:9::baseos"
HOME_URL="https://www.redhat.com/"
DOCUMENTATION_URL="https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/9"
BUG_REPORT_URL="https://bugzilla.redhat.com/"

REDHAT_BUGZILLA_PRODUCT="Red Hat Enterprise Linux 9"
REDHAT_BUGZILLA_PRODUCT_VERSION=9.3
REDHAT_SUPPORT_PRODUCT="Red Hat Enterprise Linux"
REDHAT_SUPPORT_PRODUCT_VERSION="9.3"
```

### Useful Commands

#### How to Start/Stop Apache Tomcat

Logged in as `dspace`, call the following:

```sh
tomcatRestart
```

#### Misc

```sh
# Tomcat Logs
sudo find /opt/apache-tomcat-9.0.88/logs/* -type f -name "*log" -newermt "$(date +'%Y-%m-%d')" | xargs sudo tail -f

# SSL Logs
sudo find /var/log/httpd/ -type f -name "*log" -newermt "$(date +'%Y-%m-%d')" | xargs sudo tail -f

# DSpace Logs
sudo tail -f /opt/dspace/log/dspace.log
```

### 2024-05-06

- [Installation documentation.](https://wiki.lyrasis.org/display/DSDOC7x/Installing+DSpace)
- [ChatGPT Thread 1](https://chatgpt.com/c/c4e2224d-80f8-40ed-8324-5cdc0aafb2cb)
- [ChatGPT Thread 2](https://chatgpt.com/c/229e3094-9909-46ad-9c3c-d049941ddc6c)
- [ChatGPT Thread 3](https://chatgpt.com/c/92cc79cf-682b-447a-b073-8e1ee9658672)
- [Claude Thread](https://claude.ai/chat/acd60cf8-254d-41ec-803a-baa1cf9161db)
- [Claude Thread 2](https://claude.ai/chat/ef3fa774-be1f-4f7e-9be3-c0c24ece5752)

### 2024-04-29

From Alex Sharrock

> Hey Arta, I got the vm [pedsdspace01.research.chop.edu](http://pedsdspace01.research.chop.edu/) up and running and you have admin access on there. So you should be good to go to get started on the dspace install. I added a 500gb drive mounted ot /data so go ahead and install things there. Please let me know if you have any questions or want to meet to go over things or anything like that.

- [x] Launch an instance of DSpace ➕ 2024-04-29 ⏳ 2025-04-24 📅 2024-05-22 ✅ 2024-05-21

[DSpace demo can be found here.](https://172.22.32.9/home)

- To log in:

```sh
ssh pedsdspace01.research.chop.edu
```

- [[DSpace Installation Conclusion|DSpace Installation Conclusion]]

## Log

### 2024-06-07 10:53:23am

`dspace` disappeared from `access-cron.conf` so I added it back in and I also gave `dspace` permissions over its own `crontab` file.

```sh
sudo chmod 644 /var/spool/cron/dspace
sudo chown dspace:dspace /var/spool/cron/dspace
```

### 2024-06-07 10:07:02am

Still getting the same issue.

```
--------------------- Cron Begin ------------------------

**Unmatched Entries**
PAM ERROR (Permission denied)
FAILED to authorize user with PAM (Permission denied)
PAM ERROR (Permission denied)
FAILED to authorize user with PAM (Permission denied)
PAM ERROR (Permission denied)
FAILED to authorize user with PAM (Permission denied)
PAM ERROR (Permission denied)
FAILED to authorize user with PAM (Permission denied)
PAM ERROR (Permission denied)
PAM ERROR (Permission denied)
FAILED to authorize user with PAM (Permission denied)
FAILED to authorize user with PAM (Permission denied)

---------------------- Cron End -------------------------


--------------------- pam_unix Begin ------------------------

crond:
    Unknown Entries:
       Access denied for user dspace: 10 (User not known to the underlying authentication module): 6 Time(s)
...
```

Let's check it out.

### 2024-06-05 9:59:29am

Not really worth mentioning but I had I changed the format with which I added `dspace` to `access-cron.conf`.

### 2024-06-05 9:55:40am

The `cron` issue persists:

```sh
crond:
    Unknown Entries:
       Access denied for user dspace: 10 (User not known to the underlying authentication module): 2 Time(s)
```

### 2024-06-04 12:30:33pm

IS responded (faster than I thought they would):

> In order for the `dspace` user to use cron, it has either be in one of two AD groups or in the `cron` security file.
>
> As it is a local group, anyone with sudoer rights on the system can add the user to the file `/etc/security/access-cron.conf` and it will be able to use `cron`.

Done.

### 2024-06-04 10:04:41am

I just messaged IS about the `cron` PAM configuration issue for `dspace`:

> Forgive me if this is not the proper avenue to ask for this, but I don't know where else to go.
>
> On this VM server, there is a service account called `dspace`, named after the DSpace data catalog framework (<https://wiki.lyrasis.org/display/DSDOC7x/Introduction>) which we are implementing. As part of the regular maintenance of this site, there are a series of regular `cron` commands that need to run in order for the site to function. However, `dspace` is unable to do any of these `cron` commands because of the "PAM" configuration, which I've been told can't be directly modified and is managed by Azure AD, and ultimately, what this service account lacks is an Azure AD identity. These `cron` commands need to be executed by the `dspace` service account, because if they are executed by another user, it completely disrupts the fragile file permissions system of the `dspace` backend application, which causes the site to crash.
>
> This site is intended to be used as an internal resource by researchers, meaning that it would never face the external internet.
>
> How should I approach this issue? The simplest, most straightforward ask would be for a `dspace` user account to match the one already on this server. However, in the case that that's not an option, is there a way to override the "PAM" configuration requirement without causing any problems with the apparently externally-managed user configurations on the server? Thank you very much. Arta.

Let's see what they say.

### 2024-06-03 12:39:18pm

Okay. We got it back, up and running. Alex said all he did was re-copy the `solr` cores into Tomcat or whatever it is and restart `solr`. Cool. I was able to get things going under `dspace`. Cool. So now it works again.

I also cleared some of the data in `/home` so its a lot emptier and ready for business.

I'm just gonna wait for the next batch of Logwatch issues to make sure sure the problems are relevant.

- [x] Make sure Logwatch issues are all resolved. ➕ 2024-06-03 ✅ 2024-06-03

[We also *do* eventually need to resolve the `cron` problem](https://wiki.lyrasis.org/display/DSDOC7x/Installing+DSpace#InstallingDSpace-PM2(oranotherProcessManagerforNode.jsapps)(optional,butrecommendedforProduction):~:text=Setup%20scheduled%20tasks,for%20more%20details.), which, unfortunately, would involve asking IT/IS to create a `dspace` user with full credentials:

- [x] [Fix the `cron` issue](https://wiki.lyrasis.org/display/DSDOC7x/Installing+DSpace#InstallingDSpace-PM2(oranotherProcessManagerforNode.jsapps)(optional,butrecommendedforProduction):~:text=Setup%20scheduled%20tasks,for%20more%20details.): Get IT to add `dspace` as a user. ➕ 2024-06-03 ✅ 2024-06-04

### 2024-06-03 11:44:24am

I just gave up and told Alex it would probably be way easier to go from Snapshot to working instance than try to fix this at this point. He said he would take a look at it and then restore the snapshot if it wasn't working.

The thing is, we took the snapshot at the point of the working `dev` instance, so there are still a few steps needed from there to get it to work. But they were so much more minor than what we're dealing with now, which is totally inscrutable.

If he restores the snapshot, here are the things I need to do, in order of importance:

- [x] Move the DSpace frontend directory into `/data` ➕ 2024-06-03 ✅ 2024-06-03
- [x] Install the `pm2` module that deals with the [log files](https://github.com/keymetrics/pm2-logrotate) ➕ 2024-06-03 ✅ 2024-06-03
- [x] Delete all of the `cron` commands under `dspace` ➕ 2024-06-03 ✅ 2024-06-03
- [x] `git pull` the configuration file repo, make sure it's in `/opt`, and make sure the config files in the system are the same as the ones in the repo. ➕ 2024-06-03 ✅ 2024-06-03
- [x] Address the remaining errors in Logwatch ➕ 2024-06-03 ✅ 2024-06-04

### 2024-06-03 10:35:33am

I just sent Alex the following message:

> I strongly suspect, though I'm not positive, that maybe part of what broke DSpace was setting the cron chores under a different user than dspace . The reason I think that is because when I call the DSpace binary just to see what the commands are, it vomits a wall of Java errors related to permissions:

```
> [dspace@pedsdspace01 dspace]$ ./bin/dspace
> ERROR StatusConsoleListener RollingFileManager (/opt/dspace/config/../log/dspace.log) java.io.FileNotFoundException: /opt/dspace/config/../log/dspace.log (Permission denied)
>  java.io.FileNotFoundException: /opt/dspace/config/../log/dspace.log (Permission denied)
> …
> (etc)
```

> The reason I even noticed this was because I got a ton of Logwatch alerts over the weekend, some which had to do with disk space. Apparently /home/ had ballooned to 100% usage, with `/home/dspace/.pm2/logs` approaching 6 GB. I emptied that folder but I realized I had skipped the "optional" step of installing a `pm2` log-folder-clearing module, which I have now done. I don't know how much the second issue has to do with the problem.
>
> But anyway when I forward the Tomcat port, I can reach the Tomcat splash page, but when I append /server to the end of it, I get a 404 error. So I think the DSpace backend might have broken because the DSpace binary executed commands through `cron` by a user with a different set of permissions than the dspace user.
>
> I talked to Carrie last week (at Jon's suggestion) about cron stuff because she is an expert and she said that normally, in these situations, she sets the cron commands under a different user than the service user, because of the Azure AD-related stuff.

Here's what he said:

> Yeah a few things, the first is that the server has a `/data` drive that has a couple hundred gig on it, so I would recommend moving the install there, that way space is not going to be come an issue, you should be able to just move the frontend directory and it will server the same from a different location.
>
> With Cron, what you can do is after running the jobs add a permissions step to be sure they work or for now simply stop them, as we don't fully need them yet and can work thru getting them running when needed.
>
> But just changing ownership now should also fix the issue that is currently stopping it from running

### 2024-06-03 10:19:00am

I think I figured out the problem. DSpace hated that I set the `cron` jobs under my super user account, `seyediana1`, because it interfered with its ability to log.

```
[dspace@pedsdspace01 dspace]$ ./bin/dspace
ERROR StatusConsoleListener RollingFileManager (/opt/dspace/config/../log/dspace.log) java.io.FileNotFoundException: /opt/dspace/config/../log/dspace.log (Permission denied)
 java.io.FileNotFoundException: /opt/dspace/config/../log/dspace.log (Permission denied)
 ...
(etc)
```

### 2024-06-03 9:57:32am

Disk usage in `/home`:

```sh
[seyediana1@pedsdspace01 dspace_configs_repo]$ sudo du -ah /home | sort -rh | head -n 20
16G	/home/dspace
16G	/home
5.9G	/home/dspace/dspace-angular-dspace-7.6.1
5.7G	/home/dspace/.pm2/logs
5.7G	/home/dspace/.pm2
4.8G	/home/dspace/dspace-angular-dspace-7.6.1/.angular/cache/15.2.11
4.8G	/home/dspace/dspace-angular-dspace-7.6.1/.angular/cache
4.8G	/home/dspace/dspace-angular-dspace-7.6.1/.angular
4.7G	/home/dspace/dspace-angular-dspace-7.6.1/.angular/cache/15.2.11/angular-webpack
2.8G	/home/dspace/.cache
2.2G	/home/dspace/.cache/yarn/v6
2.2G	/home/dspace/.cache/yarn
1.3G	/home/dspace/.pm2/logs/dspace-ui-out-3.log
1.2G	/home/dspace/.pm2/logs/dspace-ui-out-0.log
1.1G	/home/dspace/.pm2/logs/dspace-ui-out-4.log
1.1G	/home/dspace/.pm2/logs/dspace-ui-out-2.log
1.1G	/home/dspace/dspace-angular-dspace-7.6.1/node_modules
796M	/home/dspace/.pm2/logs/dspace-ui-out-1.log
782M	/home/dspace/dspace-angular-dspace-7.6.1/.angular/cache/15.2.11/angular-webpack/313e65b3fef75ba9431a9e0b6a1eeedd8e4e22d1
757M	/home/dspace/dspace-angular-dspace-7.6.1/.angular/cache/15.2.11/angular-webpack/77657638bacb3172d722bb71e1c66ec446a867a7
```

The `pm2` logs folder is almost 6GB lol.

### 2024-06-03 9:49:23am

Okay. Tomcat is running, but when I go to <http://localhost:8080/server,> I get a 404 error. That is insightful. Let's see if I can fix the issue.

### 2024-06-03 9:07:04am

Okay, I'm still having issues with this DSpace implementation from Logwatch.

Okay. Well. I overconfidently went on an `sudo rm -rf` spree and now the website doesn't work. At all. Not even the backend.

### 2024-05-30 10:44:27am

Never mind, I'm going to let my user account, `seyediana1`, handle all `cron` jobs.

### 2024-05-30 9:13:49am

I'm just going to let `root` handle the `cron` jobs.

### 2024-05-30 9:07:20am

Oh my God. I'm still getting that stupid "logwatch" error with the unauthorized PAM stuff.

### 2024-05-29 3:08:57pm

I'm just going to do what I did last time for now because I don't think altering `/etc/security/access.conf` or `/etc/pam.d/cron` was the cause of the `ssh` issues.

### 2024-05-29 2:01:50pm

Okay Alex said I can just add the `cron` instructions under `root`. But the instructions say to add `cron` instructions to `dspace`. So I'm just gonna give `dspace` root permissions.

Oops I guess doing that won't do anything?

I made [this post.](https://groups.google.com/g/dspace-tech/c/-2pFitLLBeE/m/pGhoj4SiAgAJ)

### 2024-05-29 1:08:54pm

Okay. So yesterday, pretty late, Alex got the thingy to start working again.

So I can get into DSpace again.

### 2024-05-28 11:17:34am

Alex responded:

> Hey Arta, I also cannot get in, the issue is that PAM is connected to the Azure AD and that the DSpace user is not in AD so I guess that is why the cron tab isn't working. Hopefully we can just restart the SSH daemon on the server. A lot of the system configs are set by puppet which is a remote management service so be careful editing those as clearly it can cause some unintended consequences

I am a bit shook. Not sure what I did here.

### 2024-05-28 9:56:10am

I need to reopen this project because I'm getting issues regarding `cron`.

Pretty much everyday, I get an email from the server. Within these emails, I get the following error messages:

```
**Unmatched Entries**
PAM ERROR (Permission denied)
FAILED to authorize user with PAM (Permission denied)
PAM ERROR (Permission denied)
FAILED to authorize user with PAM (Permission denied)
PAM ERROR (Permission denied)
PAM ERROR (Permission denied)
FAILED to authorize user with PAM (Permission denied)
FAILED to authorize user with PAM (Permission denied)
PAM ERROR (Permission denied)
FAILED to authorize user with PAM (Permission denied)
PAM ERROR (Permission denied)
PAM ERROR (Permission denied)
FAILED to authorize user with PAM (Permission denied)
FAILED to authorize user with PAM (Permission denied)
```

But now I can't even `ssh dspace` into the server.

```
(base) seyediana1@WJ7T2V3KGX ~ % ssh dspace ============================================================================= This computer system is the property of The Children's Hospital of Philadelphia and may only be accessed by authorized users. Unauthorized use of this system is strictly prohibited, and may subject violators to criminal prosecution or other penalties. Users of this system are subject to all policies and procedures set forth by The Children's Hospital of Philadelphia. The Children's Hospital of Philadelphia reserves the right to monitor, record, audit, copy or otherwise retain evidence of all activities on this system. Use of this system by any person indicates the user's consent to all of the foregoing terms and conditions. ============================================================================= Connection closed by 10.30.11.25 port 22 (base) seyediana1@WJ7T2V3KGX ~ % cat ~/.ssh/config Host dspace HostName pedsdspace01.research.chop.edu User seyediana1
```

I asked Alex about this. Maybe it has something to do with the fact that I'm on the VPN network off-site?

### 2024-05-20 4:57:13pm

Production build works.

### 2024-05-20 1:38:47pm

I got it to work. I feel incredible. The sheer sense of accomplishment as I got the `dev` frontend to work.

![[./docs/assets/img/Pasted image 20240520133924.png|Pasted image 20240520133924.png]]

A caveat is the fact that I have overridden a lot of the authentication checks. So it is working despite the fact that Apache doesn't seem to like the SSL cert/key:

```
[Mon May 20 13:37:17.864496 2024] [ssl:info] [pid 3509986:tid 3510173] (70014)End of file found: [client 10.14.54.192:54627] AH01992: SSL library error 1 reading data [Mon May 20 13:37:17.864532 2024] [ssl:info] [pid 3509986:tid 3510173] SSL Library Error: error:0A000126:SSL routines::unexpected eof while reading [Mon May 20 13:37:17.864591 2024] [ssl:debug] [pid 3509986:tid 3510173] ssl_engine_io.c(1477): (70014)End of file found: [client 10.14.54.192:54627] AH02007: SSL handshake interrupted by system [Hint: Stop button pressed in browser?!]
```

Nonetheless, this is a step in the right direction.

[Here is the Git commit allowed it to work.](https://github.com/aseyedia/dspace_backup_configs/tree/27733c246041b9619b64e6bdc4ee3c6ac0ec221a)

The changes I had made that tipped it into the right direction for `yarn start:dev` are:

- `SSLCertificateFile` was originally ` pedsdspace01.research.chop.edu.crt`, but I had switched it to the cert key `pedsdspace01.research.chop.edu.pem` because I was getting that EOF error above and I had noticed that it contained both the certificate and the private key. I changed it back.

```
SSLCertificateFile /etc/pki/tls/certs/pedsdspace01.research.chop.edu.crt
```

- Fixed the typo here, set it to SSL:

```
dspace.ui.url = https://pedsdspace01.research.chop.edu
```

and ***removing*** the following item from `server.xml` (not sure why I thought it was a good idea to put it in there in the first place):

```xml
# Redirect HTTP to HTTPS
<VirtualHost *:80>
    ServerName pedsdspace01.research.chop.edu
    RewriteEngine On
	RewriteCond %{HTTPS} off
	RewriteRule ^(.*)$ https://%{HTTP_HOST}%{REQUEST_URI} [L,R=301]
</VirtualHost>
```

but more broadly:

- `angular.json` having its authentication options disabled, specifically `allowedHosts` and `disableHostCheck`:

```json
...
"serve": {
          "builder": "@angular-builders/custom-webpack:dev-server",
          "options": {
            "allowedHosts": [
              "pedsdspace01.research.chop.edu"
            ],
            "browserTarget": "dspace-angular:build",
            "port": 4000,
            "disableHostCheck": true
          },
...
```

- Standard `config.dev.yml` settings for frontend:

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

- Standard production `local.cfg`:

```yaml
dspace.server.url = https://pedsdspace01.research.chop.edu/server

# dspace.ui.url=http://10.30.11.25:4000
dspace.ui.url = https://pedsdspace01.research.chop.edu
```

- Standard `server.xml`. Not going to show it because it's the same as what you would get if you follow the directions, but has an extra `secretRequired = "false"` argument for the AJP connector.

Also not entirely sure how much [this](https://groups.google.com/g/dspace-tech/c/F6qtImSF5Yo/m/8UzFbk9_BgAJ) post had to do with it, but I called `./bin/dspace database migrate force` even though the database migration was already all successful. I arrived there based on [this thread](https://www.mail-archive.com/dspace-tech@googlegroups.com/msg12912.html) where OP had the same inscrutable Java error as I did in `dspace.log`:

```
java.lang.NullPointerException: Cannot invoke "Object.getClass()" because 
"modelObject" is null
```

This might be a totally fragile implementation but I am willing to take the W.

### 2024-05-20 1:01:52pm

Oh yeah I forgot to mention, I added the `angular.json` lines into `angular.json`.

```json
...
"serve": {
          "builder": "@angular-builders/custom-webpack:dev-server",
          "options": {
            "allowedHosts": [
              "pedsdspace01.research.chop.edu"
            ],
            "browserTarget": "dspace-angular:build",
            "port": 4000,
            "disableHostCheck": true
          },
...
```

### 2024-05-20 1:00:05pm

Okay Alex's thing didn't really work either. So I'm back to chipping away at my thing.

By the way, this command below shows what files have been modified today in all subdirs of `cwd`.

```sh
[dspace@pedsdspace01 dspace]$ find . -type f -newermt "$(date +'%Y-%m-%d')" -print
./config/local.cfg
./config/local.cfg-20240520-124335.old
./log/dspace.log-2024-05-19
./log/dspace.log
./sitemaps/sitemap_index.html
./sitemaps/sitemap_index.xml
```

### 2024-05-20 12:23:33pm

A few things:

```sh
sudo find /opt/apache-tomcat-9.0.88/logs/* -type f -name "*log" -newermt "$(date +'%Y-%m-%d')" | xargs sudo tail -f

sudo find /var/log/httpd/ -type f -name "*log" -newermt "$(date +'%Y-%m-%d')" | xargs sudo tail -f

sudo tail -f /opt/dspace/log/dspace.log
```

These are the commands I gave Alex to monitor the logs. `sudo tail -f /var/log/httpd/ssl*` doesn't work anymore. No clue why.

So now I'm calling

```sh
sudo tail -f /var/log/httpd/ssl_request_log /var/log/httpd/error_log
```

Here ChatGPT gave me a command that will only give me the `tail -f` of files last edited today:

```sh
sudo find /var/log/httpd/ -type f -name "*log" -newermt "$(date +'%Y-%m-%d')" | xargs sudo tail -f
```

### 2024-05-20 10:30:27am

Guess who's back. It's me. Let's see where we are now.

[Here is the commit. Looks like he didn't really change much.](https://github.com/aseyedia/dspace_backup_configs/commit/c7eb76159130fce8ba7736925c9f0fed8b218b05)

Although…

```
dspace.ur.url=http://10.30.11.25:4000
```

Typo. Lol. Looks like I have to rebuild DSpace.

### 2024-05-16 3:17:34pm

 I got a response on my Google Groups post:

> Hi,
>
> not sure if this helps, I had similar problems - I believe - and disabled the hostcheck in angular.json.
> Search the section with `"serve": { … "options": {`
> and add after

```
"port": 4000
a comma and
"disableHostCheck": true
```

> CU
>
> Michael

 This might be something because I also edited the angular.json file to include this line yesterday:

```json
"architect": {
  "serve": {
    "options": {
      "allowedHosts": ["pedsdspace01.research.chop.edu"]
    }
  }
}
```

And that gave me the "cookies consent" form but nothing else, and I was trying to get it to work without touching angular.json so I reversed my changes but it looks like I might have to.

### 2024-05-15 6:00:01pm

Nightmare.

### 2024-05-15 3:12:02pm

Okay. Well. The certificates at least helped me not get that error I was getting, with invalid certs.

But it still doesn't work. I can't make heads or tails of it.

I found out, though, that I can increase the verbosity of the log in `server.xml` by setting `LogLevel` to something like `trace3`, which is the highest I can do before it starts becoming illegible.

### 2024-05-15 12:36:27pm

The IT Keys work! 🥲

So that part has been resolved. Now I just need to figure out how to link Apache to the frontend. Or whatever the issue is.

### 2024-05-15 12:06:37pm

IT finally gave me the cert/key.

```
pedsdspace01.research.chop.edu.crt
pedsdspace01.research.chop.edu.pem
```

Someone messaged me on Teams and said:

```
[11:47 AM] Ferris, James
Hello
[11:47 AM] Ferris, James
TASK2480667
[11:48 AM] Ferris, James
Common Name pedsdspace01.research.chop.edu

SAN dspace.lyrasis.org
[11:48 AM] Ferris, James
You can pickup this certificate at myapps, certificate management
[11:48 AM] Ferris, James
Password is chop1234
```

Honestly not really sure what the password is for but whatever.

### 2024-05-15 11:45:26am

I'm working. [[Google Groups Post|Google Groups Post]]. [Maybe manual cert?](https://eff-certbot.readthedocs.io/en/latest/using.html#manual)

I am in hell.

### 2024-05-15 12:20:42am

I created [a GitHub for all of my configs](https://github.com/aseyedia/dspace_backup_configs). Isn't that wacky?

I can also manually create a [Let's Encrypt certificate in case it comes to it.](https://letsencrypt.org/getting-started/#:~:text=If%20your%20hosting%20provider%20doesn%E2%80%99t,not%20plan%20to%20implement%20it.)

### 2024-05-14 7:14:51pm

Okay, it's back to normal now. `yarn test:rest` works again.

I'm going to make a troubleshooting guide. I'm calling it the [[Bad Life Troubleshooting Guide|Bad Life Troubleshooting Guide]].

### 2024-05-14 6:46:01pm

I don't know if it's worth saying but I changed Tomcat back to Alex's configuration.

```sh
sudo tail -f /opt/apache-tomcat-9.0.88/logs/*
```

### 2024-05-14 5:27:24pm

This is terrible. I'm calling it a day.

Here are some log-monitoring commands. This day started out good and just downward-spiraled into despair.

```sh 
sudo tail -f  /opt/apache-tomcat-9.0.88/logs/localhost_access_log05-14.txt /opt/apache-tomcat-9.0.88/logs/catalina.err /opt/apache-tomcat-9.0.88/logs/catalina.out /opt/apache-tomcat-9.0.88/logs/localhost_access_log-05-14.txt
```

```sh
sudo tail -f /var/log/httpd/ssl_request_log
```

```sh
sudo tail -f /var/log/httpd/ssl_error_log
```

Port-forwarding:

```sh
ssh -L 4000:localhost:4000 -L 8080:localhost:8080 -L 8983:localhost:8983 seyediana1@pedsdspace01.research.chop.edu
```

### 2024-05-14 5:16:46pm

As the day goes on, I get increasingly desparate until I just stop taking as many notes.

But anyway I am going to try to revert Tomcat to the way it was before by changing `webapps/` to `alex_webapps/` and editing `/opt/apache-tomcat-9.0.88/conf/Catalina/localhost/server.xml`

I'm basically starting over [deploying the backend.](https://wiki.lyrasis.org/display/DSDOC7x/Installing+DSpace#InstallingDSpace-UsingaSelf-SignedSSLCertificatecausestheFrontendtonotbeabletoaccesstheBackend:~:text=Deploy%20Server%20web,own%20webapps%20folder.) Maybe this is a bad idea but I'm at the point where I'm strongly considering just wiping the VM and starting over. This is so terrible. I can't believe my first project on this team ended up like this.

I think there's something going on here though:

> The name of the file (not including the suffix ".xml") will be the name of the context, so for example `server.xml` defines the context at `[http://host:8080/server](http://host:8080/server)`.  To define the _root context_ (`[http://host:8080/](http://host:8080/)`), name that context's file `ROOT.xml`.   

> To define the _root context_ (`[http://host:8080/](http://host:8080/)`), name that context's directory `ROOT`.

Maybe I need to do something like that.

### 2024-05-14 5:10:41pm

I'm still going to be working on this in my 80s.



I'm going to rebuild DSpace but maybe with these configs.

### 2024-05-14 4:55:44pm

Nightmare. Nightmare. I suspect that Alex rebuilding DSpace actually caused this whole thing to backslide, because now, when I try to visit the site, I get this:

![[./docs/assets/img/Pasted image 20240514165633.png|Pasted image 20240514165633.png]]

And I have never gotten this page before. I'm going insane. This is terrible. What if I re-rebuilt it?

### 2024-05-14 2:46:56pm

Okay, Alex has also given up. I guess he fixed up some things but couldn't fix up others:

![[./docs/assets/img/Pasted image 20240514144922.png|Pasted image 20240514144922.png]]

### 2024-05-14 1:02:54pm

Well. Looks like Alex changed `local.cfg` to `https://pedsdspace01.research.chop.edu/server`. `yarn test:rest` works but not `yarn start:dev` (as in, we still can't get DSpace frontend to talk to the backend).

### 2024-05-14 11:29:10am

Okay. I celebrated too early because the yarn test was not perfect:

```
[dspace@pedsdspace01 dspace-angular-dspace-7.6.1]$ yarn test:rest
yarn run v1.22.22
$ ts-node --project ./tsconfig.ts-node.json scripts/test-rest.ts
Building production app config
Overriding app config with /home/dspace/dspace-angular-dspace-7.6.1/config/config.yml
Overriding app config with /home/dspace/dspace-angular-dspace-7.6.1/config/config.prod.yml
…Testing connection to REST API at https://pedsdspace01.research.chop.edu/server/api…

(node:1211751) Warning: Setting the NODE_TLS_REJECT_UNAUTHORIZED environment variable to '0' makes TLS connections and HTTPS requests insecure by disabling certificate verification.
(Use `node --trace-warnings …` to show where the warning was created)
RESPONSE: 200 200 

Checking JSON returned for validity…
	"dspaceVersion" = DSpace 7.6.1
	"dspaceUI" = https://pedsdspace01.research.chop.edu:4000
	"dspaceServer" = https://pedsdspace01.research.chop.edu:8080/server
	"dspaceServer" property matches UI's "rest" config? false
	Does "/api" endpoint have HAL links ("_links" section)? true
Done in 2.42s.
```

### 2024-05-14 10:59:43am

We did it. `yarn test:rest` works. All I needed was `secretRequired="false"` in the AJP connector such that

```xml
    <Connector protocol="AJP/1.3" port="8009" redirectPort="8443" URIEncoding="UTF-8" secretRequired="false" />
```

And that made things work:

```
[dspace@pedsdspace01 dspace-angular-dspace-7.6.1]$ yarn test:rest
yarn run v1.22.22
$ ts-node --project ./tsconfig.ts-node.json scripts/test-rest.ts
Building production app config
Overriding app config with /home/dspace/dspace-angular-dspace-7.6.1/config/config.yml
Overriding app config with /home/dspace/dspace-angular-dspace-7.6.1/config/config.prod.yml
...Testing connection to REST API at https://pedsdspace01.research.chop.edu/server/api...

(node:1211751) Warning: Setting the NODE_TLS_REJECT_UNAUTHORIZED environment variable to '0' makes TLS connections and HTTPS requests insecure by disabling certificate verification.
(Use `node --trace-warnings ...` to show where the warning was created)
RESPONSE: 200 200 

Checking JSON returned for validity...
	"dspaceVersion" = DSpace 7.6.1
	"dspaceUI" = https://pedsdspace01.research.chop.edu:4000
	"dspaceServer" = https://pedsdspace01.research.chop.edu:8080/server
	"dspaceServer" property matches UI's "rest" config? false
	Does "/api" endpoint have HAL links ("_links" section)? true
Done in 2.42s.
```

Something else I did, of course, was add the node-related environment variables, which allow me to use a self-signed cert, expand the allotted memory space, and make insecure connections:

```sh
export NODE_EXTRA_CA_CERTS="/etc/pki/tls/private/apache-selfsigned.pem"
export NODE_OPTIONS="--max-old-space-size=4096"
export NODE_TLS_REJECT_UNAUTHORIZED=0
```

See more details [[05-14-24 DSpace Configuration and Issue Resolution Log|here]].

### 2024-05-14 9:30:38am

Yesterday I reinstalled the dependencies It's today.

Still doesn't work. I had to downgrade a dependency:

```
yarn remove ngx-mask
yarn add ngx-mask@9.1.2
```

That seemed to work:

```
** Angular Live Development Server is listening on localhost:4000, open your browser on https://localhost:4000/ **

✔ Compiled successfully.
✔ Browser application bundle generation complete.

Initial Chunk Files | Names   | Raw Size
runtime.js          | runtime | 14.46 kB | 

164 unchanged chunks

Build at: 2024-05-14T13:32:27.810Z - Hash: 79d09ed362c69d25 - Time: 42342ms

✔ Compiled successfully.
```

![[./docs/assets/img/Pasted image 20240514093555.png|Pasted image 20240514093555.png]]

This is progress. It looks terrible, but it's progress.

### 2024-05-13 4:50:43pm

I'm going into the hole again. I'm doing a lot of things that I'm not necessarily keeping track of. It's okay. Alex agreed to help me.

I'm trying to test it now with `yarn start:dev`, because I've kind of backslid. The SSL thing was a horrible mistake. I should not have tried to use a self-signed certificate, because it's made everything harder.

Anyway, i was getting an OOM error, so I had to call the below command and it seemed to help me bypass that error.

```
export NODE_OPTIONS="--max-old-space-size=4096"
```

Okay. I've actually made some minor progress. Using `yarn start:dev`, I was able to get this:

![[./docs/assets/img/Pasted image 20240513165519.png|Pasted image 20240513165519.png]]

HUGE!

Here are the errors it's giving me:

```
./node_modules/@ng-dynamic-forms/ui-ng-bootstrap/fesm2020/ui-ng-bootstrap.mjs:880:283-299 - Error: export 'MaskDirective' (imported as 'i4') was not found in 'ngx-mask' (possible exports: INITIAL_CONFIG, NEW_CONFIG, NGX_MASK_CONFIG, NgxMaskDirective, NgxMaskModule, NgxMaskPipe, NgxMaskService, _configFactory, initialConfig, timeMasks, withoutValidation)

** Angular Live Development Server is listening on localhost:4000, open your browser on https://localhost:4000/ **

✖ Failed to compile.
✔ Browser application bundle generation complete.

Initial Chunk Files | Names   | Raw Size
runtime.js          | runtime | 14.46 kB | 

164 unchanged chunks

Build at: 2024-05-13T20:50:30.846Z - Hash: ac00020eb06254a9 - Time: 23775ms

./node_modules/@ng-dynamic-forms/ui-ng-bootstrap/fesm2020/ui-ng-bootstrap.mjs:880:283-299 - Error: export 'MaskDirective' (imported as 'i4') was not found in 'ngx-mask' (possible exports: INITIAL_CONFIG, NEW_CONFIG, NGX_MASK_CONFIG, NgxMaskDirective, NgxMaskModule, NgxMaskPipe, NgxMaskService, _configFactory, initialConfig, timeMasks, withoutValidation)

✖ Failed to compile.
```

### 2024-05-13 11:57:47am

Here's the closest I can get:

- Self-signed certificate (`/etc/pki/tls/certs/apache-selfsigned.crt`)
	- Key (`/etc/pki/tls/private/apache-selfsigned.key`)
	- These both exist and seem to be normal. Nothing out of the ordinary here. `dspace`, which is running Tomcat and will run the frontend as well, also has permissions for it.
- Virtual Host Configuration File: `/etc/httpd/conf.d/pedsdspace01.conf`
	~~- This step was not explicitly mentioned in the instructions, which makes me wonder if this might be part of the issue? Not sure. But it's not long:~~
	- Per [Apache docs](https://httpd.apache.org/docs/2.4/en/vhosts/examples.html#:~:text=Comments-,Running%20several%20name%2Dbased%20web%20sites%20on%20a%20single%20IP%20address.,-Your%20server%20has), this creates a virtual host that listens on port 80:

```
<VirtualHost *:80>
    ServerName pedsdspace01.research.chop.edu
    DocumentRoot /var/www/html

    # Redirect HTTP to HTTPS
    RewriteEngine On
    RewriteCond %{HTTPS} off
    RewriteRule ^(.*)$ https://%{HTTP_HOST}%{REQUEST_URI} [L,R=301]
</VirtualHost>
```

- Apache SSL Configuration File: `/etc/httpd/conf.d/ssl.conf`
	- I also have no reason not to believe this file has any issues. The instructions weren't clear about where [the stuff I have inserted in here goes](https://wiki.lyrasis.org/display/DSDOC7x/Installing+DSpace#InstallingDSpace-UsingaSelf-SignedSSLCertificatecausestheFrontendtonotbeabletoaccesstheBackend:~:text=Now%2C%20setup%20a%20new%20VirtualHost%20for%20your%20site%20(using%20HTTPS%20/%20port%20443)%20which%20proxies%20all%20requests%20to%20Tomcat%27s%20AJP%20connector%20(running%20on%20port%208009)). I was told online that it goes in this file.

When I log in with `dspace`, which has permissions to all of these files, and I call `node ./dist/server/main.js`, this is the output:

```
[dspace@pedsdspace01 dspace-ui-deploy]$ node ./dist/server/main.js
Building production app config
Unable to find default config file at /home/dspace/dspace-ui-deploy/config/config.yaml
Overriding app config with /home/dspace/dspace-ui-deploy/config/config.prod.yml
Angular config.json file generated correctly at /home/dspace/dspace-ui-deploy/dist/browser/assets/config.json 

Environment extended with app config

dspace-angular
Version: 7.6.1
Environment: Production

Service key not found at ./config/ssl/key.pem
Certificate not found at ./config/ssl/key.pem
Disabling certificate validation and proceeding with a self-signed certificate. If this is a production server, it is recommended that you configure a valid certificate instead.
[12:24:54 GMT-0400 (Eastern Daylight Time)] Listening at https://localhost:4000/
```

Now, it's not obvious to me why it's saying `Service key/Certificate` not found at `./config/ssl`.

There's some additional stuff here: [[Frontend Troubleshooting|Frontend Troubleshooting]]

But the one thing I can do that does give me some sort of response is this: I call `node ./dist/server/main.js` in `[dspace-ui]` as above, and I port-forward (`ssh -L 4000:localhost:4000 seyediana1@pedsdspace01.research.chop.edu`).

Here's what happens.

I go to `https://localhost:4000/`.

![[./docs/assets/img/Pasted image 20240513125050.png|Pasted image 20240513125050.png]]
If I click on `Advanced` and then `Proceed to localhost (unsafe)`, something happens. The page stays the same, but in the `node` terminal, I get:

```
[12:49:55 GMT-0400 (Eastern Daylight Time)] Listening at https://localhost:4000/
[HPM] Proxy created: /  -> https://pedsdspace01.research.chop.edu/server/sitemaps
[HPM] Proxy created: /  -> https://pedsdspace01.research.chop.edu/server
```

and in the port-forwarding terminal, I get:

```
[seyediana1@pedsdspace01 ~]$ channel 3: open failed: connect failed: Connection refused
channel 4: open failed: connect failed: Connection refused
channel 3: open failed: connect failed: Connection refused
etc.
```

I also tried to create `key.pem` by [`cat`ting the two together](https://stackoverflow.com/questions/991758/how-to-get-pem-file-from-key-and-crt-files) because of the earlier warning message:

```
Service key not found at ./config/ssl/key.pem
Certificate not found at ./config/ssl/key.pem
```

And nothing. Doesn't produce any difference.

If I look into the httpd logs, I see stuff like this:

```

[seyediana1@pedsdspace01 ~]$ sudo less /etc/httpd/logs/ssl_request_log
[13/May/2024:12:38:06 -0400] 10.30.11.25 TLSv1.3 TLS_AES_256_GCM_SHA384 "GET /server/api HTTP/1.1" 299
[13/May/2024:12:40:07 -0400] 10.30.11.25 TLSv1.3 TLS_AES_256_GCM_SHA384 "GET /server/api HTTP/1.1" 299
[13/May/2024:12:40:46 -0400] 10.30.11.25 TLSv1.3 TLS_AES_256_GCM_SHA384 "GET /server/api HTTP/1.1" 299

[seyediana1@pedsdspace01 ~]$ sudo cat /etc/httpd/logs/ssl_error_log
[Mon May 13 00:00:06.853728 2024] [ssl:warn] [pid 2936973:tid 2936973] AH01906: pedsdspace01.research.chop.edu:443:0 server certificate is a CA certificate (BasicConstraints: CA == TRUE !?)
[Mon May 13 00:00:06.854438 2024] [ssl:warn] [pid 2936973:tid 2936973] AH01906: pedsdspace01.research.chop.edu:443:1 server certificate is a CA certificate (BasicConstraints: CA == TRUE !?)
[Mon May 13 12:38:06.958944 2024] [proxy:error] [pid 305681:tid 305767] (111)Connection refused: AH00957: AJP: attempt to connect to 127.0.0.1:8009 (localhost:8009) failed
[Mon May 13 12:38:06.958995 2024] [proxy_ajp:error] [pid 305681:tid 305767] [client 10.30.11.25:40360] AH00896: failed to make connection to backend: localhost
[Mon May 13 12:40:07.830136 2024] [proxy:error] [pid 305682:tid 305844] (111)Connection refused: AH00957: AJP: attempt to connect to 127.0.0.1:8009 (localhost:8009) failed
[Mon May 13 12:40:07.830180 2024] [proxy_ajp:error] [pid 305682:tid 305844] [client 10.30.11.25:46366] AH00896: failed to make connection to backend: localhost
[Mon May 13 12:40:46.025352 2024] [proxy:error] [pid 305682:tid 305854] (111)Connection refused: AH00957: AJP: attempt to connect to 127.0.0.1:8009 (localhost:8009) failed
[Mon May 13 12:40:46.025387 2024] [proxy_ajp:error] [pid 305682:tid 305854] [client 10.30.11.25:53598] AH00896: failed to make connection to backend: localhost
[Mon May 13 13:01:15.167413 2024] [ssl:warn] [pid 481573:tid 481573] AH01906: pedsdspace01.research.chop.edu:443:0 server certificate is a CA certificate (BasicConstraints: CA == TRUE !?)
[Mon May 13 13:01:15.168041 2024] [ssl:warn] [pid 481573:tid 481573] AH01906: pedsdspace01.research.chop.edu:443:1 server certificate is a CA certificate (BasicConstraints: CA == TRUE !?)
[Mon May 13 13:01:15.177421 2024] [ssl:warn] [pid 481573:tid 481573] AH01906: pedsdspace01.research.chop.edu:443:0 server certificate is a CA certificate (BasicConstraints: CA == TRUE !?)
[Mon May 13 13:01:15.178028 2024] [ssl:warn] [pid 481573:tid 481573] AH01906: pedsdspace01.research.chop.edu:443:1 server certificate is a CA certificate (BasicConstraints: CA == TRUE !?)
[Mon May 13 13:02:35.781994 2024] [ssl:warn] [pid 482080:tid 482080] AH01906: pedsdspace01.research.chop.edu:443:0 server certificate is a CA certificate (BasicConstraints: CA == TRUE !?)
[Mon May 13 13:02:35.791304 2024] [ssl:warn] [pid 482080:tid 482080] AH01906: pedsdspace01.research.chop.edu:443:0 server certificate is a CA certificate (BasicConstraints: CA == TRUE !?)
```

And finally, if I turn off `ssl` in `config.prod.yml` (the config file for the NodeJS frontend), I do get the DSpace site:

![[./docs/assets/img/Pasted image 20240513133046.png|Pasted image 20240513133046.png]]

But it returns a 500 error, which suggests that it can't communicate with the backend.

It makes me wonder if it would be more worth it if I waited to get an SSL cert from IT.

### 2024-05-13 11:14:58am

I tried downloading the certificate and adding it to my lappy and still nothing. But it's for the site and not `localhost`.

### 2024-05-13 11:11:02am

This is a message that I typed out but then didn't send to Alex:

> I can't even replicate it like I did last week - I was really in the flow of things last Wednesday and now I'm struggling to get back into the swing of things. But basically I was able to load the DSpace logo and default layout except it gave me a 500 error - not sure why. And that was only when I connected with http, and not https. The backends all work (<http://localhost:8080/,> <http://localhost:8983/solr,> and <http://localhost:8080/server/#/server/api,> but I can't get past the SSL certificate part. The instructions on setting a self-signed certificate did not produce any change as far as I remember.

### 2024-05-13 10:55:36am

I just requested an SSL certificate CHOP's employee service portal. Ain't that something? I think this whole thing is going to be monumentally easier if I just get the certificate from the beginning.

### 2024-05-13 10:11:27am

I'm trying to pick back up from where I was and I'm finding it wildly difficult to reactivate my flow state. I kind of wanna go get (another) cup of coffee.

I think I like Claude more than GPT-4. I wish I could do both without it being $40. I guess I could if I just used APIs.

### 2024-05-08 6:27:56pm

I don't have the brain capacity to know where I left off. [[Claude Log Wednesday, May 08, 2024|Here is a Claude-generated log]]. I also made a video but it's half a gig, so just look on the Desktop.

### 2024-05-08 3:43:23pm

Okay. I just remembered that I had to [rebuild and reinstall DSpace](https://wiki.lyrasis.org/display/DSDOC7x/Configuration+Reference#:~:text=Instead%2C%20we%20recommend,to%20*.new%20files.) if I want to change the config files:

```sh
cd [dspace-source]/dspace/
mvn package
cd [dspace-source]/dspace/target/dspace-installer
ant update_configs
```

### 2024-05-08 3:35:13pm

I am so lost. At this point, just look at this [[How I Overcame the SSL Issue|Claude thread to figure out how I resolved the issue with the SSL certificate.]]

### 2024-05-08 1:41:45pm

Here's the DNS querying result when I try my own personal computer, not connected to the VPN. This makes sense because this is a tool intended to be used internally within the enterprise network.

```sh
(base) artas@MacBook-Pro ~ % nslookup pedsdspace01.research.chop.edu; dig pedsdspace01.research.chop.edu
Server:		192.168.0.1
Address:	192.168.0.1#53

** server can't find pedsdspace01.research.chop.edu: NXDOMAIN


; <<>> DiG 9.10.6 <<>> pedsdspace01.research.chop.edu
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NXDOMAIN, id: 22769
;; flags: qr rd ra; QUERY: 1, ANSWER: 0, AUTHORITY: 1, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 512
;; QUESTION SECTION:
;pedsdspace01.research.chop.edu.	IN	A

;; AUTHORITY SECTION:
research.chop.edu.	120	IN	SOA	ns10.chop.edu. please_set_email.absolutely.nowhere. 318 10800 3600 1209600 1800

;; Query time: 29 msec
;; SERVER: 192.168.0.1#53(192.168.0.1)
;; WHEN: Wed May 08 13:40:50 EDT 2024
;; MSG SIZE  rcvd: 135
```

Okay. [Alex has confirmed that yes](https://pedsnet.slack.com/archives/D06UK3WKQMU/p1715190252078889), it is to be expected that this website is not available to the external network, so I should have done what ClaudeGPT recommended I do this morning which is self-sign the certification:

> Yeah there isn't really anything we can do. For now just get a self signed cert and run with that, and then when everything is in place we can get a cert from IS that will cover things and be officially signed.

### 2024-05-08 12:41:12pm

I'm going to be completely honest. I have followed ChatGPT and Claude's advice to get this far. None of the instructions for getting this thing SSL-certified are working.

I tried uninstalling and reinstalling `certbot` to no avail. It was first telling me that there was not "virtual host" listening on port 80, even though, in the DSpace instructions, that was the step **after** this one. I'm supposed to be able to do this step without having to fiddle with the virtual host.

```
[seyediana1@pedsdspace01 ~]$ sudo certbot certonly --apache -d pedsdspace01.research.chop.edu

Saving debug log to /var/log/letsencrypt/letsencrypt.log

Requesting a certificate for pedsdspace01.research.chop.edu

Unable to find a virtual host listening on port 80 which is currently needed for Certbot to prove to the CA that you control your domain. Please add a virtual host for port 80.

Ask for help or search for solutions at https://community.letsencrypt.org. See the logfile /var/log/letsencrypt/letsencrypt.log or re-run Certbot with -v for more details.
```

So, at the advice of Claude, I (again; I had already did all of these steps once, undid them, and redid them to see if that would fix anything) added the virtual host to a configuration file for my domain:

```sh
sudo vi /etc/httpd/conf.d/pedsdspace01.conf
```

Here's what I added:

```
<VirtualHost *:80>
    ServerName pedsdspace01.research.chop.edu
    DocumentRoot /var/www/html
</VirtualHost>
```

These steps work fine

```
sudo apachectl configtest
sudo systemctl restart httpd
```

But eventually, I call `sudo certbot certonly --apache -d pedsdspace01.research.chop.edu`, which gives me the same error message as above.

So I used `nslookup` (DNS querying tool) and `dig` (another DNS querying tool) and get this:

```sh
[seyediana1@pedsdspace01 ~]$ nslookup pedsdspace01.research.chop.edu; dig pedsdspace01.research.chop.edu
Server:		10.26.3.200
Address:	10.26.3.200#53

Name:	pedsdspace01.research.chop.edu
Address: 10.30.11.25

; <<>> DiG 9.16.23-RH <<>> pedsdspace01.research.chop.edu
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 15629
;; flags: qr aa rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 4096
;; QUESTION SECTION:
;pedsdspace01.research.chop.edu.	IN	A

;; ANSWER SECTION:
pedsdspace01.research.chop.edu.	28800 IN A	10.30.11.25

;; Query time: 3 msec
;; SERVER: 10.26.3.200#53(10.26.3.200)
;; WHEN: Wed May 08 12:40:43 EDT 2024
;; MSG SIZE  rcvd: 75
```

### 2024-05-08 12:16:04pm

Okay. I'm just having an impossible time with `certbot` as I am sure you have noticed.

Here's the root of my issue:

```sh
[seyediana1@pedsdspace01 ~]$ sudo certbot --apache
Saving debug log to /var/log/letsencrypt/letsencrypt.log
Error while running apachectl configtest.

AH00526: Syntax error on line 85 of /etc/httpd/conf.d/ssl.conf:
SSLCertificateFile: file '/etc/pki/tls/certs/localhost.crt' does not exist or is empty

The apache plugin is not working; there may be problems with your existing configuration.
The error was: MisconfigurationError("Error while running apachectl configtest.\n\nAH00526: Syntax error on line 85 of /etc/httpd/conf.d/ssl.conf:\nSSLCertificateFile: file '/etc/pki/tls/certs/localhost.crt' does not exist or is empty\n")
[seyediana1@pedsdspace01 ~]$ sudo yum install python3-certbot-apache^C
[seyediana1@pedsdspace01 ~]$ ^C
[seyediana1@pedsdspace01 ~]$ sudo vi /etc/httpd/conf.d/ssl.conf
[sudo] password for seyediana1: 
Sorry, try again.
[sudo] password for seyediana1: 
Sorry, try again.
[sudo] password for seyediana1: 
sudo: 3 incorrect password attempts
[seyediana1@pedsdspace01 ~]$ sudo vi /etc/httpd/conf.d/ssl.conf
[sudo] password for seyediana1: 
[seyediana1@pedsdspace01 ~]$ sudo vi /etc/httpd/conf.d/ssl.conf
[seyediana1@pedsdspace01 ~]$ sudo apachectl configtest
Syntax OK
[seyediana1@pedsdspace01 ~]$ sudo systemctl restart httpd
Job for httpd.service failed because the control process exited with error code.
See "systemctl status httpd.service" and "journalctl -xeu httpd.service" for details.
[seyediana1@pedsdspace01 ~]$ systemctl status httpd.service
× httpd.service - The Apache HTTP Server
     Loaded: loaded (/usr/lib/systemd/system/httpd.service; enabled; preset: disabled)
     Active: failed (Result: exit-code) since Wed 2024-05-08 11:50:06 EDT; 8s ago
   Duration: 18h 59min 21.556s
       Docs: man:httpd.service(8)
    Process: 2892980 ExecStart=/usr/sbin/httpd $OPTIONS -DFOREGROUND (code=exited, status=1/FAILURE)
   Main PID: 2892980 (code=exited, status=1/FAILURE)
     Status: "Reading configuration…"
        CPU: 45ms
[seyediana1@pedsdspace01 ~]$ sudo vi /etc/httpd/conf.d/ssl.conf
[seyediana1@pedsdspace01 ~]$ systemctl status httpd.service
× httpd.service - The Apache HTTP Server
     Loaded: loaded (/usr/lib/systemd/system/httpd.service; enabled; preset: disabled)
     Active: failed (Result: exit-code) since Wed 2024-05-08 11:50:06 EDT; 32s ago
   Duration: 18h 59min 21.556s
       Docs: man:httpd.service(8)
    Process: 2892980 ExecStart=/usr/sbin/httpd $OPTIONS -DFOREGROUND (code=exited, status=1/FAILURE)
   Main PID: 2892980 (code=exited, status=1/FAILURE)
     Status: "Reading configuration…"
        CPU: 45ms
[seyediana1@pedsdspace01 ~]$ sudo systemctl restart httpd
[seyediana1@pedsdspace01 ~]$ systemctl status httpd.service
● httpd.service - The Apache HTTP Server
     Loaded: loaded (/usr/lib/systemd/system/httpd.service; enabled; preset: disabled)
     Active: active (running) since Wed 2024-05-08 11:50:43 EDT; 2s ago
       Docs: man:httpd.service(8)
   Main PID: 2893107 (httpd)
     Status: "Started, listening on: port 443, port 80"
      Tasks: 213 (limit: 100254)
     Memory: 41.6M
        CPU: 104ms
     CGroup: /system.slice/httpd.service
             ├─2893107 /usr/sbin/httpd -DFOREGROUND
             ├─2893109 /usr/sbin/httpd -DFOREGROUND
             ├─2893110 /usr/sbin/httpd -DFOREGROUND
             ├─2893111 /usr/sbin/httpd -DFOREGROUND
             └─2893112 /usr/sbin/httpd -DFOREGROUND
```

Here, ChatGPT instructed me to comment out `SSLCertificateFile /etc/pki/tls/certs/localhost.crt` on line 85. Well, I did that, and restarting `httpd` didn't work, so I uncommented them again, and now it works. Dumb.

Also.

Once I installed `python3-certbot-apache` (even though I also installed `certbot` after removed it like the instructions on the Certbot's website said):

```sh
sudo yum remove certbot
# I couldn't get snap to install or work. It didn't like these commands.
# sudo snap install --classic certbot
# sudo snap install certbot

# This worked:
sudo yum install certbot

which certbot
# /usr/bin/certbot
```

### 2024-05-08 12:05:25pm

#### Steps to Resolve SSL Certificate Issue and Update DNS for Public Access

1. **Ensure `Listen 80` is Present:**
   - Confirmed `Listen 80` is already in the Apache configuration, so no need to add it again.

2. **Create a New Virtual Host Configuration File:**
   - Created a new configuration file for the domain:

     ```sh
     sudo vi /etc/httpd/conf.d/pedsdspace01.conf
     ```

3. **Add the Virtual Host Configuration:**
   - Added the following content to set up a virtual host for port 80:

     ```apache
     <VirtualHost *:80>
         ServerName pedsdspace01.research.chop.edu
         DocumentRoot /var/www/html

         <Directory /var/www/html>
             Options Indexes FollowSymLinks
             AllowOverride All
             Require all granted
         </Directory>

         ErrorLog /var/log/httpd/pedsdspace01_error.log
         CustomLog /var/log/httpd/pedsdspace01_access.log combined
     </VirtualHost>
     ```

4. **Test the Apache Configuration:**
   - Tested the configuration to ensure there are no syntax errors:

     ```sh
     sudo apachectl configtest
     ```

5. **Restart Apache:**
   - Restarted the Apache service to apply the new configuration:

     ```sh
     sudo systemctl restart httpd
     ```

6. **Verify DNS Records:**
   - Checked that `pedsdspace01.research.chop.edu` resolves to a public IP address. Current configuration shows a private IP (10.30.11.25), which is not accessible from the public internet.

7. **Update DNS Records:**
   - Updated DNS records to point to the public IP address of the server. Used the DNS provider's management console to add/update the A record:
     - **Hostname:** `pedsdspace01.research.chop.edu`
     - **Type:** `A`
     - **Value:** [Public IP Address]

8. **Wait for DNS Propagation:**
   - Allowed time for DNS changes to propagate across the internet.

9. **Retry Certbot to Obtain Certificate:**
   - After DNS records updated, ran Certbot to obtain the SSL certificate:

     ```sh
     sudo certbot certonly --apache
     ```

10. **Follow Certbot Prompts:**
    - Entered email address and agreed to the terms of service.
    - Chose to share email address with the EFF (optional).
    - Entered domain name (`pedsdspace01.research.chop.edu`).

11. **Update SSL Configuration:**
    - Edited the `ssl.conf` file to point to the new certificates:

      ```sh
      sudo vi /etc/httpd/conf.d/ssl.conf
      ```

      Updated lines:

      ```apache
      SSLCertificateFile /etc/letsencrypt/live/pedsdspace01.research.chop.edu/fullchain.pem
      SSLCertificateKeyFile /etc/letsencrypt/live/pedsdspace01.research.chop.edu/privkey.pem
      ```

12. **Test and Restart Apache:**
    - Tested Apache configuration:

      ```sh
      sudo apachectl configtest
      ```

    - Restarted Apache:

      ```sh
      sudo systemctl restart httpd
      ```

13. **Verify HTTPS Access:**
    - Opened a web browser and visited `https://pedsdspace01.research.chop.edu` to verify that the SSL certificate is properly installed and the site loads correctly.

### 2024-05-08 10:56:53am

I'm following the instructions to [install Certbot](https://certbot.eff.org/instructions?ws=apache&os=fedora).

```
sudo snap install --classic certbot
```

This command, even after I installed `snapd`, isn't working.

### 2024-05-08 10:14:31am

I'm back.

The Zionist regime of Israel is committing a holocaust against the indigenous people of Palestine with full US complicity and support.

Here are the port-forwarding commands

```sh
ssh -L 8080:localhost:8080 seyediana1@pedsdspace01.research.chop.edu
ssh -L 8983:localhost:8983 seyediana1@pedsdspace01.research.chop.edu
ssh -L 4000:localhost:4000 seyediana1@pedsdspace01.research.chop.edu
```

The first one is Tomcat, the second one is Solr, and honestly idk what the 3rd one is. I think nothing.

### 2024-05-07 4:47:13pm

Here's another example of the instructions just hoping you figure things out:

> 1. Restart Apache to enable these modules

Okay, how do I do that? I guess I just have to call `apachectl restart` because, I don't know, sounds right.

```
[seyediana1@pedsdspace01 conf.modules.d]$ apachectl restart
==== AUTHENTICATING FOR org.freedesktop.systemd1.manage-units ====
Authentication is required to restart 'httpd.service'.
Authenticating as: root
Password: 
==== AUTHENTICATION COMPLETE ====
```

That's where I'm going to leave things for today. Pick it back up [here](https://wiki.lyrasis.org/display/DSDOC7x/Installing+DSpace#InstallingDSpace-BackendInstallation:~:text=Obtain%20an%20SSL%20certificate%20for%20HTTPS%20support.%20If%20you%20don%27t%20have%20one%20yet%2C%20you%20can%20use%20Let%27s%20Encrypt%20(for%20free)%20using%20the%20%22certbot%22%20tool%3A%20https%3A//certbot.eff.org/%C2%A0).

### 2024-05-07 4:41:27pm

The instructions are outdated or just not right for RHEL and never exactly explain how to install or use these modules so I had to check whether these modules are there myself:

```
cd /etc/httpd/conf.modules.d/
ls -l
grep -i 'LoadModule' *.conf
```

They were all there. But actually I didn't even need to do that.

```sh
[seyediana1@pedsdspace01 conf.modules.d]$ httpd -M | grep -E "headers|proxy"
 headers_module (shared)
 proxy_module (shared)
 proxy_ajp_module (shared)
 proxy_balancer_module (shared)
 proxy_connect_module (shared)
 proxy_express_module (shared)
 proxy_fcgi_module (shared)
 proxy_fdpass_module (shared)
 proxy_ftp_module (shared)
 proxy_http_module (shared)
 proxy_hcheck_module (shared)
 proxy_scgi_module (shared)
 proxy_uwsgi_module (shared)
 proxy_wstunnel_module (shared)
 proxy_http2_module (shared)
```

They're all there but under different names. So what the hell was the point of any of that.

### 2024-05-07 4:31:09pm

I just installed https like so:

```sh
sudo yum install httpd
sudo systemctl enable httpd
sudo systemctl start httpd
```

### 2024-05-07 4:23:51pm

Okay so I realized that the reason was that I was trying to connect to the URL again instead of `localhost` after port forwarding. I'm not going to get into it here but basically it was a non-issue.

On a separate note, I skipped a few sections before going to the frontend part so I went back and I had to edit the `cron` file for the DSpace user. I was getting this error:

```
You (dspace) are not allowed to access to (crontab) because of pam configuration.
```

So I tried to figure it out with ChatGPT and was getting nowhere, so I tried a quick Google search and found [this](https://serverfault.com/a/620186).

```
sudo vim /etc/security/access.conf
```

Add this line to the file above:

```
+: dspace: cron crond:0
```

### 2024-05-07 4:06:37pm

I've been getting lazy about taking notes.

But basically I'm trying to install the front end and I'm getting the following error:

```sh
node ./dist/server/main.js
```

But I just realized I accidentally skipped the part where I'm supposed to get an SSL certificate.

### 2024-05-07 2:55:14pm

Downloading version 20 of Node.Js:

```sh
sudo dnf module -y install nodejs:20
```

### 2024-05-07 12:53:09pm

I did it. I got the REST API backend.

![[./docs/assets/img/Pasted image 20240507125320.png|Pasted image 20240507125320.png]]

### 2024-05-07 10:57:07am

Okay. I tried to update the configurations of DSpace and I assumed I had to delete the installation directory to start over again, but only after I did that did I find out that there is a command to [just update the configurations.](https://wiki.lyrasis.org/display/DSDOC7x/Configuration+Reference#ConfigurationReference-Thelocal.cfgConfigurationPropertiesFile:~:text=One%20way%20to,ant%20update_configs) Oops. Oh well.

Also I'm adding `update-statistics.dbfile` in `local.cfg`.

### 2024-05-07 10:26:41am

It doesn't work.

I tried port-forwarding the default backend server page:

```sh
ssh -L 8080:localhost:8080/server seyediana1@pedsdspace01.research.chop.edu
```

But it's giving me 404 at <http://localhost:8080/server/>. I think it's because I removed the `/server` from the config file. I need to reinstall DSpace.

I can, however, get Tomcat:

![[./docs/assets/img/Pasted image 20240507102952.png|Pasted image 20240507102952.png]]

But I can't get the REST API backend as [demonstrated here](https://demo.dspace.org/server/#/server/api).

### 2024-05-07 10:21:44am

I created an admin account:

```sh
[dspace@pedsdspace01 dspace]$ ./bin/dspace create-administrator
Creating an initial administrator account
E-mail address: seyediana1@chop.edu
First name: Arta
Last name: Seyedian 
Is the above data correct? (y or n): y
Password will not display on screen.
Password: admin
Again to confirm: admin
Administrator account created
```

### 2024-05-07 9:59:52am

Alex suggested that I use port forwarding, which is something that didn't occur to me.

![[./docs/assets/img/Pasted image 20240507100122.png|Pasted image 20240507100122.png]]

His suggestion worked 🙏.

### 2024-05-06 5:03:50pm

Okay. I am tired so I'm just gonna plug in a [[Post-DSpace Backend Installation Checkpoint|GPT-generated log of where I'm leaving off]].

### 2024-05-06 4:26:17pm

I have to create a user for `solr`:

```sh
useradd -m solr
```

I'm sticking to my guns, these directions suck.

### 2024-05-06 3:41:32pm

For the first time during this whole process, I am actually furious. I am livid.

These animals release new versions of DSpace but don't update the installation instructions. [Buried deep within the release notes](https://wiki.lyrasis.org/display/DSDOC7x/Release+Notes#:~:text=DSpace%20now%20has,oai.path%3Doai%22) is the following:

> **DSpace now has a single, backend "server" webapp to deploy in Tomcat (or similar).** In DSpace 6.x and below, different machine interfaces (OAI-PMH, SWORD v1 or v2, RDF, REST API) were provided via separate deployable webapps.  Now, all those interfaces along with the new REST API are in a single, "server" webapp built on [Spring Boot](https://spring.io/projects/spring-boot).  You can now control which interfaces are enabled, and what path they appear on via configuration (e.g. "oai.enabled=true" and "oai.path=oai")

Amazing. Great. Thanks for letting us know. The violent irony is that the installation instructions were for DSpace 7 and not DSpace 6. they just forgot to or otherwise didn't want to (sadistically) let us know.

Anyway, thanks for the open source software lol. I guess I could suggest an edit to them. Or several.

Oh.

[These installation instructions](https://wiki.lyrasis.org/display/DSDOC6x/Installing+DSpace#InstallingDSpace-RelationalDatabase:(PostgreSQLorOracle)), which, I'm honestly not sure where I got them from, are for DSpace 6. Not 7. So it's 100% on me. Whoops lol. I am the animal.

Well. [Here is the documentation](https://wiki.lyrasis.org/display/DSDOC7x/Installing+DSpace). Useful to find it when I'm basically done.

Just glancing at the docs, it looks like that's the only major difference.

### 2024-05-06 3:18:39pm

Here are the settings I've updated in your DSpace configuration for future reference:

- **DSpace server URL**: `http://pedsdspace01.research.chop.edu:8080`
- **DSpace UI URL**: `http://pedsdspace01.research.chop.edu:4000`
- **Database URL**: `jdbc:postgresql://pedsdspace01.research.chop.edu:5432/dspace`

The documentation is pretty bad. I've just run into a lot of bad or outdated documentation doing this. For example, just for DSpace, the fields in the config documentation do not match up with the actual fields in `local.cfg.EXAMPLE`. The documentation also says nothing about how `/server` limits which web apps you get, though, I feel like I'm being a perfectionist. Like I probably didn't need to spend all that time figuring out how to get those dumbass dependencies to work for Apache Ant, since I'm probably not going to be using them anyway.

### 2024-05-06 2:57:59pm

I deleted DSpace and unpacked it again. This is the `local.cfg` file:

```sh
dspace.server.url = http://localhost:8080/server
```

So the `/server` part of the URL might explain why I only found a `/server` folder in the `webapps` folder.

### 2024-05-06 2:44:51pm

I'm running into another issue.

There is a fundamental mismatch between what is supposed to be in the `dspace/webapps` folder and what actually is:

```sh
[dspace@pedsdspace01 opt]$ ls dspace/webapps/
server
```

There's supposed to be a bunch of web apps in here. In fact, it's even in the `stdout` of the installation:

```sh
copy_webapps:

     [copy] Copying 1166 files to /opt/dspace/webapps

     [copy] Copying 1 file to /opt/dspace/webapps
```

### 2024-05-06 2:41:04pm

Hooray! The development build is successful!!!!!

```sh
     [echo]  The DSpace code has been installed.
     [echo] 
     [echo]  To complete installation, you should do the following:
     [echo] 
     [echo]  * Setup your Web servlet container (e.g. Tomcat) to look for your
     [echo]    DSpace web applications in: /opt/dspace/webapps/
     [echo] 
     [echo]    OR, copy any web applications from /opt/dspace/webapps/ to
     [echo]    the appropriate place for your servlet container.
     [echo]    (e.g. '$CATALINA_HOME/webapps' for Tomcat)
     [echo] 
     [echo]  * Start up your servlet container (e.g. Tomcat). DSpace now will
     [echo]    initialize the database on the first startup.
     [echo] 
     [echo]  * Make an initial administrator account (an e-person) in DSpace:
     [echo] 
     [echo]    /opt/dspace/bin/dspace create-administrator
     [echo] 
     [echo]  You should then be able to access your DSpace's REST API:
     [echo] 
     [echo]    http://localhost:8080/server
     [echo] 
     [echo] ====================================================================
     [echo]         

BUILD SUCCESSFUL
Total time: 6 seconds
```

### 2024-05-06 2:32:48pm

I had to install `pgcrypto`:

```sh
sudo yum install postgresql16-contrib
```

Then I had to recreate it:

```sh
psql --username=postgres dspace -c "CREATE EXTENSION pgcrypto;"
```

### 2024-05-06 2:27:06pm

Okay. this is not working. I can't get `pgcrypto` to exist even though I remember it working before.

Also, I'm going through the `psql` installation guide and I noticed something off. [The modification to `pg_hba.conf`](https://wiki.lyrasis.org/display/DSDOC6x/Installing+DSpace#InstallingDSpace-RelationalDatabase:(PostgreSQLorOracle):~:text=host%20dspace%20dspace%20127.0.0.1%20255.255.255.255%20md5) should be:

```
host    dspace          dspace          127.0.0.1/32            md5
```

### 2024-05-06 12:54:38pm

I have to do this shit all over again because I fucked up.

```sh
[seyediana1@pedsdspace01 opt]$ sudo chown -R dspace:dspace DSpace-dspace-7.6.1/

[seyediana1@pedsdspace01 opt]$ sudo chown -R dspace:dspace dspace/
```

What a pain. Worst comes to worse, I'll just use the Docker container.

### 2024-05-06 12:49:51pm

What's kind of driving me crazy is that this whole thing is available via Docker. So all of this was potentially pointless.

### 2024-05-06 12:44:19pm

Well. I done did it again. I accidentally deleted my whole DSpace source folder and have to start over again. What a pain in the ass. At least it's just the config file.

### 2024-05-06 12:20:52pm

Okay. I'm pretty much back to where I was before. I installed DSpace. I'm picking up from [here](https://wiki.lyrasis.org/display/DSDOC7x/Installing+DSpace#:~:text=Install%20DSpace%20Backend%3A%20As%20the%20dspace%20UNIX%20user%2C%20install%20DSpace%20to%20%5Bdspace%5D%3A).

```
cd [dspace-source]/dspace/target/dspace-installer
ant fresh_install
```

### 2024-05-06 11:44:20am

Okay. Tomcat is working under DSpace now. Hurray.

### 2024-05-06 10:46:29am

- [x] Apache Maven ✅ 2024-05-06
- [x] Apache Solr ✅ 2024-05-06
- [x] Apache Ant ✅ 2024-05-06
- [x] Apache Tomcat ✅ 2024-05-06
	- Turns out I don't have `sudo yum install java-17-openjdk-devel` installed; weird how it's a separate thing. Actually kind of frustrating. Oh well.

Still installing Tomcat. I made it so anyone in the `dspace` group (which includes `seyediana1` and `dspace`) has full permissions and ownership over the Tomcat directory:

```sh
sudo chown :dspace apache-tomcat-9.0.88 -R
sudo chmod 770 apache-tomcat-9.0.88 -R
```

```sh
./bin/jsvc \
    -stop \
    -pidfile /opt/apache-tomcat-9.0.88/temp/jsvc.pid \
    -classpath /opt/apache-tomcat-9.0.88/bin/bootstrap.jar:/opt/apache-tomcat-9.0.88/bin/tomcat-juli.jar \
    -outfile /opt/apache-tomcat-9.0.88/logs/catalina.out \
    -errfile /opt/apache-tomcat-9.0.88/logs/catalina.err \
    -Dcatalina.home=/opt/apache-tomcat-9.0.88 \
    -Dcatalina.base=/opt/apache-tomcat-9.0.88 \
    -Djava.util.logging.manager=org.apache.juli.ClassLoaderLogManager \
    -Djava.util.logging.config.file=/opt/apache-tomcat-9.0.88/conf/logging.properties \
    org.apache.catalina.startup.Bootstrap
```

### 2024-05-06 10:10:17am

I'm back. And I'm ready for revenge.

I just `sudo yum remove`d `sudo yum remove java-1.8.0-openjdk`.

Now I have to start over and get rid of the dependencies I built with Java.

Now I'm going to call this fella: `sudo yum install java-17-openjdk`.

### 2024-05-01 4:24:12pm

Given that I have a few weeks to finish this and I already meticulously documented everything, I think it might be the wisest choice not to juggle multiple JDK versions and then wonder why shit crashes all of the time, but use the most up-to-date version of JDK that's still compatible with DSpace (JDK 17).

I think the reason I screwed up is because the first link they open for instructions on how to install JDK is openjdk.org/install/, which, visually, is not super straightforward:

![[./docs/assets/img/Pasted image 20240501163552.png|Pasted image 20240501163552.png]]

My eyes immediately gravitated towards JDK 8 because I saw that it was providing the breakdown of how to download/install it based on your particular

### 2024-05-01 4:10:17pm

Well. I installed OpenJDK version 1.8, which corresponds to JDK 8, and I'm pretty sure I built all of the dependencies so far against OpenJDK version 1.8, and now I'm seeing that DSpace *requires* JDK 11 or 17. But the thing is that I thought I saw somewhere that one of the dependencies specifically asked for version 8? Maybe it was Maven? I'm not really sure… But I wonder if this means I have to just scrap everything and start all over again…

### 2024-05-01 3:53:24pm

Okay. I got an error trying to[ build DSpace](https://wiki.lyrasis.org/display/DSDOC7x/Installing+DSpace#InstallingDSpace-(Optional)IPtoCityDatabaseforLocation-basedStatistics:~:text=Build%20the%20Installation%20Package%3A%20As%20the%20dspace%20UNIX%20user%2C%20generate%20the%20DSpace%20installation%20package.).

```
[**WARNING**] Rule 0: org.apache.maven.plugins.enforcer.RequireJavaVersion failed with message:

Detected JDK Version: 1.8.0-402 is not in the allowed range 11.

[**INFO**] **------------------------------------------------------------------------**

[**INFO**] **Reactor Summary for DSpace Parent Project 7.6.1:**

[**INFO**] 

[**INFO**] DSpace Parent Project .............................. **FAILURE** [ 13.692 s]

[**INFO**] DSpace Services Framework :: API and Implementation  **SKIPPED**

[etc...]
```

ChatGPT is saying it's because I'm using the wrong version of Java? But I followed the instructions where it said to use OpenJDK…

### 2024-05-01 3:47:00pm

Okay. Now I'm actually using `vim` for once, making changes to `local.cfg` in the DSpace source folder.

There's a whole section dedicated to email:

```
[REDACTED for brevity]
```

Might be something to look into if we want to better monitor DSpace.

### 2024-05-01 2:52:00pm

Okay. I think I'm done with Tomcat for now. Let's just get to the part where I inevitably have to troubleshoot the stupid thing.

Let's install the backend. Starting with Postgres.

As before, I am calling the following commands to actually get into the Postgres shell:

```sh
sudo -iu postgres
psql
```

```sh
createuser --username=postgres --no-superuser --pwprompt dspace
```

### 2024-05-01 2:26:57pm

```sh
CATALINA_BASE=$CATALINA_HOME
cd $CATALINA_HOME
./bin/jsvc \
	-pidfile /opt/apache-tomcat-9.0.88/temp/jsvc.pid \
    -classpath $CATALINA_HOME/bin/bootstrap.jar:$CATALINA_HOME/bin/tomcat-juli.jar \
    -outfile $CATALINA_BASE/logs/catalina.out \
    -errfile $CATALINA_BASE/logs/catalina.err \
    -Dcatalina.home=$CATALINA_HOME \
    -Dcatalina.base=$CATALINA_BASE \
    -Djava.util.logging.manager=org.apache.juli.ClassLoaderLogManager \
    -Djava.util.logging.config.file=$CATALINA_BASE/conf/logging.properties \
    org.apache.catalina.startup.Bootstrap
```

Okay so the problem was literally that `dspace` didn't have permission to write the `pid` file to `/var/run` and so I just changed to location of where it could write using the `-pidfile` flag.

Here's the command to stop:

```sh
./bin/jsvc \
    -stop \
    -pidfile /opt/apache-tomcat-9.0.88/temp/jsvc.pid \
    -classpath $CATALINA_HOME/bin/bootstrap.jar:$CATALINA_HOME/bin/tomcat-juli.jar \
    -outfile $CATALINA_HOME/logs/catalina.out \
    -errfile $CATALINA_HOME/logs/catalina.err \
    -Dcatalina.home=$CATALINA_HOME \
    -Dcatalina.base=$CATALINA_HOME \
    -Djava.util.logging.manager=org.apache.juli.ClassLoaderLogManager \
    -Djava.util.logging.config.file=$CATALINA_HOME/conf/logging.properties \
    org.apache.catalina.startup.Bootstrap
```

### 2024-05-01 12:27:08pm

Okay. Not sure why the Tomcat directories are under `root`; ChatGPT is saying it's because I did the extraction using `sudo`. Oh well. Now we are going to change ownership.

```sh
sudo chown -R dspace:dspace /opt/apache-tomcat-9.0.88
sudo chmod -R 755 /opt/apache-tomcat-9.0.88
sudo chmod -R 700 /opt/apache-tomcat-9.0.88/conf
```

Also I added myself to the `dspace` group so I can feel less insane:

```sh
sudo usermod -a -G dspace seyediana1
groups seyediana1
```

Here's the password for `dspace`:

```
pedsnet-metadata
```

Although I guess I didn't need to do that because I was allowed to log into `dspace` with no password with just the following command:

```
sudo -u dspace bash
```

I gave the `dspace` boy all of the same `$PATH`s as before:

```sh
export JAVA_HOME=/etc/alternatives/java_sdk_1.8.0_openjdk
export ANT_HOME=/opt/apache-ant-1.9.16
export CATALINA_HOME=/opt/apache-tomcat-9.0.88

export PATH=$JAVA_HOME/bin:\
$ANT_HOME/bin:\
$HOME/.local/bin:\
$HOME/bin:\
/opt/apache-maven-3.9.6/bin:\
/usr/local/bin:\
/usr/bin:\
/usr/local/sbin:\
/usr/sbin:\
/opt/puppetlabs/bin:\
$PATH
```

### 2024-05-01 12:12:27pm

Root shenanigans.

I changed the password for root to my CHOP password, then I realized that that was a bad idea, so I told Alex I changed the password and anyway I realized I was doing it wrong. I changed the root password again to

```
dspace-pedsnet
```

Anyway, I just created

```sh
useradd -m dspace
```

And I don't know. I guess I can just use that user. I'm just confused. I guess my reading of the instructions was correct, which is that I install first then give the new account permissions to use it. This is so confusing.

### 2024-05-01 11:57:35am

I have to be a `root` user to enter the Tomcat directory and actually install it.

`sudo cd $CATALINA_HOME/bin` doesn't work. So I had to set

### 2024-05-01 10:59:39am

I'm reading the documentation for Tomcat. There's something called `CATALINA_HOME` and another thing called `CATALINA_BASE`.

```sh
 CATALINA_BASE=/tmp/tomcat_base1 bin/catalina.sh start
```

But apparently `CATALINA_HOME` is the only mandatory one. It doesn't tell me to do this explicitly but I guess I can stick it in my `$PATH` variable in `~/.bashrc`.

```sh
# Set environment variables for various tools
export JAVA_HOME=/etc/alternatives/java_sdk_1.8.0_openjdk
export ANT_HOME=/opt/apache-ant-1.9.16
export CATALINA_HOME=/opt/apache-tomcat-9.0.88

# Consolidate PATH settings
export PATH=$JAVA_HOME/bin:\
$ANT_HOME/bin:\
$HOME/.local/bin:\
$HOME/bin:\
/opt/apache-maven-3.9.6/bin:\
/usr/local/bin:\
/usr/bin:\
/usr/local/sbin:\
/usr/sbin:\
/opt/puppetlabs/bin:\
$PATH
```

### 2024-05-01 10:54:00am

I'm at the step where I'm given the "option" to create a user account called `dspace`. But it's also saying:

> The choice that makes the most sense for you will probably depend on how you installed your servlet container (Tomcat/Jetty/etc).  If you installed it from source, you will need to create a user account to run it, and that account can be named anything, e.g. 'dspace'.  If you used your operating system's package manager to install the container, then a user account should have been created as part of that process and it will be much easier to use that account than to try to change it.

I downloaded Tomcat and stuck it in my `/opt` folder, but I haven't actually [installed](https://tomcat.apache.org/tomcat-9.0-doc/setup.html#Introduction) it, I'm now realizing. So I'm going to backtrack a little bit and try to install the dang thing.

### 2024-05-01 10:51:21am

Okay apparently I also need to alter the `local.cfg` file but I think that might be after I install DSpace.

> - Once the "dbip-city-lite.mmdb" database file is installed on your system,  you will need to configure its location as the value of `usage-statistics.dbfile` in your `local.cfg` configuration file.

Let's make a note for myself:

- [x] Alter `local.cfg` file after installing DSpace. ➕ 2024-05-01 ✅ 2024-05-08

### 2024-05-01 10:23:22am

It would be cool, though, not totally necessary, obviously, to see where people would access DSpace from. So I'm going to try to install [DP-IP's City Lite database](https://db-ip.com/db/lite.php) in the hopes that it will be easy to implement.

Once it's up-and-running and there's an HTML page, I need to install this snippet to actually get the results:

```javascript
<a href='https://db-ip.com'>IP Geolocation by DB-IP</a>
```

### 2024-04-30 4:37:32pm

Here's something else I should probably hold onto:

> #### (Optional) IP to City Database for Location-based Statistics
>
> Optionally, if you wish to record the geographic locations of clients in DSpace usage statistics records, you will need to install (and regularly update) one of the following:
>
> - Either, a copy of [MaxMind's GeoLite City database](https://dev.maxmind.com/geoip/geoip2/geolite2/) (in MMDB format)
>     - NOTE: Installing MaxMind GeoLite2 is _free._  However, you **must** sign up for a (free) MaxMind account in order to obtain a license key to use the GeoLite2 database.
>     - You may download GeoLite2 directly from MaxMind, or many Linux distributions provide the `geoipupdate` tool directly via their package manager.  You will still need to configure your license key prior to usage.
>     - Once the "GeoLite2-City.mmdb" database file is installed on your system,  you will need to configure its location as the value of `usage-statistics.dbfile` in your `local.cfg` configuration file. 
>     - See the "Managing the City Database File" section of [SOLR Statistics](https://wiki.lyrasis.org/display/DSDOC7x/SOLR+Statistics) for more information about using a City Database with DSpace.
> - Or, you can alternatively use/install [DB-IP's City Lite database](https://db-ip.com/db/download/ip-to-city-lite) (in MMDB format)
>     - This database is also free to use, but does **not** require an account to download.
>     - Once the "dbip-city-lite.mmdb" database file is installed on your system,  you will need to configure its location as the value of `usage-statistics.dbfile` in your `local.cfg` configuration file. 
>     - See the "Managing the City Database File" section of [SOLR Statistics](https://wiki.lyrasis.org/display/DSDOC7x/SOLR+Statistics) for more information about using a City Database with DSpace.

I feel like I'm approaching the end of whatever this thing is today that I'm working on.

Okay. A halt. Not a screeching one. But the train is slowing down and nearing the station. I'm packing up my bags, making sure I have my keys. I glance over at my neighbor sitting directly adjacent to me. They had the window seat, and at one point they fell asleep with their head rested on the window. I can tell they're awake from the reflection of their eyes in the window, but they won't turn around to look at me, and that's fine. I don't want to bother anyone. I don't need anything from them. But I couldn't help that we shared something special, them and I, on this train ride. As this fleeting thought exits my mind I get up and get off the train and maybe go find a bathroom to vape in.

[Here's my bookmark, at **Backend Installation**.](https://wiki.lyrasis.org/display/DSDOC7x/Installing+DSpace#InstallingDSpace-UNIX-likeOSorMicrosoftWindows:~:text=Database%20with%20DSpace.-,Backend%20Installation,-Install%20all%20the)

### 2024-04-30 4:33:09pm

Okay, weird: I can't enter any of the folders inside of the Tomcat directory because I don't have permissions; `sudo cd` just returns me back to the directory I'm in and I can only look inside with `sudo ls -alh`. Kind of fishy but for now, just something to keep in mind.

```sh
[seyediana1@pedsdspace01 apache-tomcat-9.0.88]$ pwd -P
/opt/apache-tomcat-9.0.88
[seyediana1@pedsdspace01 apache-tomcat-9.0.88]$ sudo cd bin/
[seyediana1@pedsdspace01 apache-tomcat-9.0.88]$ sudo cd lib/
[seyediana1@pedsdspace01 apache-tomcat-9.0.88]$ sudo ls bin/
bootstrap.jar	    commons-daemon.jar		  digest.sh	    shutdown.sh		  tool-wrapper.sh
catalina.bat	    commons-daemon-native.tar.gz  makebase.bat	    startup.bat		  version.bat
catalina.sh	    configtest.bat		  makebase.sh	    startup.sh		  version.sh
catalina-tasks.xml  configtest.sh		  setclasspath.bat  tomcat-juli.jar
ciphers.bat	    daemon.sh			  setclasspath.sh   tomcat-native.tar.gz
ciphers.sh	    digest.bat			  shutdown.bat	    tool-wrapper.bat
[seyediana1@pedsdspace01 apache-tomcat-9.0.88]$ sudo ls lib
annotations-api.jar	  ecj-4.20.jar	   tomcat-api.jar      tomcat-i18n-ja.jar     tomcat-util.jar
catalina-ant.jar	  el-api.jar	   tomcat-coyote.jar   tomcat-i18n-ko.jar     tomcat-util-scan.jar
catalina-ha.jar		  jasper-el.jar    tomcat-dbcp.jar     tomcat-i18n-pt-BR.jar  tomcat-websocket.jar
catalina.jar		  jasper.jar	   tomcat-i18n-cs.jar  tomcat-i18n-ru.jar     websocket-api.jar
catalina-ssi.jar	  jaspic-api.jar   tomcat-i18n-de.jar  tomcat-i18n-zh-CN.jar
catalina-storeconfig.jar  jsp-api.jar	   tomcat-i18n-es.jar  tomcat-jdbc.jar
catalina-tribes.jar	  servlet-api.jar  tomcat-i18n-fr.jar  tomcat-jni.jar
```

[I honestly don't quite understand the documentation here](https://tomcat.apache.org/tomcat-9.0-doc/introduction.html). Whatever. Who cares. As long as I don't have to start anything.

### 2024-04-30 4:25:21pm

Alright. Time to install Tomcat.

!!!!!! Something important to remember about Tomcat!!!!!!

It needs permissions to mess around in the DSpace directory. We don't have that directory yet. Maybe we will soon. But for now, we will remember these commands:

- [x] Either give Tomcat the necessary permissions or make it a user. ➕ 2024-05-01 ✅ 2024-05-08

```sh
# Change [dspace] and all subfolders to be owned by "tomcat"
chown -R tomcat:tomcat [dspace]
```

Here's another important note:

> - You need to ensure that Tomcat a) has enough memory to run DSpace, and b) uses UTF-8 as its default file encoding for international character support. So ensure in your startup scripts (etc) that the following environment variable is set: _JAVA_OPTS="-Xmx512M -Xms64M -Dfile.encoding=UTF-8"_

But wait! There's more!

> **Modifications in** **_[tomcat]/conf/server.xml_** : You also need to alter Tomcat's default configuration to support searching and browsing of multi-byte UTF-8 correctly. You need to add a configuration option to the _\<Connector\>_ element in _[tomcat]/config/server.xml_: _URIEncoding="UTF-8"_ e.g. if you're using the default Tomcat config, it should read:

```html
<!-- Define a non-SSL HTTP/1.1 Connector on port 8080 -->
<Connector port="8080"
              minSpareThreads="25"
              enableLookups="false"
              redirectPort="8443"
              connectionTimeout="20000"
              disableUploadTimeout="true"
              URIEncoding="UTF-8"/>
```

### 2024-04-30 3:28:19pm

```sh
sudo scp -r solr-8.11.3.tgz seyediana1@pedsdspace01.research.chop.edu:~
```

### 2024-04-30 3:23:58pm

This thing is broken. It's giving me this error?

```sh
[seyediana1@pedsdspace01 solr]$ solr start -p 8984
*** [WARN] *** Your open file limit is currently 1024.  
 It should be set to 65000 to avoid operational disruption. 
 If you no longer wish to see this warning, set SOLR_ULIMIT_CHECKS to false in your profile or solr.in.sh
*** [WARN] ***  Your Max Processes Limit is currently 62659. 
 It should be set to 65000 to avoid operational disruption. 
 If you no longer wish to see this warning, set SOLR_ULIMIT_CHECKS to false in your profile or solr.in.sh

ERROR: start.jar file not found in /opt/solr-8.11.3/solr/server!
Please check your -d parameter to set the correct Solr server directory.
```

But the original unzipped folder doesn't have it either? Also I downloaded it from the website and sent it before unzipping it? None of these tools work.

I think it's because I downloaded source and not binary. Not sure why I did that. I guess I didn't think it would make a difference.

### 2024-04-30 3:18:19pm

Okay. So Apache Solr only works if I include the `bin/` folder in `$PATH`. Otherwise it doesn't work for some reason. Who cares.

```sh
export PATH=/opt/solr-8.11.3/solr/bin:$PATH
```

### 2024-04-30 3:08:48pm

I had to install Apache Solr by downloading it locally and then `scp`ing the zip file over to the home directory:

```sh
sudo scp -r solr-8.11.3-src.tgz seyediana1@pedsdspace01.research.chop.edu:~
```

Uncompressing it there, and then sending it to `/opt`. This wasn't in the instructions, but I inferred that I had to add the base directory for Solr to my `$PATH`:

```sh
echo 'export PATH="/opt/solr-8.11.3/solr/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```

### 2024-04-30 2:06:20pm

Here's a little guide on how to connect to a fresh instance of PostgresQL:

After installing PostgreSQL on your Red Hat system, you can connect to the PostgreSQL database using the following steps:

1. Switch to the PostgreSQL user (default user is "postgres"):

   ```
   sudo -iu postgres
   ```

2. Connect to the PostgreSQL database using the `psql` command:

   ```
   psql
   ```

   This will open the PostgreSQL interactive terminal.

3. Once connected, you can interact with the database using SQL commands. For example, to list the available databases, you can use the `\l` command:

   ```
   \l
   ```

4. To connect to a specific database, use the `\c` command followed by the database name:

   ```
   \c database_name
   ```

   Replace `database_name` with the name of the database you want to connect to.

5. To exit the PostgreSQL interactive terminal, use the `\q` command:

   ```
   \q
   ```

Alternatively, you can also connect to the PostgreSQL database directly from the command line using the `psql` command with additional parameters:

```
psql -U username -d database_name
```

- Replace `username` with the PostgreSQL username you want to use (default is "postgres").
- Replace `database_name` with the name of the database you want to connect to.

If you are connecting to a PostgreSQL server running on a different host or port, you can specify the host and port using the `-h` and `-p` options, respectively:

```
psql -U username -d database_name -h host_address -p port_number
```

- Replace `host_address` with the IP address or hostname of the PostgreSQL server.
- Replace `port_number` with the port number on which the PostgreSQL server is running (default is 5432).

You will be prompted for the password associated with the specified username.

Remember to replace `username`, `database_name`, `host_address`, and `port_number` with the appropriate values based on your PostgreSQL installation and configuration.

### 2024-04-30 1:54:25pm

After installing Postgres, you need to edit the config file:

```sh
sudo nano /var/lib/pgsql/16/data/postgresql.conf
```

The instructions don't really mention this; I don't know why they make you have to do detective work.

Also I need to edit this file:

```sh
sudo nano /var/lib/pgsql/16/data/pg_hba.conf
```

I added this line:

```sh
host dspace dspace 127.0.0.1 255.255.255.255 md5
```

except I removed the space and added tabs between each field. Hope it works.

### 2024-04-30 1:43:08pm

Now I'm installed Postgres

```sh
sudo dnf install -y https://download.postgresql.org/pub/repos/yum/reporpms/EL-9-x86_64/pgdg-redhat-repo-latest.noarch.rpm
sudo dnf -qy module disable postgresql
sudo dnf install -y postgresql16-server
sudo /usr/pgsql-16/bin/postgresql-16-setup initdb
sudo systemctl enable postgresql-16
sudo systemctl start postgresql-16
```

### 2024-04-30 12:36:25pm

Installing Apache Ant was giving me a headache because I was encountering this error:

```sh
jspc:
[artifact:dependencies] Downloading: tomcat/jasper-compiler/4.1.36/jasper-compiler-4.1.36.pom from repository central at https://repo1.maven.org/maven2/
[artifact:dependencies] Unable to locate resource in repository
[artifact:dependencies] [INFO] Unable to find resource 'tomcat:jasper-compiler:pom:4.1.36' in repository central (https://repo1.maven.org/maven2/)
[artifact:dependencies] Downloading: tomcat/jasper-runtime/4.1.36/jasper-runtime-4.1.36.pom from repository central at https://repo1.maven.org/maven2/
[artifact:dependencies] Unable to locate resource in repository
[artifact:dependencies] [INFO] Unable to find resource 'tomcat:jasper-runtime:pom:4.1.36' in repository central (https://repo1.maven.org/maven2/)
```

A `pom` file does not exist for that version of `jasper-compile`. I had to go into `/opt/apache-ant-1.9.16/lib/libraries.properties` and change line 54 from version `4.1.36` to version `5.0.16`.

```html
# Later versions of Tomcat provide a jspc task
jasper-compiler.version=5.0.16
jasper-runtime.version=${jasper-compiler.version}
```

I also had to alter the NetRexx remote source from the IBM FTP server in lines 354-372 of `fetch.xml`:

```
  <target name="-fetch-netrexx" if="have.commons.net">
    <get src="https://www.netrexx.org/files/NetRexx-4.06-GA.zip"
       dest="${temp.dir}/NetRexx.zip" skipexisting="true"/>
  </target>

  <target name="-fetch-netrexx-no-commons-net" unless="have.commons.net">
    <get src="https://www.netrexx.org/files/NetRexx-4.06-GA.zip"
       dest="${temp.dir}/NetRexx.zip" skipexisting="true"/>
  </target>
```

I also had to get the SHA-256 hash of the zip file:

```sh
WJ7T2V3KGX:Downloads seyediana1$ shasum -a 256 NetRexx-4.06-GA.zip 
ba88c2291900097d0d377cbcb295e4fe9b696e84a125aa14e0594d7778e86568  NetRexx-4.06-GA.zip
```

I replaced the old hash with the new one on line 32 of `libraries.properties`:

```
# hashes of libraries loaded over insecure connections
netrexx.sha256=ba88c2291900097d0d377cbcb295e4fe9b696e84a125aa14e0594d7778e86568
```

Only then will the default command work:

```sh
ant -f /opt/apache-ant-1.9.16/fetch.xml -Ddest=system
```

By the way, this was all for [step 5 of the Apache Ant installation process](https://ant.apache.org/manual/index.html), which is for getting library dependencies. Who knows if I will ever need them.

### 2024-04-30 11:41:27am

Okay, let's set up Apache Ant:

```sh
[seyediana1@pedsdspace01 ~]$ ll /usr/lib/jvm/
total 0
lrwxrwxrwx. 1 root root  26 Apr 30 11:30 java -> /etc/alternatives/java_sdk
lrwxrwxrwx. 1 root root  32 Apr 30 11:30 java-1.8.0 -> /etc/alternatives/java_sdk_1.8.0
lrwxrwxrwx. 1 root root  40 Apr 30 11:30 java-1.8.0-openjdk -> /etc/alternatives/java_sdk_1.8.0_openjdk
drwxr-xr-x. 7 root root 135 Apr 30 11:30 java-1.8.0-openjdk-1.8.0.402.b06-2.el9.x86_64
lrwxrwxrwx. 1 root root  34 Apr 30 11:30 java-openjdk -> /etc/alternatives/java_sdk_openjdk
lrwxrwxrwx. 1 root root  21 Apr 30 10:48 jre -> /etc/alternatives/jre
lrwxrwxrwx. 1 root root  27 Apr 30 10:48 jre-1.8.0 -> /etc/alternatives/jre_1.8.0
lrwxrwxrwx. 1 root root  35 Apr 30 10:48 jre-1.8.0-openjdk -> /etc/alternatives/jre_1.8.0_openjdk
lrwxrwxrwx. 1 root root  49 Jan 12 15:40 jre-1.8.0-openjdk-1.8.0.402.b06-2.el9.x86_64 -> java-1.8.0-openjdk-1.8.0.402.b06-2.el9.x86_64/jre
lrwxrwxrwx. 1 root root  29 Apr 30 10:48 jre-openjdk -> /etc/alternatives/jre_openjdk
```

Just pick one that doesn't end in `jre`.

```sh
[seyediana1@pedsdspace01 ~]$ ls /etc/alternatives/java_sdk_1.8.0_openjdk
ASSEMBLY_EXCEPTION  bin  include  jre  lib  LICENSE  tapset  THIRD_PARTY_README
```

Put in `~/.bashrc`:

```sh
export PATH=$PATH:/opt/apache-maven-3.9.6/bin
export JAVA_HOME=/etc/alternatives/java_sdk_1.8.0_openjdk
export PATH=$JAVA_HOME/bin:$PATH
export ANT_HOME=/opt/apache-ant-1.9.16
export PATH=$PATH:$ANT_HOME/bin
```

### 2024-04-30 11:31:32am

I also apparently have to not only install `jdk` (which for some reason only gives me the `jre`) but also the `opendevel` version:

```sh
sudo yum install java-1.8.0-openjdk-devel
```

### 2024-04-30 10:48:18am

Installed `openjdk`:

```sh
sudo yum install java-1.8.0-openjdk
```

### 2024-04-30 10:23:56am - Additional Details from Alex

Got another message from Alex yesterday:

> 1. [https://pedsnet.github.io/Variable-Dictionary/](https://pedsnet.github.io/Variable-Dictionary/) Then can get to the actual repo from there [PEDSnet VARIABLE DICTIONARY](https://pedsnet.github.io/Variable-Dictionary/)
> 2. Don't worry about the actual data until the instance is up and running, from there we can meet with Anabel and team to understand what goes into it.
> 3. the PCORnet one, but we need both up and running, improvements and ui redesign will be mostly for PCORnet and can use that knowledge for our internal as we see it being needed

So I think the first step will be to just follow the DSpace documentation and follow the directions.

### 2024-04-29 4:51:56pm

Okay. We're starting now. Seems like a gargantuan task.

