---
editor-width: 
aliases: []
tags:
  - PCORnet_Projects_SSQ
  - PCORnet_Projects_AQL
  - PCORnet_Projects_DSpace
status: 
date: 2024-04-17 16:59
type: meeting
date created: Thursday, May 30th 2024, 1:00:43 pm
date modified: Wednesday, June 12th 2024, 9:45:22 am
priority: 🟢 - Low
---

tags: [[Meetings|Meetings]]
Date: [[2024-05-30-Thursday|2024-05-30-Thursday]]

## [[2024-05-30 Project Round Up|2024-05-30 Project Round Up]]

- [[2024-05-30 Project Round Up#Agenda/Questions|Agenda/Questions]]
- [[2024-05-30 Project Round Up#Raw Notes|Raw Notes]]
  - [[2024-05-30 Project Round Up#Raw Notes|Transcription]]
  - [[2024-05-30 Project Round Up#Raw Notes|Post-Meeting Reflection]]
- [[2024-05-30 Project Round Up#Processed Notes|Processed Notes]]

**Attendees**:

### Agenda/Questions

- Project recap

### Raw Notes

#### Transcription

- Setting priorities regardless of which direction we go in
- The one big thing that is changing is AQL opportunities - ending
- DSpace
	- PCORnet has provided us with branding guidelines
	- We will regardless end up doing basic modifications to the look and feel of the websites
	- Getting a read on angular web tech
	- Open source - we don't have great access to structured training
	- Learn as much about DSpace environment
		- version control - i've already started doing this
	- Make it look PCORI-like - either-or internal or external
		- Sometime in the next few weeks, it would be nice to have some sort of non-default branding, a banner/footer
		- no specific guidelines for now - just moving the ball in the direction of reskinning the site
		- Starting the process of how to do this and if we're capable etc.
		- No priority on which DSpace instance we work on first, but let's make sure I have access to the AWS instance (external) first
- SSQ - the first version
	~~- Currently, the way that interaction works is that Pitt (Brian)~~
	- Take a look at it
		- how Alex deploys changes from Pitt
		- How this stuff gets loaded, what we have running
		- it's on another tomcat server
- There are going to be 5 applications stacks that I'm going to have to be aware of
	- SSQ 1
		- Completely custom purpose-built website built on Node.js with psql backend
		- We are not currently responsible for that dev work but maybe in the future, modification of it, play in data viz world
	- SSQ 2
		- Two headed beast
			- Atlas
				- He'll give me a couple of locations that I can start with
				- He wants me to get grounded on how to use Atlas
				- Get an idea of user interface, basic understanding
				- From there, start talking about the data backend of it, mostly what Atlas requires a little bit of user management
				- Repetitive alteration, when we refresh underlying data, we have to change data source connection to that data structure, we have to do some running to do pre-built summarizations
				- That process is all documented
				- Having a knowledge of how to use Atlas
					- What URL it points to, how it works
				- In particular, we have a config of Atlas that is different enough that someone is going to be checking each and every menu item (8-10) to make sure fxnlaity works
			- I2B2 (lower priority)
				- He's asking me to wait on this until we know more about what this thing is
				- We're involved in deploying it, standing it up, then brinigng me up to use, test, having an understanding
				- Might work through technical complexities by the end of next week
		- Both of these are decision-support systems, pre-packaged, pre-built
			- Atlas "I wanna know what drug exposure looks like in these samples"
				- Data overview/summary platform
				- Also allows data scientists to perform data analysis on database
				- Ad-hoc querying platform
				- The good thing is that once i start use Atlas, i'll start to understand their lingo/jargon
				- Basically an extremely sophisticated SQL GUI
				- Both I2B2 and Atlas are ??? (I forgot)
					- Facts vs dimensions
						- The data warehouse built under I2B2 is more facts vs dimensions
							- Dimension is something like date or gender
							- Once you apply the dimension, you have a lot of things underneath that are facts
							- Example: in-patient encounters are better summarized in I2B2 - they're inpatient by month across a 4 year period
							- I2B2 gets mostly used by healthcare economics people, like CFOs
	- DSpace
		- Internal and External (two subprojects)
		- The differentiation is internal vs external
		- There will be plenty of instances, maybe a third, where the content that gets loaded in one will be identical to the stuff that's in the other
		- We need to know metadata for codesets both internally and externally in some cases
		- **In the next couple of weeks, hope to get some reskin**
		- Annabelle will start directing getting comfortable with getting documentation in DSpace environment, and the key might be how I tag the information.
		- The metadata elements of a document need to be tagged which allow it to be searchable, etc.
		- Dimensionally, a piece of content/document presented out to the webserver is going to have 5 specific pieces of metadata on it
			- author/authors - searchable
			- disease or dx - if someone searches for diabetes, it comes up
			- grant
	- AQL
		- We're waiting on that, he would like to propose to wait for a few weeks and lets target something that would be the ground up creation of a single query
		- GUI - table it for now

#### Post-Meeting Reflection

I also accepted the full-time contract offer in this meeting.

### Processed Notes

#### Overall Priorities and Project Directions

- **Priority Setting**: Establishing priorities is crucial, irrespective of the project direction.
- **AQL Opportunities**: The current opportunities for AQL are ending, necessitating a shift in focus.

#### DSpace Projects

- **Branding and Modifications**:
  - PCORnet has provided branding guidelines.
  - Basic modifications to the look and feel of both internal and external DSpace websites are required.
  - Initial steps include:
    - Familiarizing with Angular web technology.
    - Implementing version control (already started).
    - Creating non-default branding elements like banners and footers.
  - There is no specific priority on which DSpace instance (internal or external) to work on first.
  - Ensure access to the AWS instance for the external DSpace site as a priority.

#### SSQ (Self-Service Query) Initiatives

- **SSQ 1**:
  - A completely custom, purpose-built website using Node.js with a PostgreSQL backend.
  - Current responsibility does not include development but may involve future modifications and data visualization.
  - Tasks:
    - Reviewing how changes are deployed from Pittsburgh (Alex).
    - Understanding the deployment and operational aspects of the system.
    - It runs on another Tomcat server.
- **SSQ 2**:
  - **Atlas**:
    - Begin with understanding the user interface and basic functionality of Atlas.
    - Locations for getting started will be provided.
    - Tasks:
      - Understanding user management and data backend requirements.
      - Repetitive alterations needed when refreshing underlying data.
      - Document the process for pre-built summarizations.
      - Ensuring the functionality of menu items (8-10 items) due to configuration differences.
      - Learn how Atlas operates, its URL, and its configuration.
  - **I2B2**:
    - Lower priority; wait for further details.
    - Involvement includes deployment, usage, testing, and understanding the system.
    - Technical complexities to be addressed potentially by the end of next week.
  - **Decision-Support Systems**:
    - Atlas and I2B2 are decision-support platforms:
      - Atlas provides a data overview and summary platform, allowing ad-hoc querying and data analysis.
      - Understand the SQL GUI and the distinction between facts (e.g., inpatient encounters) and dimensions (e.g., date, gender).

#### Detailed Tasks for DSpace

- **Internal vs. External Instances**:
  - Differentiate between internal and external DSpace projects.
  - Some content may be identical across both instances.
  - Metadata tagging is crucial for searchability (author, disease, grant).
  - Annabelle will assist with documentation and tagging in the DSpace environment.
  - Immediate focus on reskinning DSpace within the next few weeks.
  - Ensure all documents have key metadata elements:
    - Author/authors.
    - Disease or diagnosis.
    - Grant information.

#### AQL (Alternative Query Language)

- **Current Status**:
  - AQL project is currently on hold.
  - Proposal to wait a few weeks before starting the ground-up creation of a single query.
  - The development of a GUI for AQL is tabled for now.
