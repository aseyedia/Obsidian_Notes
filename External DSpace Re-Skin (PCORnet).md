---
editor-width: 
aliases: []
status: 
date created: Tuesday, April 23rd 2024, 11:54:05 am
date modified: Wednesday, July 3rd 2024, 11:07:49 am
tags:
  - PCORnet_Projects_DSpace
priority: 🚨 - High
project: DSpace
tech: 
---

> [!todo] Incomplete Tasks
>
> ```tasks
> # this query is inside of a callout. Learn more about callouts [here.](https://help.obsidian.md/Editing+and+formatting/Callouts)
> path includes {{query.file.path}}
> hide backlink
> not done
> ```

[[./docs/hidden/Unified DSpace Project Log|Unified DSpace Project Log]]

> [!todo] Incomplete Tasks UNIFIED
>
> ```tasks
> # this query is inside of a callout. Learn more about callouts [here.](https://help.obsidian.md/Editing+and+formatting/Callouts)
> path includes CHOP PCORnet Team/Project Management/DSpace/Unified DSpace Project Log.md
> sort by created
> # hide backlink
> not done
> ```

## Background

We're creating a public version of [[Internal DSpace Instance Setup Log|DSpace]] on AWS for PCORnet (confusing). Part of what brought me on for full-time was the assignment to tool around with the UI. It feels like I'm being boxed out or something. I don't know.

## Resources

- From Annabelle:

