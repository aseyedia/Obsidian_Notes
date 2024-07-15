---
editor-width: 
aliases: []
tags:
  - PCORnet_Projects_DSpace
status: 
date: 2024-06-11 10:02
type: meeting
date created: Wednesday, April 17th 2024, 4:59:33 pm
date modified: Tuesday, June 11th 2024, 10:56:19 am
---

tags: [[Meetings|Meetings]]
Date: [[Work Daily Log|Work Daily Log]]

## [[2024-06-11 Metadata Meeting|2024-06-11 Metadata Meeting]]

- [[2024-06-11 Metadata Meeting#Agenda/Questions|Agenda/Questions]]
- [[2024-06-11 Metadata Meeting#Raw Notes|Raw Notes]]
  - [[2024-06-11 Metadata Meeting#Raw Notes|Transcription]]
  - [[2024-06-11 Metadata Meeting#Raw Notes|Post-Meeting Reflection]]
- [[2024-06-11 Metadata Meeting#Processed Notes|Processed Notes]]

**Attendees**:

- Annabelle
- Jon
- Alex
- Ashley

### Agenda/Questions

- DSpace-related things

### Raw Notes

#### Transcription

- Alex is showing his UI changes to the DSpace frontend
- Annabelle is going to have the codesets ready by mid-July
- <https://172.22.32.9/home>
- You can edit the frontend for the PEDSnet while Annabelle is uploading
- Create a deposit form for the PEDSnet instance and then maybe we will use it for the PCORnet instance.
- Take a look at changing the label names
- [Example](https://172.22.32.9/items/5839e278-a919-4018-92f5-947e9f35b489):

On the simple item page:

![[./docs/assets/img/Pasted image 20240611102747.png|Pasted image 20240611102747.png]]

`Code set intended to identify patients with adult respiratory distress syndrome (ARDS).` is under "Abstract," but on the full item page:

![[./docs/assets/img/Pasted image 20240611102708.png|Pasted image 20240611102708.png]]

`Code set intended to identify patients with adult respiratory distress syndrome (ARDS).` is under `dc.description.abstract`.

Actually her issue is the other way around. And she said she is going to send me the mapping.

Basically she wants the "Simple item page" to map to the "Full item page" in her preferred way.

#### Post-Meeting Reflection

- Alex has already made some major headways with customizing the frontend UI
- I can edit the frontend UI while Annabelle is uploading codesets; those two things shouldn't conflict with each other.
- The frontend requirements for the internal (PEDSnet) DSpace are very minor whereas the frontend requirements for the external (PCORnet) DSpace are detailed and meticulous.

### Processed Notes