---
editor-width: 
aliases: []
status: 
title: Internal DSpace Re-skin Project
date created: Tuesday, April 23rd 2024, 11:54:05 am
date modified: Wednesday, July 3rd 2024, 12:52:33 pm
tags:
  - PCORnet_Projects_DSpace
priority: 🟢 - Low
project: DSpace
tech: 
---

> [!todo] Incomplete Tasks
>
> ```tasks
> # this query is inside of a callout. Learn more about callouts [here.](https://help.obsidian.md/Editing+and+formatting/Callouts)
> path includes {{query.file.path}}
> sort by created
> hide backlink
> not done
> ```

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

- [[2024-05-30 Project Round Up|2024-05-30 Project Round Up]]

I'm taking over Alla's job of doing the suicide-inducing and eye-plucking (according to Alex and Jon lol) of fixing the widgets and basically performing tedious frontend stuff.

I am supposed to do this for both the internal and external instance, but I am still waiting on getting access to the external DSpace AWS instance.

So for now I will focus internally.

## Resources

- [[Internal DSpace Instance Setup Log|Internal DSpace Instance Setup Log]]
- ChatGPT Threads
	- [Thread 1](https://chatgpt.com/c/a7438949-f559-4b3e-8d92-ed8a264e83b5)
- **DSpace Documentation** (Descending level of priority)
	- [User Interface Customization](https://wiki.lyrasis.org/display/DSDOC7x/User+Interface+Customization)
		- The DSpace 7.x User Interface (UI) is built on the Angular framework, with data served from the DSpace Backend REST API and HTML pages generated via TypeScript. Customizing the UI involves understanding Angular components, Bootstrap for layout, and Sass for styling. Key customization steps include modifying global styles, logos, navigation links, and footers, running the UI in developer mode for live changes, and ensuring components are correctly registered and updated in your theme's module files.
	- [User Interface Configuration](https://wiki.lyrasis.org/display/DSDOC7x/User+Interface+Configuration)
		- The DSpace 7.x User Interface (UI) uses YAML configuration files starting from version 7.2, replacing the earlier TypeScript-based configuration format. This new format supports runtime configuration, allowing changes to be applied simply by restarting the UI. Key configuration areas include UI core settings, REST API settings, caching, authentication, and theming. The system also supports overriding configurations via environment variables and .env files for added flexibility.
	- ==[Submission User Interface](https://wiki.lyrasis.org/display/DSDOC7x/User+Interface+Configuration)==
		- In DSpace 7.x, the item submission process is configured through XML files, notably `item-submission.xml` and `submission-forms.xml`. The process consists of several steps, including selecting a collection, describing the item, uploading files, agreeing to a license, and depositing the item. Additional optional steps, such as setting item access conditions or assigning Creative Commons licenses, can be enabled as needed. Customizing the submission process involves modifying these XML files to reorder, add, or remove steps, and creating custom metadata forms tailored to specific collections or item types. Detailed documentation within these configuration files and supplementary resources in the DSpace documentation help guide these customizations.
	- [Configurable Workflow](https://wiki.lyrasis.org/display/DSDOC7x/Configurable+Workflow)
		- The DSpace 7.x documentation outlines the configuration of workflows, which define how documents are reviewed and edited before publication in the repository. Each workflow comprises a series of steps, each associated with specific roles and actions. These configurations are set in `workflow.xml` using Spring Bean syntax, allowing for flexible, customizable workflows. Data migration scripts are provided to convert older workflows to the new system, ensuring seamless transition and compatibility.
	- [PEDSnet Mockup](https://www.figma.com/design/1RKPvReaUR1IaHiRXvIIhy/PEDSpace_Mock_Up?node-id=0-1)
		- Mock up of desired final product
	- [Annabelle's Kanban](https://github.com/users/abelpink/projects/1)
	- [Jira Page](https://atlassian.chop.edu/jira/browse/OPS-374)

- [x] Customize UI ➕ 2024-05-30 ✅ 2024-06-11
- [x] Configure UI ➕ 2024-05-30 ✅ 2024-06-11
- [x] Submission UI ➕ 2024-05-30 ✅ 2024-06-11
- [x] Configure Workflow ➕ 2024-05-30 ✅ 2024-06-11
- [x] Make some aesthetic improvements to the DSpace skin, have it at least say "PCORnet" ➕ 2024-06-11 ✅ 2024-06-12
- [ ] Set up a public depository ➕ 2024-06-11
- [x] Make the "simple item page" and the "full item page" match - reach out to Annabelle for her mapping for this one ➕ 2024-06-11 ✅ 2024-07-02
- [x] Make sure the website matches the mockup ➕ 2024-06-13 ✅ 2024-07-02
- [x] Change "Community/Category" to "Collection" and "Collection" to "Domain ➕ 2024-06-13 ✅ 2024-07-02
- [x] Will eventually say PEDSpace instead of PEDSnet Variable Dictionary ➕ 2024-06-18 ✅ 2024-06-18
- [x] the Github ticket Annabelle made ✅ 2024-06-18
- [x] Collections/Domains ✅ 2024-06-18
- [ ] Data deposit form

## Additional Notes

## Log

### 2024-07-02 10:52:31am

I brought in all of the PCORnet custom theme changes.

There are so many issues with permissions in this DSpace instance. I wish I just kept it all under one account. This sucks.

```sh
sudo chown -R :dspace *
sudo chmod -R 775 src/app/
```

You just need to call these two constantly.

### 2024-06-18 2:42:16pm

- For "Collection" (Case-Sensitive)

```regex
(?<=:\s(?:[^{}]|(?:\{\{.*?\}\}))*?)\bCollection\b
```

- For "collection" (Case-Sensitive)

```regex
(?<=:\s(?:[^{}]|(?:\{\{.*?\}\}))*?)\bcollection\b
```

- For "Communities" (Case-Sensitive)

```regex
(?<=:\s(?:[^{}]|(?:\{\{.*?\}\}))*?)\bCommunities\b
```

- For "communities" (Case-Sensitive)

```regex
(?<=:\s(?:[^{}]|(?:\{\{.*?\}\}))*?)\bcommunities\b
```

- For "Community" (Case-Sensitive)

```regex
(?<=:\s(?:[^{}]|(?:\{\{.*?\}\}))*?)\bCommunity\b
```

- For "community" (Case-Sensitive)

```regex
(?<=:\s(?:[^{}]|(?:\{\{.*?\}\}))*?)\bcommunity\b
```

### 2024-06-18 1:42:57pm

okay never mind i don't need the pexels api key

### 2024-06-18 1:40:36pm

Pexels API key

```spoiler-block
yMRmI6I5eGNz9NVlVv1O0apaJlGwHUDGkAP3zg7APFt8WPHuKkFz00ma
```

### 2024-06-18 12:23:05pm

I am back.

### 2024-06-12 2:03:01pm

Okay. I'm shelving this for now. I am going to move onto the [[External DSpace Re-Skin (PCORnet)|External DSpace Re-Skin (PCORnet)]].

### 2024-06-12 1:15:39pm

I'm going to wrap up this project (for now) and deploy the production build. Switching over to the AWS instance of DSpace.

Wait actually I'm going to change the title and some of the words around.

### 2024-06-12 9:39:51am

I got a lot of good compliments from Annabelle which were really nice to receive. The site is coming along nicely. Another thing though that I forgot about is the fact that I need to change the capitalization for "Category." When I changed "Community" to "Category," I capitalized the "C" for all of them, but the issue with that is sometimes there are words like "Subcategories" and other times, the "C" just isn't intended to be capitalized.

- [x] Control for "Category" capitalization - maybe rollback first? ➕ 2024-06-12 ✅ 2024-06-12

Make sure you have Aa enabled:

```
(?<=:\s"[^"]*)Community
(?<=:\s"[^"]*)Communities
(?<=:\s"[^"]*)community
(?<=:\s"[^"]*)communities
```

### 2024-06-11 4:59:19pm

Alright I changed Community to Category and also I changed the background except I'm not thrilled with how its cropped on full screen. Oh well. I'll do that tomorrow.

- [x] Change how the banner is cropped when screen is maximized. ➕ 2024-06-11 ✅ 2024-06-12

### 2024-06-11 3:59:37pm

Okay so I did some stuff. I changed around the homepage for DSpace. I am also changing the word "Community" to "Category."

Here's the regex I'm using to find+replace in VSCode:

```
(?<=:\s"[^"]*)Community
(?<=:\s"[^"]*)Communities
```

### 2024-06-11 3:36:28pm

Alright. So I got some things I need to do. Also Alex is going to talk to me about transferring the internal DSpace stuff over to me.

### 2024-06-11 10:56:41am

Alright. I'm a little confused and I feel like I'm kind of being mismanaged here a little bit, but [[2024-06-11 Metadata Meeting|it seems like]] Alex is working on the [[External DSpace Re-Skin (PCORnet)|External DSpace Re-Skin (PCORnet)]] and I guess the way it's going to work is that I'm going to make changes to the the PEDSnet instance (this one) as like a pilot of sorts and then we're going to figure out how to implement them in the PCORnet (external) instance.

### 2024-06-10 12:35:53pm

Okay I am back.

### 2024-06-07 2:37:05pm

Oh wow I found some [useful videos.](https://www.google.com/search?q=dspace+ui&rlz=1C5CHFA_en&oq=dspace+ui&gs_lcrp=EgZjaHJvbWUyCQgAEEUYORiABDIHCAEQABiABDIHCAIQABiABDIHCAMQABiABDIHCAQQABiABDIHCAUQABiABDIGCAYQRRg8MgYIBxBFGDzSAQgyNjQ3ajBqN6gCALACAA&sourceid=chrome&ie=UTF-8)

### 2024-06-05 10:29:44am

I'm starting over. I uploaded the `custom` theme back to the DSpace Angular folder to replace the old one and I'm going to work out of a copy of the custom folder now.

Actually, I'm just going to use a copy of the "example" DSpace theme to train and then move onto the the "custom" theme.

### 2024-06-04 4:27:30pm

Okay I've been going at this thing for a little bit now.

The big issue I'm running into is step [right here.](https://wiki.lyrasis.org/display/DSDOC7x/User+Interface+Customization#UserInterfaceCustomization-ThemeDirectories&DesignPrinciples:~:text=Copy%20your%20logo%20to%20your%20theme%27s%20assets/images/%20folder.%20Anything%20in%20this%20theme%20folder%20will%20be%20deployed%20to%20/assets/%5Btheme%2Dname%5D/images/%20URL%C2%A0%20path.)

Basically, it's like this super fine-grain, minute thing about how like… apparently, the images I put into `src/themes/custom/assets/images` (from now on I will start from the project root directory, but for future reference, this is the full path `/opt/dspace-angular-dspace-7.6.1/src/themes/custom/assets/images`) will be "deployed" to `/assets/custom/images/`. Except, that doesn't happen. Supposedly, these images eventually end up in `dist/`?

Anyway, what's further bewildering is that in `src/themes/custom/app/header/header.component.html`, the default logo image path:

```html
<img src="assets/images/dspace-logo.svg" [attr.alt]="'menu.header.image.logo' | translate"/>
```

works the same as the custom image path???

```html
<img src="/custom/assets/images/dspace-logo.svg" [attr.alt]="'menu.header.image.logo' | translate"/>
```

But using an imported image doesn't work at all????

```sh
<img src="assets/images/pedsnet-cropped-1.svg" [attr.alt]="'menu.header.image.logo' | translate"/>
```

Also wtf is this:

![[./docs/assets/img/Pasted image 20240604165324.png|Pasted image 20240604165324.png]]

This is brutal. I think starting tomorrow, I'm going to just completely reset the DSpace Angular thing back to its first commit, download the DSpace UI frontend again but to a different directory (a development instance in like `/data` or something), and start all over. I think even delete the GitHub repo I made for it because I want to start from a blank slate.

### 2024-05-30 4:32:56pm

Alright. Let's get started on this damn stuff.

DSpace UI is build on Angular. The Data comes from DSpace's REST API backend, and the final HTML pages are generated via TypeScript.

> You do not need to know Angular or TypeScript to theme or brand the UI.