> And some other resources:
> The project Jira Ticket: [https://atlassian.chop.edu/jira/browse/OPS-364](https://atlassian.chop.edu/jira/browse/OPS-364)
> QF Project folder in Sharepoint: [QF Metadata Repository](https://chop365.sharepoint.com/:f:/r/teams/RSCH-ACRC/Shared%20Documents/PCORnet%20QF/QF%20Phase%202/QF%20Metadata%20Repository?csf=1&web=1&e=kf7iqN)
> A rough roadmap for the dev of the PCORnet site, which will be pretty much mirrored in PEDSnet site: [Development_Task_List.xlsx](https://chop365.sharepoint.com/:x:/r/teams/RSCH-ACRC/Shared%20Documents/PCORnet%20QF/QF%20Phase%202/QF%20Metadata%20Repository/Repository_Development/Development_Task_List.xlsx?d=w1ab2f2e372d24728a20f5c31d91d0f05&csf=1&web=1&e=YMbNfR)

- [URL to the instance](https://172.22.32.9/home)
- [ChatGPT Thread (Same as Internal)](https://chatgpt.com/c/a7438949-f559-4b3e-8d92-ed8a264e83b5)
- [Auxiliary ChatGPT Thread](https://chatgpt.com/c/c7819040-97c8-41dd-b0fa-5b65bf4744cd)
- [PCORnet Site Mockup](https://www.figma.com/design/IUnfzvRbxTA3rMU7NStE6u/QF-Mock-ups?node-id=0-1)
- [User Interface Customization](https://wiki.lyrasis.org/display/DSDOC7x/User+Interface+Customization)
- [Annabelle's Kanban](https://github.com/users/abelpink/projects/1)
- [[Metadata Item Mapping|Metadata Item Mapping]]

## Additional Notes

## Log

### 2024-06-25 2:21:09pm

I just got a free student edition of Bootstrap Studio.

```spoiler-block
9a8155e00b6d3d1430f924d74f90da46c3acbda2
```

- [ ] Change 'code sets' to 'concept sets' ➕ 2024-06-25
- [x] fix "Collections" & "Domains" ➕ 2024-06-25 ✅ 2024-06-27

### 2024-06-17 4:16:25pm

### 2024-06-17 1:53:21pm

From the meeting:

- [x] Fix the Communities/Collections ➕ 2024-06-17 ✅ 2024-06-17
- [x] "All of Repository" ➕ 2024-06-17 ✅ 2024-06-17
- [ ] Registered trademark symbol ➕ 2024-06-17
- [ ] Data deposit button ➕ 2024-06-17
- [ ] [[Metadata Item Mapping|Metadata Item Mapping]]

### 2024-06-17 1:12:59pm

Okay. Now I'm going to change what was originally known as a collection page but has now been changed to a "domain" page:

![[./docs/assets/img/Pasted image 20240617131352.png|Pasted image 20240617131352.png]]

Basically all I need is the banner.

Here are the RGB values: `144	189	77`

![[./docs/assets/img/Pasted image 20240617131746.png|Pasted image 20240617131746.png]]

That was kind of easy. It's not exactly matching the mock up but it might be good enough. We will have to see.

I'm tired, and anxious, and I ~~feel~~ am noticing that I am experiencing ~~bad~~ the sensation of discomfort and dysphoria. I'm a little bit over-stimulated right now. Maybe I'm crashing. I'm feeling tired.

### 2024-06-17 1:05:38pm

It's telling me that I need to restart Ubuntu, but I'm worried something might happen and Alex is away for vacation right now, so I think it's best to wait.

- [x] Reboot when Alex gets back ➕ 2024-06-17 📅 2024-06-24 ✅ 2024-07-03

### 2024-06-14 11:57:34am

I'm taking a break from this for now. I am right now running a `tmux` session with the `dev` server running so I need to remember to attach to that session or shut it down:

- [x] Shut down `tmux yarn start:dev` session with `tmux a` ➕ 2024-06-14 ✅ 2024-06-17

I made some solid progress on this thing.

Here are some of my issues:

- [x] The homepage banner is too low res - it gets stretched and pixelated. Bad. Better to use a higher res image, even a purple one. Actually I can just generate one with ChatGPT. Never mind. ChatGPT sucks. I'm sure we can find a higher res version of this image somewhere. ➕ 2024-06-14 ✅ 2024-06-14
- [x] I can't figure out how to get the banner to stretch across the width of the screen ➕ 2024-06-14 ✅ 2024-06-17

### 2024-06-14 10:44:03am

I decided to switch to a "custom" theme and I copied all of my changes so far into the custom theme. [It works.](https://github.com/Query-Fulfillment/PCORnet-dspace-angular-dev/commit/72caaa9ff2499451fc4218393b0e751b47c4eeb1)

### 2024-06-13 5:01:22pm

I got a lot done. I'm not going to get into it all. But I left off trying to import the `community-page` module.

![[./docs/assets/img/Pasted image 20240613170148.png|Pasted image 20240613170148.png]]

### 2024-06-13 3:43:02pm

Okay so the `en.json5` file is a bit more complicated. There are placeholders inside curly brackets (i.e. `{{ community }}`) that should NOT be touched. I restored `en.json5` to `init` and asked Chat to write me a script that would handle these instances:

> okay. i just gave you a text file. I want you to write me a shell script (doesn't have to be shell, it can really be anything, like python for example) that turns all instances of the word 'Community' (plural, upper/lower case) to 'Collection', and all instances of 'Collection' to 'Domain.' You'll probably want to do collection to domain first, so as to not change both community and collection to domain. Also, I want you to create some exceptions:
>
> 1. these changes ONLY apply to text occurring after the first colon in a line:
>
> so for example:
>
> "communityList.breadcrumbs": "Community List",
>
> I do NOT want to change the word 'community' inside of the parameter name 'communityList'. Only after the colon i.e. "Community List"
>
> 1. I do NOT want to change words occurring between curly brackets:
>
> "community.curate.header": "Curate Community: {{community}}",
>
> Here, the only word that would change is 'Community' in 'Curate Community':
>
> "community.curate.header": "Curate Collection: {{community}}",

it wrote me the following:

```python
import re

def replace_words(file_path):
    # Read the contents of the file
    with open(file_path, 'r') as file:
        lines = file.readlines()

    # Function to perform the replacements
    def replace_after_colon(text):
        # Split text by ':'
        parts = text.split(':', 1)
        if len(parts) < 2:
            return text

        # The text after the colon
        after_colon = parts[1]

        # Replace 'Collection' with 'Domain' first
        after_colon = re.sub(r'(?<!\{)(?i)\bCollection\b(?!\})', 'Domain', after_colon)

        # Then replace 'Community' with 'Collection'
        after_colon = re.sub(r'(?<!\{)(?i)\bCommunity\b(?!\})', 'Collection', after_colon)

        # Reconstruct the line
        return parts[0] + ':' + after_colon

    # Perform replacements for each line
    updated_lines = [replace_after_colon(line) for line in lines]

    # Write the updated contents back to the file
    with open(file_path, 'w') as file:
        file.writelines(updated_lines)

# Specify the path to your file
file_path = '/mnt/data/en.json5'
replace_words(file_path)
```

### 2024-06-13 3:13:37pm

I wanted to make changes to `footer` using my theme, so I copy+pasted it into `app/`, but it wasn't being recognized by Angular? so I had to update `eager-theme-module.ts`.

### 2024-06-13 2:20:24pm

Alright so Annabelle is saying turn 'community' to 'collection' and 'collection' to 'domain.' Okay.
- [x] Change "Community/Category" to "Collection" and "Collection" to "Domain ➕ 2024-06-13 ✅ 2024-06-13

```
(?<=:\s"[^"]*)Collection
(?<=:\s"[^"]*)collection
(?<=:\s"[^"]*)Category
(?<=:\s"[^"]*)Categories
(?<=:\s"[^"]*)category
(?<=:\s"[^"]*)categories
```

The first two lines also select plural `collections`.

### 2024-06-13 1:50:46pm

Phoney-baloney. That's a funny phrase. Apparently there are mockups that I was never given.

![[./docs/assets/img/Pasted image 20240613135119.png|Pasted image 20240613135119.png]]

Here's what this thing looks like so far.

### 2024-06-13 9:08:23am

I didn't mention this yesterday, but basically, I started my own branch and I deleted everything in the folder and replaced it with a fresh copy of DSpace, and basically it's looking A-OK so far.

### 2024-06-12 2:45:38pm

He also didn't do the `cron` commands.

### 2024-06-12 2:04:52pm

Okay, I met with Alex a few hours ago and he gave me the run-down on how to do this damn thing. He's already made a lot of changes, I guess to gain a familiarity with this stuff. Anyway, he did not do things the way I would/did do them on the server, which isn't to cast aspersions, but only to highlight how differently we approached this.

For example, he didn't use `jvsc`; instead, he used the `shutdown.sh` and `startup.sh` scripts for Tomcat. Also he runs the site out of a `tmux` session using the `node ./dist/server/main.js` command, which works but doesn't keep a log. Also, there is a weird thing that happens when you go to the site:

![[./docs/assets/img/Pasted image 20240612141505.png|Pasted image 20240612141505.png]]

It first loads this completely default or at least mostly default

### 2024-06-11 11:06:29am

I still don't have the AWS information. I have not been onboarded on this project yet. Also I'm pretty sure the public depository thing is just for the PEDSnet instance. [[2024-06-11 Metadata Meeting|2024-06-11 Metadata Meeting]]

### 2024-06-11 9:58:01am

It's up again.

### 2024-06-11 9:42:09am

The DSpace website is down because it's "undergoing maintenance."

![[./docs/assets/img/Pasted image 20240611094646.png|Pasted image 20240611094646.png]]

### 2024-06-10 4:22:00pm

Alex just told me to look into setting up a public depository.

- [x] Set up a public depository ➕ 2024-06-10 ✅ 2024-06-11