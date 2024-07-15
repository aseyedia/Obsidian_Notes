---
editor-width: 
tags:
  - "#PCORnet_Projects_SSQ"
  - PCORnet_Projects_DSpace
  - PCORnet
  - PCORnet_Projects_SSQ
status: 🚨 - High
progress: 
date: 2024-04-23 14:04
type: meeting
aliases: []
title: 2024-04-23 SSQ and DSpace
date created: Tuesday, April 23rd 2024, 2:04:10 pm
date modified: Thursday, June 13th 2024, 1:53:04 pm
project: SSQ
---

tags: [[🗣 Meetings MOC|🗣 Meetings MOC]]
Date: [[2024-04-23-Tuesday|2024-04-23-Tuesday]]

## [[2024-04-23 SSQ and DSpace|2024-04-23 SSQ and DSpace]]

**Attendees**:
- Alex

### Agenda/Questions

- SSQ
- DSpace

### Raw Notes

- <https://172.22.32.12/ssq/account/sign-in>
- so far demo breakdown
- <https://github.com/Query-Fulfillment/patronus_viz>
	- this is the gh repo for the whole ssq thing
	- `backend/routes/`
		- all python fastapi
		- I didn't get a lot of notes for the Atlas frontend/backend stuff because it was so visual but it's okay, more as an intro; not to the point of running and using it to create cohorts and things like that
- Where this is going
	- in limbo
	- built by a summer student at pitt
	- pcornet demanded it, so there was a sprint
	- We're getting the data in
		- then postgres backend
		- (data sharing agreements)
- Jira is a ticketing service
	- distributing queries
	- sends out query, they send the results back in S3
- aws project idea: for each bucket, event notification (file upload notification), which sends out a notie which sends to simple notie service, which sends to emails, but what you can do is that you can set a lambda function that responds to a json request when a file gets uploaded
	- a file gets uploaded to whatever bucket (results of a query as a zip file)
	- we get an email, just a big json
	- but it could go to a lambda function, which takes the file name and bucket name, and moves it to the proper location (just a different bucket, `drnoc/`, has permissions to duke team who analyzes and approves queries, `pcornet-qf` which is used for agg of regular query results)
		- if it's a data curation file, goes to `drnoc/`, if it's the results of a query, it goes to `pcornet-qf`
		- to make it more complicated, there can also be one-off studies where because of data sharing agreements, sometimes there are study-specific buckets (a researcher from the same institution who has a one-off study, would get their own bucket even if they you know blah blah blah)
- **Snowflake instance:**
	- We have a few different dbs
	- the main reason we use SNF is to work with a group in missouri and they use a program called i2b2 (integrating biology and the bedside)
	- they have connected i2b2 to snowflake and this is all pcornet data going into snf
	- we created "secure views" and shared them with missouri
		- a secure view (query statement)
		- `select` vs `view` vs `secure view`
			- it's secure because they can see the results but not query or underlying data (de-identification also by shifting birthday)
- Atlas
	- Cohort building tool, similar to other application, frontend connects to backend, deployed through tomcat
	- two ways to install atlas
		- straight to server (tomcat, api, etc)
		- or just use docker containers, which is what we want to do
			- they have prevented us from using docker network/podman network to communicate between containers
			- all postgres servers are built on redhat which use podman
			- because we're on an ec2 server, we can use docker network
	- Atlas is a tool specifically made for OMOP
	- we have to make views that convert pcornet to OMOP

- [x] Take a look at Atlas docs; requires OHDSI WebAPI ➕ 2024-04-23 ✅ 2024-05-29

- DSpace
	- Library management system to contain our metadata
	- we have one for pcornet and we're getting one for pedsnet (internally)
	- Right now we have a variable dict which is just a github io page (it's just documentation)
	- we have a lot of info, studies, etc, we want a place for all of it to come together.
	- DSpace is a prebuilt metadata library
	- uses apache maven and ant and solr and tomcat, etc
	- what he and i will be doing is filling in the gaps before getting a contractor to come fix up to front end
		- understand how the application works better and more in-depth, mostly concerning how items are laid out
		- right now we're confined to their defined dataset, which makes things more rigid
			- their pre-built metadata fields are limit
			- we're expecting that if we add fields named after what's available to the backend, it doesn't work; it does not show the autocomplete and just says 'no'
			- So he will set up a local VM for me and what i can do from there and work through the installation process of it and that will serve as the internal space that we have
				- gives us a second instance that we can build on
				- also gives us more knowledge; if i know more stuff i can be someone else who knows about this kind of junk
				- the reason we are doing this is because we are replacing the variable dictionary gh thing
				- he said we'll get this done in a month 😱
				- this is my task
				- this is my project
- setting up the DSpace instance in a month
	- working with her to push that into the instance that we have
	- the other piece is that we made a redcap form and it takes in information and there is information about code sets that fill the github io page

- [x] Create a task note for the the DSpace project FOR PEDSNET - we are just setting up an instance ➕ 2024-04-23 ✅ 2024-04-29

- DSpace is going to serve two purposes
	- deliverable for pcornet; they want a metadata repo
		- they want a collection of codesets, studies, data quality modules
	- An internal library of PEDSnet variable dictionary - OMOP centered
- How do i mentally categorize these techs?
	- generally, going to phase 4 of pcornet, there is a push to update the tech
	- The way it has worked historically is archaic
	- self service query 1 - already
	- I2B2 - more in-depth cohort creation, basically the same as Atlas
	- Atlas - cohort creation for PEDsnet
	- DSpace - streamline metadata accessibility and interface
		- also pedsnet for internal use
- as an aside, why is SAS so popular with the government?
- so he will let me know when he sets up a VM for me

#### Post-Meeting Reflection

### Processed Notes

- **SSQ Highlights**:
    - A demo breakdown was provided, along with a reference to the GitHub repository for SSQ, highlighting backend routes using Python FastAPI.
    - The project is currently in limbo, initiated due to a demand from PCORnet and built by a summer student at Pitt.
    - The process of data integration into a Postgres backend is underway, subject to data sharing agreements.
    - Mention of using Jira for distributing queries and AWS for managing query result notifications via email or Lambda functions for file management.
- **Snowflake Instance**:
    - Utilization of Snowflake (SNF) for collaboration with a group in Missouri using i2b2, with secure views created for data sharing.
- **Atlas**:
    - A cohort building tool, with discussions on installation methods and its specific use for OMOP data.
    - A task was noted to review Atlas documentation and OHDSI WebAPI.
- **DSpace**:
    - Described as a library management system for metadata, with plans to set up an instance for PEDSnet internally.
    - The project involves understanding DSpace more deeply and potentially adding new metadata fields.
    - A task was set to create a note for the DSpace project for PEDSnet, aiming to consolidate information and streamline metadata accessibility.
    - DSpace serves dual purposes: as a deliverable for PCORnet and as an internal library for PEDSnet's variable dictionary.
- **Technological Categorization**:
    - Reflections on updating technology for phase 4 of PCORnet, comparing various tools like SSQ, i2b2, Atlas, and DSpace for different purposes.
- **Miscellaneous**:
    - A brief mention of the popularity of SAS with the government.
    - A note that a VM setup for further DSpace development will be communicated.