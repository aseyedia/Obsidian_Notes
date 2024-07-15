---
share: true
editor-width: 
aliases: []
status: 
date created: Tuesday, April 23rd 2024, 11:54:05 am
date modified: Monday, July 15th 2024, 2:55:45 pm
tags:
  - PCORnet_Projects_DSpace
  - PCORnet_Projects_DSpace_PEDSpace
priority: 🚨 - High
project: DSpace
tech: 
---

> [!danger] Incomplete Tasks Related to Upgrading
> ```tasks
> path includes {{query.file.path}}
> heading includes Upgrading
> group by tags
 > hide backlink
 > not done
 > ```

> [!todo] Incomplete Tasks for Customization and Configuration
>
> ```tasks
> path includes {{query.file.path}}
> heading does not include Issues
> heading does not include Upgrading
> hide backlink
> not done
> ```
>
> > [!caution] Issues
> >
> > ```tasks
> > path includes {{query.file.path}}
> > heading includes Issues
> > hide backlink
> > not done
> > ```

> [!success]- Completed Tasks - All
> ```tasks
> path includes {{query.file.path}}
> # heading does not include Issues
> hide backlink
> done
> ```

## Background

This page is for both of the DSpace projects.

## Resources

- [Internal DSpace Re-skin Project](../../Internal%20DSpace%20Re-skin%20Project.md)
- [External DSpace Re-Skin (PCORnet)](../../External%20DSpace%20Re-Skin%20(PCORnet).md)
	- [CGPT Thread](https://chatgpt.com/c/c7819040-97c8-41dd-b0fa-5b65bf4744cd)
- ~~[Metadata Item Mapping](../../Metadata%20Item%20Mapping.md)~~

### Upgrade Links

- [DSpace 8.0 Release Notes](https://wiki.lyrasis.org/display/DSDOC8x/Release+Notes#ReleaseNotes-8.0ReleaseNotes)
- [Upgrading DSpace (frontend and backend) to 8.0 Documentation](https://wiki.lyrasis.org/display/DSDOC8x/Upgrading+DSpace)
- [CGPT Thread (Same as External DSpace Re-Skin)](https://chatgpt.com/c/c7819040-97c8-41dd-b0fa-5b65bf4744cd)
- [DSpace 7.6 Install Docs (for reference)](https://wiki.lyrasis.org/display/DSDOC7x/Installing+DSpace)
- [DSpace 8.0 Install Docs (for reference)](https://wiki.lyrasis.org/display/DSDOC8x/Installing+DSpace)

## Additional Notes

### Issues

- [x] Solve the issue with `webui.browse.index` and `webui.browse.link` ➕ 2024-07-02 ✅ 2024-07-03
- [ ] Look into why "version" information won't show up ➕ 2024-07-02
- [ ] Figure out how to return search queries that sort by "item created" by default. ➕ 2024-07-02

### Upgrading

- [x] Tarball both the frontend and backend for BOTH instances ➕ 2024-07-02 ✅ 2024-07-03
- [x] Tell Alex to snapshot both instances ⏫ ➕ 2024-07-02 ✅ 2024-07-03
- [ ] Make sure that, when upgrading PEDSpace, I move the source code in `/opt` to `/data`, which is part of a much larger filesystem. #PCORnet/Projects/DSpace/PEDSpace ➕ 2024-07-03
- [ ] When upgrading PEDSpace, remember that it's a RedHat system, so Postgres configurations are in `/var/lib/pgsql/`. Might have to migrate to `/data`. #PCORnet/Projects/DSpace/PEDSpace ➕ 2024-07-03

## Log

### 2024-07-15 12:18:33pm

I've been going insane because I can't see relationships because it keeps saying I need a valid `dspace.entity.type` for each item, even though it had that, [and now turns out they're just not dang enabled.](https://wiki.lyrasis.org/display/DSDOC8x/Configurable+Entities#:~:text=a%20tree%20structure.-,Default%20Entity%20Models,Each%20org%20units%20can%20link%20to%20projects%2C%20people%20and%20publications,-Journals)

- [ ] Configure/Initialize [Entities](https://wiki.lyrasis.org/display/DSDOC8x/Configurable+Entities) ➕ 2024-07-15

### 2024-07-15 12:16:08pm

Remember this:

```sh
psql -U dspace -d dspace -h localhost
```

### 2024-07-15 11:45:52am

Okay I'm pretty much done with the merge. and upgrading. tying up some loose ends but other than that I'm pretty much done with DSpace.

### 2024-07-10 4:38:46pm

I just realized that one of the reasons it's probably not working is that I need to update all the `.ts` files, so I had the script overwrite them for me.

### 2024-07-10 4:14:26pm

`yarn ng generate @angular/core:standalone --path src/theme/PCORnet-custom-7/`, after all this time, will only update one file. Just one. Why? I don't know.

I'm going to try `yarn lint-fix` again this time. Let's see if it works.

### 2024-07-10 3:47:44pm

I updated it to include subdirectories:

```sh
#!/bin/bash

# Default source and destination directories
src_dir="/opt/dspace-angular-dspace-8.0/src/themes/custom/app"
dst_dir="/opt/dspace-angular-dspace-8.0/src/themes/PCORnet-custom-7/app"

# Default options
verbose=false
update_existing=false

# Function to display usage information
usage() {
    echo "Usage: $0 [-s source_dir] [-d destination_dir] [-v] [-u]"
    echo "  -s: Source directory (default: $src_dir)"
    echo "  -d: Destination directory (default: $dst_dir)"
    echo "  -v: Verbose mode"
    echo "  -u: Update existing files if source is newer"
    exit 1
}

# Parse command-line options
while getopts ":s:d:vu" opt; do
    case $opt in
        s) src_dir="$OPTARG" ;;
        d) dst_dir="$OPTARG" ;;
        v) verbose=true ;;
        u) update_existing=true ;;
        \?) echo "Invalid option -$OPTARG" >&2; usage ;;
    esac
done

# Function to copy missing directories and files
copy_missing() {
    local src="$1"
    local dst="$2"
    
    # Loop through each item in the source directory
    for item in "$src"/*; do
        # Get the base name of the item
        local base_item=$(basename "$item")
        
        # If the item is a directory
        if [ -d "$item" ]; then
            # Create the directory in the destination if it does not exist
            if [ ! -d "$dst/$base_item" ]; then
                mkdir -p "$dst/$base_item"
                $verbose && echo "Created directory $dst/$base_item"
            fi
            # Recursively copy missing subdirectories and files
            copy_missing "$item" "$dst/$base_item"
        # If the item is a file
        elif [ -f "$item" ]; then
            # Copy the file if it does not exist in the destination
            if [ ! -f "$dst/$base_item" ]; then
                cp "$item" "$dst"
                $verbose && echo "Copied file $base_item to $dst"
            elif $update_existing && [ "$item" -nt "$dst/$base_item" ]; then
                cp "$item" "$dst"
                $verbose && echo "Updated file $base_item in $dst"
            fi
        fi
    done
}

# Check if source directory exists
if [ ! -d "$src_dir" ]; then
    echo "Error: Source directory $src_dir does not exist." >&2
    exit 1
fi

# Check if destination directory exists, create if it doesn't
if [ ! -d "$dst_dir" ]; then
    mkdir -p "$dst_dir"
    echo "Created destination directory $dst_dir"
fi

# Start the copying process
copy_missing "$src_dir" "$dst_dir"

echo "Synchronization complete."
```

### 2024-07-10 3:34:30pm

I'm copying over the eager theme and lazy theme whatever modules. In order to do that, I have to make sure the components that they're referencing exist:

```sh
#!/bin/bash

# Source and destination directories
src_dir="/opt/dspace-angular-dspace-8.0/src/themes/custom/app"
dst_dir="/opt/dspace-angular-dspace-8.0/src/themes/PCORnet-custom-7/app"

# Loop through each folder in the source directory
for dir in "$src_dir"/*; do
  # Get the base name of the directory
  base_dir=$(basename "$dir")
  
  # Check if the directory does not exist in the destination
  if [ ! -d "$dst_dir/$base_dir" ]; then
    # Copy the directory to the destination
    cp -r "$dir" "$dst_dir"
    echo "Copied $base_dir to $dst_dir"
  else
    echo "$base_dir already exists in $dst_dir"
  fi
done
```

### 2024-07-10 1:42:10pm

Okay.

```sh
ubuntu@ip-172-22-32-9:/opt/dspace-angular-dspace-8.0$ yarn lint
yarn run v1.22.22
$ yarn build:lint && yarn lint:nobuild
$ rimraf 'lint/dist/**/*.js' 'lint/dist/**/*.js.map' && tsc -b lint/tsconfig.json
$ ng lint

Linting "dspace-angular"…
```

I'm linting this massive bastard now. I probably should have specified a directory. But Idk.

### 2024-07-10 1:27:14pm

Okay it took me more than a few hours but I finally fixed it. Everything is finally normal. We are back.

### 2024-07-10 12:57:37pm

Oh my gosh. I just realized a major issue. Because I was merging staight from HEAD in `origin/main`, I was getting the early development version of DSpace 9. That's why the homepage said DSpace 9. Now that I'm trying to "restore" DSpace, I am using the pre-packaged installation version, and THAT is actually DSpace 8. So I am doing the right thing anyway. This goose chase helped me realize that I was trying to install and deploy an extremely early version of DSpace 9.

![Pasted image 20240710130115.png](../assets/img/Pasted%20image%2020240710130115.png)

### 2024-07-10 12:00:24pm

I'm deeper in the whole. I tried to fix it with `yarn` and going to an earlier commit and it didn't work. At this point, it is becoming monumentally evident that just downloading a clean frontend and starting over from scratch is preferable.

### 2024-07-10 11:13:48am

This whole thing has been a brutal nightmare. My dumbass went in a circle because I basically got confused and deleted all my `node_modules` and then reinstalled them… etc. Before I remembered that my packages are managed by `yarn` and not `npm`.

So anyway. I think I finally am back to normal? Maybe?

`/Applications/Plover.app/Contents/MacOS/Plover -s plover_plugins install plover-lapwing-aio`

### 2024-07-10 10:27:35am

Okay well the script ran but now it's all errors. This is kind of a pain in the ass. This whole upgrade process. And the frontend has been worse than the backend.

> 1. Fix any errors in all "<…>.module.ts" files in your theme folder. These errors may be mostly caused by importing old modules that are now deleted: simply delete any lines that refer to such modules;

They're just saying this all willy-nilly like it's a walk in the park.

### 2024-07-10 10:02:37am

So here's where I left things off yesterday:

![2024-07-09-Tuesday > End of Day Review](../../Work%20Daily%20Log.md#End%20of%20Day%20Review)

[And this is because my dumbass just naively installed `ng-common` when Ubuntu asked me if that's what I meant, not `angular/cli`](https://stackoverflow.com/questions/45145535/uninstall-the-mg-editor-and-use-angular-cli-properly):

```sh
sudo apt remove mg ng-common
```

```sh
npm install -g @angular/cli
```

```sh
ubuntu@ip-172-22-32-9:/opt/dspace-angular-dspace-latest$ ng generate @angular/core:standalone --path src/themes/PCORnet-custom
? Would you like to enable autocompletion? This will set up your terminal so pressing TAB while typing Angular CLI
 commands will show possible options and autocomplete arguments. (Enabling autocompletion will modify 
configuration files in your home directory.) Yes
Appended `source <(ng completion script)` to `/home/ubuntu/.bashrc`. Restart your terminal or run the following to autocomplete `ng` commands:

    source <(ng completion script)
? Choose the type of migration: (Use arrow keys)
❯ Convert all components, directives and pipes to standalone 
  Remove unnecessary NgModule classes 
  Bootstrap the application using standalone APIs
```

It's working now.

### 2024-07-09 3:05:42pm

Okay so it looks like it doesn't like it when my old `PCORnet-custom` folder is just chillin' in the damn themes directory. So kind of a big setback since I've customized so much stuff so far but whatever.

### 2024-07-09 1:32:53pm

- [ ] Tweak and perfect PEDSpace README ➕ 2024-07-09
- [ ] Tweak and perfect PCORnet README ➕ 2024-07-09

### 2024-07-09 12:32:30pm

Okay so all I had to do was actually call `yarn install` first before `yarn start:dev`. But it does actually make sense to just `git merge` the upgrade.

### 2024-07-09 11:49:50am

I'll explain later.

```sh
angular.json
config/.gitignore
config/config.yml
src/app/item-page/simple/field-components/specific-field/abstract/item-page-abstract-field.component.ts
src/app/item-page/simple/field-components/specific-field/date/item-page-date-field.component.ts
src/app/item-page/simple/field-components/specific-field/item-page-field.component.ts
src/app/shared/shared.module.ts
src/assets/i18n/en.json5
src/index.html
src/themes/dspace/app/home-page/home-news/home-news.component.html
src/themes/eager-themes.module.ts
```

```
y - stage this hunk
n - do not stage this hunk
q - quit; do not stage this hunk or any of the remaining ones
a - stage this hunk and all later hunks in the file
d - do not stage this hunk or any of the later hunks in the file
g - select a hunk to go to
/ - search for a hunk matching the given regex
j - leave this hunk undecided, see next undecided hunk
J - leave this hunk undecided, see next hunk
k - leave this hunk undecided, see previous undecided hunk
K - leave this hunk undecided, see previous hunk
s - split the current hunk into smaller hunks
e - manually edit the current hunk
p - print the current hunk
? - print help
```

### 2024-07-09 10:21:59am

Meeting:

- [x] A way to track changes on DSpace ⏫ ➕ 2024-07-09 ✅ 2024-07-09
	- Tracking via GitHub
	- A way to display what has been going on and will be happening etc.
	- Use the READMEs
	- These are two separate grants

Okay. So we just had a meeting wherein I learned that I should create two different READMEs for both of these DSpace initiatives for different "interested parties" on GitHub, and I should probably figure out a way to maybe maintain a log at least semi-regularly? I don't think it's that serious; probably Annabel can manage it via GH issues/projects, but I at least need to set up the READMEs. Maybe once a week I can update them?

### 2024-07-09 9:48:41am

Okay. We are getting there.

### 2024-07-08 4:42:55pm

```
Files preferring 'theirs': 3835
Files needing conflict resolution: 28
```

### 2024-07-08 4:03:07pm

```sh
git ls-tree -r --name-only $(git rev-list --max-parents=0 HEAD)
```

All files from initial commit.

### 2024-07-08 3:23:48pm

Okay I'm backing up Tomcat and I'm going to get rid of it.

### 2024-07-08 3:15:54pm

Okay I thought I finished and then I realized that Tomcat is old (9) and I should just run the bundled Tomcat 10 bundled with Java.

### 2024-07-08 12:14:41pm

**Arta Seyedian (1 minute ago)**:
DSpace 7.x installation only [generates 4 empty Solr cores](https://wiki.lyrasis.org/display/DSDOC7x/Installing+DSpace#InstallingDSpace-ApacheSolr8.x(full-textindex/searchservice):~:text=Copy%20Solr%20cores%3A%C2%A0%20DSpace%20installation%20creates%20a%20set%20of%20four%20empty%20Solr%20cores%20already%20configured.%C2%A0). Those were the cores that I copied and pasted into my Solr `configsets` directory: the ones I had already pasted in there. For some reason, that caused Solr to break, potentially related to `write.lock` files.

DSpace 8.x installation [generates 6 empty Solr cores](https://wiki.lyrasis.org/display/DSDOC8x/Installing+DSpace#InstallingDSpace-ApacheSolr8.x(full-textindex/searchservice):~:text=Copy%20Solr%20cores%3A%C2%A0%20DSpace%20installation%20creates%20a%20set%20of%20six%20empty%20Solr%20cores%20already%20configured.%C2%A0). However, because I am upgrading, those Solr cores never end up in `[dspace]`; they remain in `[dspace-source]`. For some reason, copying those empty Solr cores into Solr also breaks things.

I also just realized the source of this problem, which is that the DSpace installation directory parameter in source config file was pointing to the wrong location `/dspace` by default, instead of where my files actually were, which is `/opt/dspace`. So, be wary of this problem.

Either drop in your `local.cfg` from before or remember to tweak `dspace.cfg`. (edited)

### 2024-07-08 12:07:35pm

**Arta (11:33 AM)**:
I'm starting to wonder if it would be easier at this point to just do a fresh install of DSpace 8 than use any of their "upgrade" instructions, because either the instructions aren't complete or the built-in upgrading process in DSpace just doesn't work.

Actually I know their instructions aren't complete, but also now I'm beginning to suspect that their upgrade mechanism also isn't complete.

**Alex Shorrocka (11:44 AM)**:
Gotcha, yeah if that's the path you want to take then I don't think its a bad one especially if you are having lots of issues with upgrading , all the UI changes you made should be able to be dropped over, the dbs as well should operate the same so hopefully that wouldn't require any changes.

**Arta (11:49 AM)**:
Yeah it's just nothing about the upgrade process works the way you would expect it to

For example, I spent hours struggling with the new DSpace-Solr instructions only to find out that the upgrade a) didn't bring new empty Solr cores into the DSpace installation directory and b) I had to add some lines to the new additional cores to make the compatible with Solr 9.5.0, which wasn't in the instructions. Now I am struggling with the re-indexing and the reason is a little too much to get into but it boils down to the upgrade process not actually upgrading the DSpace installation.

and I don't want this whole thing to day any more than a week max, preferably for both. Given that we have barely used DSpace for anything so far, I think it's okay to just start over with a new installation, and in the future, maybe we can wait longer to upgrade.

Anyway. I'm just complaining at this point but all of this is to say that upgrading is not as simple and straightforward as one would hope.

**Arta (12:06 PM)**:
okay false alarm. Thanks for letting me complain. looks like when i built and ran DSpace from source last week, the config had the installation directory set to `/dspace` instead of `/opt/dspace` , which only occurred to me after I sent those messages. It's actually working now. thanks for listening.

### 2024-07-08 11:31:15am

I pretty much feel like the upgrade process straight up doesn't work.

Here's why I say this: I followed the upgrade instructions only to find out just now that `dspace.cfg` was never updated. It should have been, presumably. But it wasn't.

### 2024-07-08 11:24:44am

Never mind, I forgot that I override it in `local.cfg`.

### 2024-07-08 11:13:41am

Okay I am rebuilding with `mvn -U -X clean package` because I am not sure but I think there is a bad path for the DSpace install directory in `dspace.cfg`… there are files in `/dspace` when it should be `/opt/dspace`.

### 2024-07-08 11:07:37am

Okay `dspace.cfg` looked jacked up. I guess I accidentally updated the source config file and not the deployed one.

### 2024-07-08 10:50:50am

It's like every stop has an issue that I have to do mortal combat with.

```sh
ubuntu@ip-172-22-32-9:/opt/dspace$ ./bin/dspace index-discovery -b
usage: index-discovery
 -b,--build          (re)build index, wiping out current one if it exists
 -c,--clean          clean existing index removing any documents that no
                     longer exist in the db
 -d,--delete         delete all records from existing index
 -f,--force          if updating existing index, force each handle to be
                     reindexed even if uptodate
 -h,--help           print this help message
 -i,--index <arg>    add or update an Item, Collection or Community based
                     on its handle or uuid
 -r,--remove <arg>   remove an Item, Collection or Community from index
                     based on its handle
 -s,--spellchecker   Rebuild the spellchecker, can be combined with -b and
                     -f.
org.apache.commons.cli.ParseException: Unable to create a new DSpace Context: Cannot invoke "org.dspace.core.DBConnection.setConnectionMode(boolean, boolean)" because "this.dbConnection" is null
        at org.dspace.discovery.IndexClient.setup(IndexClient.java:163)
        at org.dspace.scripts.DSpaceRunnable.parse(DSpaceRunnable.java:122)
        at org.dspace.scripts.DSpaceRunnable.initialize(DSpaceRunnable.java:95)
        at org.dspace.app.launcher.ScriptLauncher.executeScript(ScriptLauncher.java:149)
        at org.dspace.app.launcher.ScriptLauncher.handleScript(ScriptLauncher.java:132)
        at org.dspace.app.launcher.ScriptLauncher.main(ScriptLauncher.java:99)
```

Why is `this.dbConnection` null. Why is `org.dspace.core.DBConnection` null.

`./bin/dspace database info` only returns successes. `./bin/dspace database migrate` returns no errors.

```sh
ubuntu@ip-172-22-32-9:/opt/dspace$ ./bin/dspace database test

Attempting to connect to database
Connected successfully!

Database Type: postgres
Database URL: jdbc:postgresql://localhost:5432/dspace
Database Schema: public
Database Username: dspace
Database Software: PostgreSQL version 16.2 (Ubuntu 16.2-1.pgdg22.04+1)
Database Driver: PostgreSQL JDBC Driver version 42.6.0
PostgreSQL 'pgcrypto' extension installed/up-to-date? true (version=1.3)
FlywayDB Version: 8.4.4
```

```sh
ubuntu@ip-172-22-32-9:/opt/dspace$ ./bin/dspace database validate

Database URL: jdbc:postgresql://localhost:5432/dspace
Attempting to validate database status (and migration checksums) via FlywayDB...
No errors thrown. Validation succeeded. (Check dspace logs for more details)
```

### 2024-07-08 10:14:29am

Hello. I'm back. Here is what I spent hours on last Friday:

**Arta Seyedian (July 5, 3 days ago)**:
Hello again,
I am upgrading DSpace 7.6 to 8.0 as I mentioned before, and I am at the step where I am copying the Solr cores over from the updated DSpace backend installation (`[dspace]`) over to the Solr directory… except, in `[dspace]`, the cores in `[dspace]/solr/*` have the same timestamps as when I first installed DSpace 7.6, and when I copy them over to `[solr]/`, the Solr Search core no longer works.
```shell
ubuntu@ip-172-22-32-9:/opt$ ll -h /opt/dspace/solr/  
total 24K  
drwxrwxr-x  6 ubuntu ubuntu 4.0K Apr 12 15:50 ./  
drwxr-xr-x 18 ubuntu ubuntu 4.0K Apr 13 01:15 ../  
drwxrwxr-x  3 ubuntu ubuntu 4.0K Apr 12 15:50 authority/  
drwxrwxr-x  3 ubuntu ubuntu 4.0K Apr 12 15:50 oai/  
drwxrwxr-x  3 ubuntu ubuntu 4.0K Apr 12 15:50 search/  
drwxrwxr-x  3 ubuntu ubuntu 4.0K Apr 12 15:50 statistics/  
```  
I followed the directions precisely, and skipped the sections that were not relevant to my installation (upgrading from 6.x or 5.x). The funny thing is that copying the Solr cores over from the `[dspace-source]` does yield a functioning Solr instance with an apparently successful `http://localhost:8983/solr/search/select` result:
```json
{
  "responseHeader":{
    "status":0,
    "QTime":20,
    "params":{ }
  },
  "response":{
    "numFound":0,
    "start":0,
    "numFoundExact":true,
    "docs":[ ]
  }
} 
```  
But then do these empty cores also need to be in the `[dspace]` directory? This is all with a yet-to-be deployed dev instance so we're not really concerned if our Solr stats get wiped out, but nonetheless, I am a bit confused at this point and just want to make sure I'm being thorough in this upgrade process. (edited)

**Arta Seyedian (July 5, 3 days ago)**:
I tried reverting my changes, btw, to a configsets/ folder from before I started the upgrade process, and now it seems like my Solr installation might be toast. Not sure what I did wrong, but I do know that copying the cores in `[dspace]/solr` into my Solr installation didn't work. Maybe I didn't stop Solr before attempting this? (edited)

**Arta Seyedian (July 5, 3 days ago)**:
Okay. I think I found the problem.
DSpace 7.x installation only generates 4 empty Solr cores. Those were the cores that I copied and pasted into my Solr configsets directory: the ones I had already pasted in there. For some reason, that caused Solr to break, potentially related to `write.lock` files.
DSpace 8.x installation generates 6 empty Solr cores. However, because I am upgrading, those Solr cores never end up in `[dspace]`; they remain in `[dspace-source]`. For some reason, copying those empty Solr cores into Solr also breaks things.

**Arta Seyedian (July 5, 3 days ago)**:
The Upgrading DSpace instructions don't mention this, which has led to my confusion.

**Arta Seyedian (July 5, 3 days ago)**:
So I am going to attempt to restore everything back to the various backups I had made :crossed_fingers: and try again.

**Arta Seyedian (July 5, 3 days ago)**:
Okay so the issue with the Solr search core is answered in the installation instructions here. Unfortunately now, I am facing the issue where Solr won't appear at all, search core or not.

**Arta Seyedian (July 5, 3 days ago)**:
Finally figured it out.
First of all, I called `./bin/solr start -f` to be able to carefully read the out/err. One major issue that I had was that in trying to replace my deployed `configsets/` folder with my backup `configsets/` folder, I ended up accidentally moving `configsets/` INSIDE of `configsets/`, which I guess confused Solr, because it searches recursively in its directories to find all cores and can't tolerate duplicate cores.
Secondly, I am using Solr 9.5.0. I am taking over someone else who set up this DSpace instance, so I'm not sure why they used Solr 9.x even though the instructions explicitly say not to, but they took care to include these lines in `search/conf/solrconfig.xml`. However, the two new Solr cores for DSpace 8.x, `qaevent/` and `suggestion/` also need to have the Solr library path specified in their respective `solrconfig.xml` files.
Thank you for reading, I hope this ends up helping someone eventually. (edited)

### 2024-07-05 5:44:11pm

Finally figured it out.

First of all, I called `./bin/solr start -f` to be able to carefully read the out/err. One major issue that I had was that in trying to replace my deployed `configsets/` folder with my backup `configsets/` folder, I ended up accidentally moving `configsets/` INSIDE of `configsets/`, which I guess confused Solr, because it can't tolerate duplicate cores.

Secondly, I am using Solr 9.5.0. I am taking over someone else who set up this DSpace instance, so I'm not sure why the used Solr 9.x even though the instructions explicitly say not to, but they took care to include these lines in `search/conf/solrconfig.xml`. However, the two new Solr cores for DSpace 8.x, `qaevent/` and `suggestion/` also need to have the Solr library path specified in their respective `solrconfig.xml` files.

Thank you for reading, I hope this ends up helping someone eventually.

### 2024-07-05 5:20:41pm

Okay.

First of all, you need to call the following command to get debug information.

```sh
./bin/solr start -f
```

Secondly: don't put a `configset/` folder within your `configsite/` folder.

### 2024-07-05 4:55:33pm

Okay so the issue with the Solr search core is answered in the [installation instructions here](https://wiki.lyrasis.org/display/DSDOC7x/Installing+DSpace?focusedCommentId=246284451#comment-246284451). Unfortunately now, I am facing the issue where Solr won't appear at all, search core or not.

### 2024-07-05 4:34:25pm

#### Summary

I am upgrading DSpace 7.6 to 8.0, and I encountered issues with the Solr cores. Here's the detailed account of what happened:

#### Issues Encountered

1. **Timestamp Discrepancy:**
   - The cores in `[dspace]/solr/*` have the same timestamps as when Alex first installed DSpace 7.6.
   - Copying these over to `[solr]` caused the Solr Search core to stop working.

2. **Empty Cores:**
   - DSpace 8.x generates 6 empty Solr cores.
   - These cores remain in `[dspace-source]` and don't get moved to `[dspace]` during the upgrade.

3. **Reverting Changes:**
   - Tried reverting changes to a `configsets/` folder from before the upgrade process.
   - This led to Solr showing a strange page saying "Searching for Solr? You must type the correct path. Solr will respond."

4. **Permissions Issues:**
   - Realized permissions issues were causing problems when copying directories.
   - Corrected permissions but still faced issues with Solr cores not working.

5. **Restoring Solr Backup:**
   - Restored from `configsets.bak`.
   - This made Solr work again, but I am unsure of what exactly went wrong during the process.

#### Slack Message Sent

Hello again,
I am upgrading DSpace 7.6 to 8.0 as I mentioned before, and I am at the step where I am copying the solr cores over from the updated DSpace backend installation ([dspace]) over to the solr directory… except, in [dspace], the cores in [dspace]/solr/* have the same timestamps as when I first installed DSpace 7.6, and when I copy them over to `[solr]/`, the Solr Search core no longer works.

```
ubuntu@ip-172-22-32-9:/opt$ ll -h /opt/dspace/solr/
total 24K
drwxrwxr-x 6 ubuntu ubuntu 4.0K Apr 12 15:50 ./
drwxr-xr-x 18 ubuntu ubuntu 4.0K Apr 13 01:15 ../
drwxrwxr-x 3 ubuntu ubuntu 4.0K Apr 12 15:50 authority/
drwxrwxr-x 3 ubuntu ubuntu 4.0K Apr 12 15:50 oai/
drwxrwxr-x 3 ubuntu ubuntu 4.0K Apr 12 15:50 search/
drwxrwxr-x 3 ubuntu ubuntu 4.0K Apr 12 15:50 statistics/
```

I followed the directions precisely, and skipped the sections that were not relevant to my installation (upgrading from 6.x or 5.x). The funny thing is that copying the Solr cores over from the [dspace-source] does yield a functioning Solr instance with an apparently successful <http://localhost:8983/solr/search/select> result:

```json
{
  "responseHeader":{
    "status":0,
    "QTime":20,
    "params":{ }
  },
  "response":{
    "numFound":0,
    "start":0,
    "numFoundExact":true,
    "docs":[ ]
  }
}
```

But then do these empty cores also need to be in the [dspace] directory? This is all with a yet-to-be deployed dev instance so we're not really concerned if our Solr stats get wiped out, but nonetheless I am a bit confused at this point and just want to make sure I'm being thorough in this upgrade process.
I tried reverting my changes, btw, to a configsets/ folder from before I started the upgrade process, and now it seems like my Solr installation might be toast. Not sure what I did wrong, but I do know that copying the cores in [dspace]/solr into my Solr installation didn't work. Maybe I didn't stop Solr before attempting this?

Okay. I think I found the problem.

DSpace 7.x installation only generates 4 empty Solr cores. Those were the cores that I copied and pasted into my Solr configsets directory: the ones I had already pasted in there. For some reason, that caused Solr to break, potentially related to `write.lock` files.

DSpace 8.x installation generates 6 empty Solr cores. However, because I am upgrading, those Solr cores never end up in [dspace]; they remain in [dspace-source]. For some reason, copying those empty Solr cores into Solr also breaks things.

The Upgrading DSpace instructions don't mention this, which has led to my confusion. So I am going to attempt to restore everything back to the various backups I had made 🤞 and try again.

### 2024-07-05 2:15:23pm

The Solr step is not working.

> [!note]
> Here is how to copy to backup cores over to the running Solr instance:
>
> ```sh
> sudo cp -rT /opt/backups/solr-9.5.0/server/solr/configsets /opt/solr-9.5.0/server/solr/configsets
> sudo chown -R ubuntu:ubuntu /opt/solr-9.5.0/server/solr/configsets
> ```

### 2024-07-05 12:20:31pm

The way this setup works, it upgrades the dspace backend code IN PLACE, so while it says shit like this:

> 1. _Make sure your existing local.cfg is in the source directory_ (e.g. `[dspace-source]/dspace/config/local.cfg`).  That way your existing configuration gets reinstalled alongside the new version of DSpace.

If you made any intermediary changes to the config, you need to deploy them to the backend, because, for example, the Postgres connection won't work if you decided to go with a new password.

### 2024-07-05 12:20:27pm

`$(date +%Y-%m-%d)`

### 2024-07-03 5:22:46pm

I'm just making a note to say that I finished building the DSpace backend.

```sh
mvn -U -X clean package
```

I'm calling it for today.

### 2024-07-03 3:52:12pm

Okay. I finally need to move on.

I'm going to also backup `/opt/solr`.

```sh
sudo tar -czvf solr-backup-$(date +%Y-%m-%d).tar.gz /opt/solr-9.5.0/
```

### 2024-07-03 3:24:50pm

Okay it took me forever to do the first step only to find out that I needed the `-h localhost` flag:

```sh
pg_dump -U dspace -h localhost -f /opt/backups/postgres-backup-$(date +%Y-%m-%d).sql dspace
```

- **`-h localhost`**: Connects to the database via TCP/IP on localhost, avoiding peer authentication issues with Unix sockets.

So apparently my problem was that Postgres was trying to authenticate me via Unix socket shell, which means that it was just taking my Unix username and trying to match it up against the same username in its system, which, of course, wasn't working, because `ubuntu` is not a Postgres user. `-h localhost` instructions Postgres to connect over TCP/IP, and that connection is to be made locally i.e. with the same machine calling the command.

The authentication worked because of the configuration in the `pg_hba.conf` file, which controls the authentication methods for different types of connections. By default, it allows TCP/IP connections from localhost using scram-sha-256 or md5 authentication:

```
# IPv4 local connections:
host    all             all             127.0.0.1/32            scram-sha-256
# IPv6 local connections:
host    all             all             ::1/128                 scram-sha-256
```

Inserting a line like this also ensures the `dspace` user can connect from localhost:

```
host    dspace          dspace          127.0.0.1/32            md5
```

Uncommenting `listen_addresses` in the `postgresql.conf` file ensured that Postgres was listening for TCP/IP connections on localhost, which is required for this connection method to work properly.

### 2024-07-03 2:01:01pm

Alright. Well. Most of the day is gone. That's okay, I guess. Not sure what happened to the day or what I really spent my time doing. But it's all good.

Also I just found out that [they're intending on making PEDSpace public](https://pedsnet.slack.com/archives/C055UAV1W31/p1720025408473779):

![Pasted image 20240703140418.png](../assets/img/Pasted%20image%2020240703140418.png)

Which, lol, it would have been nice if they told me beforehand. God.

### 2024-07-03 1:48:02pm

Alright I fixed it. Turns out my ~~dumbass~~ genius mind copied the cfg files from PCORnet's DSpace.

### 2024-07-03 12:51:25pm

Now PEDSpace is down and I have no clue why. So I have to look into that.

### 2024-07-03 12:20:23pm

Here are tarball commands:

```sh
sudo tar -czvf dspace-angular-frontend-backup-$(date +%Y-%m-%d).tar.gz /opt/dspace-angular-dspace-7.6.1
sudo tar -czvf dspace-deployed-backend-backup-$(date +%Y-%m-%d).tar.gz /opt/dspace
```

### 2024-07-03 12:03:17pm

I am procrastinating on this so hard because a) I am tired and keep having weird blood sugar yo-yos in the middle of the night that wake me up and I can't go back to sleep until I eat and my body digests the food but also b) it is murdering me how much work this is beginning to look like it's going to be.

But it's okay. I have this song on my side:

<iframe style="border-radius:12px" src="https://open.spotify.com/embed/track/4neRBrzDQE74psj802v2mr?utm_source=generator" width="40%" height="152" frameBorder="0" allowfullscreen="" allow="autoplay; clipboard-write; encrypted-media; fullscreen; picture-in-picture" loading="lazy"></iframe>

It's a good song. It makes me feel good. And I am listening to it on repeat. Good job to me. I am one of the privileged few.

### 2024-07-03 11:45:06am

Okay. I have to start upgrading to 8.0 now. Damn it. Should I make a new project log for upgrading? I think I should. Nah it's okay.

### 2024-07-03 11:22:02am

Okay that worked.

Changing

```sh
# webui.browse.index.5 = type:metadata:dc.type:text
# webui.browse.index.6 = relation:metadata:dc.relation:text
webui.browse.index.5 = type:metadata:dc.type:valueList
webui.browse.index.6 = relation:metadata:dc.relation:valueList
```

made the link go straight to browsing items which share that metadata field value. Great!

One thing I'm not thrilled about is that the relation links don't take me straight to the file, it takes me to a browse page for the file:

![Pasted image 20240703114037.png](../assets/img/Pasted%20image%2020240703114037.png)

But honestly I just have to make do with what I got. 👍

### 2024-07-03 11:08:54am

![2024-07-03-Wednesday > 2024-07-03 10 42 23am](../../Work%20Daily%20Log.md#2024-07-03%2010%2042%2023am)

Look at the part where someone responded to my post in the Slack. I'm going to try it now.

### 2024-07-02 4:14:01pm

I just finished the simple item view. I spent a while on my phone to I guess mentally reset.

There are a few things I should probably touch on.

![Metadata Item Mapping > Element Table](../../Metadata%20Item%20Mapping.md#Element%20Table)

Do you see this table? There are a few issues:

\1. `webui.browse.index`

This is driving me up the wall. I'm trying to make it so that some of these "searchable metadata fields" (I'm talking mostly about `dc.type` but also `dc.relation`) lands you directly onto the "item"-level page, or, what the documentation calls "single" item view. What I mean by this is that I want the "browse" page to be an index of items that *share* that metadata field.

\2. `webui.browse.link`

This renders a link on the simple (and maybe full?) item view that allows users to "click" and then browse the index for that particular thing. I think the frustration I mentioned in the previous point actually belongs in here, because really what `webui.browse.index` does is it sets up the "search by" dropdown in the navbar:

![400](../assets/img/Pasted%20image%2020240702165409.png)

And then that "search by" gets called when you click on the link? Anyway, it's not even working for. `dc.type`, which is the most straightforward metadata field to browse; I want to view all items that share the current item's particular type, I don't want to search all types that begin with the same string as the current item's type. Why would I ever even want that.?

But anyway changing

```
webui.browse.index = type:metadata:dc.type:text
```

to

```
webui.browse.index = type:item:dc.type:text
```

causes the whole thing to stop working.

\3. Version info

For some reason, the version info automatically generates in PEDSpace in both the simple and full item view but not for PCORnet. I don't know why.

\4. Hannieh's complaints

[Hanieh was not satisfied with the state of the website and how queries return on the search page:](https://pedsnet.slack.com/archives/C055UAV1W31/p1719948167181789)

![Pasted image 20240702170338.png](../assets/img/Pasted%20image%2020240702170338.png)

So I guess I have to figure out how to *automatically* sort search query results by when the data was created, not when the form was submitted.

Anyway, next on the list is upgrading. Alex has recommended that we back everything up by tarballing both the backend and frontend entirely **and** potentially getting a snapshot of both. That's what I'm working on tomorrow.

### 2024-07-02 3:37:35pm

I just finished the simple item view.

### 2024-07-02 2:53:00pm

Dude sometimes you have to call `/opt/dspace/bin/dspace index-discovery -b`. Don't get pissed about it but just FYI.

### 2024-07-02 12:51:29pm

Okay. I synced both PCORnet and PEDSpace.

### 2024-07-01 4:20:35pm

Too much to cover. Jon said all of my work was "outstanding." Later in the meeting we talked about politics.

### 2024-07-01 1:34:00pm

We are [having a meeting](../../2024-07-01%20PCORnet%20DSpace%20Meeting.md).

### 2024-07-01 12:13:49pm

Looks like DSpace 8.0 frontend and backend were released last week… looks like I might have to [upgrade](https://wiki.lyrasis.org/display/DSDOC8x/Upgrading+DSpace)…

Honestly this is really taking the wind out of my sails. I now I have to deliberate whether I should upgrade now (which is a massively non-trivial task with a lot of [breaking changes](https://wiki.lyrasis.org/display/DSDOC8x/Release+Notes#ReleaseNotes-BreakingChanges)) to avoid having to do so later, or just plow ahead with what I have and get something up-and-running ASAP.

One of the major things seems to be that the new DSpace requires Java 17. PCORnet DSpace (AWS) seems to be running Java 11 but PEDSpace (the one whose backend I set up) seems to be running Java 17. Okay.

Both DSpaces also seem to be running Tomcat 9.

I think I'm going to sticking to version 7 and then think about version 8 later.

### 2024-07-01 10:47:04am

Instead of a log entry, [here](https://pedsnet.slack.com/archives/D076H8YDD2P/p1719845032380809) is a message I sent Annabel.

- [x] Figure out the issues with `dspace.entity.type` - is there a way to rename or change these "types?" ➕ 2024-07-01 ✅ 2024-07-15

### 2024-07-01 10:29:01am

Logging is increasingly feeling pointless because my `git commit` history is more acting as a log than anything else.

### 2024-06-27 3:31:16pm

Okay nevermind.

### 2024-06-27 3:03:46pm

This is on the PCORnet-custom in the AWS DSpace.

I really need to figure what this means:

Warning: The file is part of the TypeScript compilation but it's unused. Add only entry points to the 'files' or 'include' properties in your tsconfig.

Paths:
1. `/opt/dspace-angular-dspace-7.6.1/src/themes/PCORnet-custom/app/shared/comcol-page-handle/comcol-page-handle.component.ts`
2. `/opt/dspace-angular-dspace-7.6.1/src/themes/PCORnet-custom/app/workspace-items-delete-page/workspace-items-delete/workspace-items-delete.component.ts`
3. `/opt/dspace-angular-dspace-7.6.1/src/themes/custom/app/entity-groups/journal-entities/item-pages/journal-issue/journal-issue.component.ts`
4. `/opt/dspace-angular-dspace-7.6.1/src/themes/custom/app/entity-groups/journal-entities/item-pages/journal-volume/journal-volume.component.ts`
5. `/opt/dspace-angular-dspace-7.6.1/src/themes/custom/app/entity-groups/journal-entities/item-pages/journal/journal.component.ts`
6. `/opt/dspace-angular-dspace-7.6.1/src/themes/custom/app/entity-groups/research-entities/item-pages/person/person.component.ts`
7. `/opt/dspace-angular-dspace-7.6.1/src/themes/custom/app/item-page/simple/item-types/publication/publication.component.ts`
8. `/opt/dspace-angular-dspace-7.6.1/src/themes/custom/app/item-page/simple/item-types/untyped-item/untyped-item.component.ts`
9. `/opt/dspace-angular-dspace-7.6.1/src/themes/custom/app/shared/comcol-page-handle/comcol-page-handle.component.ts`
10. `/opt/dspace-angular-dspace-7.6.1/src/themes/custom/app/shared/object-list/browse-entry-list-element/browse-entry-list-element.component.ts`
11. `/opt/dspace-angular-dspace-7.6.1/src/themes/custom/app/shared/object-list/collection-list-element/collection-list-element.component.ts`
12. `/opt/dspace-angular-dspace-7.6.1/src/themes/custom/app/shared/object-list/community-list-element/community-list-element.component.ts`
13. `/opt/dspace-angular-dspace-7.6.1/src/themes/custom/app/shared/object-list/search-result-list-element/item-search-result/item-types/item/item-search-result-list-element.component.ts`
14. `/opt/dspace-angular-dspace-7.6.1/src/themes/custom/app/shared/object-list/sidebar-search-list-element/item-types/publication-sidebar-search-list-element.component.ts`
15. `/opt/dspace-angular-dspace-7.6.1/src/themes/custom/app/shared/starts-with/date/starts-with-date.component.ts`
16. `/opt/dspace-angular-dspace-7.6.1/src/themes/custom/app/shared/starts-with/text/starts-with-text.component.ts`
17. `/opt/dspace-angular-dspace-7.6.1/src/themes/custom/app/workspace-items-delete-page/workspace-items-delete/workspace-items-delete.component.ts`

### 2024-06-27 2:42:31pm

- [x] Make `dc.subject.other` "look better" on the simple item page view. ➕ 2024-06-27 ✅ 2024-07-02

### 2024-06-27 10:49:06am

- [ ] We need to [set up the email stuff](https://groups.google.com/g/dspace-tech/c/VSpukAL6Gag/m/6MkaZPm5CgAJ) and update `dspace.cfg`. ➕ 2024-06-27

### 2024-06-27 9:35:12am

Yesterday I did too much to even mention. I'm gonna try to get the `git diff` in here.

Here's a summary of the key changes made between the first commit from yesterday morning (96fc4c009330d3f03c8a8a5149ca5aa945439190) and the last one (31f8957371252aac34672b854607c362e62aed0c):

~~1. **Added New Pipe**:~~
   ~~- Introduced `FirstObjectValuePipe` to extract the first value from an object in the `shared.module.ts`.~~

1. **i18n Updates**:
   - Modified several keys in the `en.json5` file to update labels and translations, such as "Provenance", "Reviewer" to "Reviewer(s)", and updated repository title to "PCORnet Knowledge Repository".

2. **Component Decorator Updates**:
   - Changed the `listableObjectComponent` decorator in several components from 'custom' to 'PCORnet-custom', ensuring they align with the correct theme.

3. **UI Enhancements**:
   - Updated the untyped-item component to display additional metadata fields.
   - Introduced a `badge-container` and styled badges for displaying metadata in a visually appealing manner.
   - Added conditional logic to display the first element of `dc.description.provenance` and structured the `dc.description` field to separate the main description and cohort description.

4. **Styling Changes**:
   - Added new styles to ensure uniformity in headers and to style the badge elements correctly.

5. **Component Adjustments**:
   - Adjusted various components to ensure they reference the correct paths and templates, particularly focusing on the `PCORnet-custom` theme components.

### 2024-06-26 11:17:37am

There's too much to catch up. But I got rid of some awkward padding under the "Authors" field.

### 2024-06-25 4:36:58pm

Okay

- [x] apply security patches before deployment ➕ 2024-06-25 ✅ 2024-07-03

### 2024-06-25 3:09:45pm

I have given up trying to keep a coherent log. At least on the level of granularity that I was doing before.

I fixed some shit. I broke some shit. It doesn't matter.

Just try to make everything passable. Try to do a first pass. You can go back later.

### 2024-06-24 3:57:30pm

I did it. All I had to do was look it up in the damn Google group. And now it seems so obvious in hindsight but it took me multiple celestial epochs to figure it out. I don't know if I would have every figured it out, actually.

Here. Look. Look at the themed `untyped-item.component.ts`.

```ts
/**
 * Component that represents an untyped Item page
 */
@listableObjectComponent(Item, ViewMode.StandalonePage, Context.Any, 'custom')
@Component({
  selector: 'ds-untyped-item',
  styleUrls: ['./untyped-item.component.scss'],
  // styleUrls: ['../../../../../../../app/item-page/simple/item-types/untyped-item/untyped-item.component.scss'],
  templateUrl: './untyped-item.component.html',
  // templateUrl: '../../../../../../../app/item-page/simple/item-types/untyped-item/untyped-item.component.html',
  changeDetection: ChangeDetectionStrategy.OnPush,
})
```

What do you notice? Look at the decorator.

```ts
@listableObjectComponent(Item, ViewMode.StandalonePage, Context.Any, 'custom')
```

Look at the arguments. Do any of them stand out? What about the fourth one?

The fourth parameter for `listableObjectComponent` is THEME. Because I sourced these files from the default custom theme, by default, its theme is set to `custom`, when it should be set to THE NAME OF YOUR THEME.

It would be great if the documentation mentioned this at all! That would be amazing! That would be so good! It would be *ecstasy-inducing* if the documentation mentioned this. Instead, I had to go to the [dspace-tech](https://groups.google.com/g/dspace-tech/c/lAeqmu_QWKw/m/CmYUmOoDDQAJ) Google Groups page to find a PDF from a WEBINAR from TWO YEARS AGO. In a Google Drive. So I had to load the URL on my phone, and then download it as a PDF and AirDrop it to my MacBook.

![Pasted image 20240624160325.png](../assets/img/Pasted%20image%2020240624160325.png)

![Pasted image 20240624160638.png](../assets/img/Pasted%20image%2020240624160638.png)

### 2024-06-24 3:21:45pm

I didn't find it out but I did find out that this DSpace thing has something called `store-devtools` installed by default, which also comes with a chrome extension which basically lets you do debugging.

### 2024-06-24 3:00:32pm

~~[I think I figured it out.](https://gitkraken.dev/link/dnNjb2RlOi8vZWFtb2Rpby5naXRsZW5zL2xpbmsvci84MTNkNWUxNDlhOTM3ZGZjOWY5OTI3MzBjZDdmMDIzMDAwMDg5YzY2L2Yvc3JjL2FwcC9pdGVtLXBhZ2Uvc2ltcGxlL3RoZW1lZC1pdGVtLXBhZ2UuY29tcG9uZW50LnRzP3VybD1odHRwcyUzQSUyRiUyRmdpdGh1Yi5jb20lMkZRdWVyeS1GdWxmaWxsbWVudCUyRlBDT1JuZXQtZHNwYWNlLWFuZ3VsYXItZGV2LmdpdCZsaW5lcz0xOS0yMg%3D%3D?origin=gitlens)
~~

### 2024-06-24 2:41:13pm

This is so weird.

```
src/themes/PCORnet-custom/app/item-page/simple/item-page.component.html
```

The themed `item-page.component.html` (not to be confused with the themed `untyped-item.component.html`) will overwrite the default `untyped-item.component.html`. I. DON'T. KNOW. WHY.

### 2024-06-24 2:16:51pm

I feel like a fool.

After hours and hours and hours of agonizing troubleshooting, I finally figured out what should have been obvious to me all along: the simple item view is being loaded from the default directory. Why? I don't know. But it is.

The simple item view is being loaded from `src/app/item-page/simple/item-types/untyped-item/untyped-item.component.html` instead of `src/themes/PCORnet-custom/app/item-page/simple/item-types/untyped-item/untyped-item.component.html`.

![Pasted image 20240624142003.png](../assets/img/Pasted%20image%2020240624142003.png)

Now, why is this happening? I don't know. Is it important that I figure out how to make it load from my theme instead of just directly altering the default simple item view? Maybe. But when it comes down to it, I'm not going to sink all that much time into it.

### 2024-06-20 2:53:46pm

Okay, so where I left it is that I am having an impossible time altering the simple item view, particularly inside of my theme folder, even though the Chrome devtools seem to indicate that it's being loaded.

#### Troubleshooting and Debugging UntypedItemComponent

**Date**: June 20, 2024

Today, I worked on the `UntypedItemComponent` in my DSpace Angular project, specifically within the PCORnet-custom theme. My primary goal was to make changes to the component and ensure they were reflected on the page.

**Issues Encountered**:
- **Changes Not Reflected**: Despite modifying the component, no changes were visible on the rendered page.
- **Component Loading**: It appeared that the `UntypedItemComponent` was being loaded correctly, but changes to its source file did not affect the output.
- **Errors in Console**: Numerous errors in the console, including WebSocket connection issues and unauthorized API calls, which complicated debugging efforts.

### 2024-06-20 12:48:22pm

Lol my genius brain forgot to update the main `eager-theme.module.ts` thingy.
Sure, here's a succinct summary of what happened and what we accomplished:

**Issue**: Changes to the `untyped-item.component.html` were not being reflected on the webpage, causing confusion about whether the correct template was being used.

**Steps Taken**:
1. **Initial Confusion**: Discovered that the `eager-theme.module.ts` was not correctly pointing to the custom theme, causing the custom templates and styles not to load.
2. **Correction**: Updated `eager-theme.module.ts` to import the correct custom theme module.
3. **Build and Verification**: Ran the build process to ensure the changes were compiled. Checked the network in DevTools to verify that the custom components were being imported.
4. **Visibility Issue**: Found that the test message was being rendered but not visible due to CSS issues.
5. **Solution**: Added a prominent test message in the HTML and styled it to ensure visibility. Verified the changes in the browser.

**Outcome**: Successfully identified and resolved the issue. The custom theme and component changes are now being correctly loaded and rendered, and the test message confirmed that changes are reflected.

### 2024-06-18 3:21:44pm

Tasks:

- [x] Why come we can't send emails? ➕ 2024-06-18 ✅ 2024-07-02
- [ ] Deposit form
	- [x] Making everything available for the deposit form ✅ 2024-07-02
	- [ ] Admin deposit form vs external user deposit form
- [x] Simple item view 🔺 ✅ 2024-07-02
- [ ] Workflow system