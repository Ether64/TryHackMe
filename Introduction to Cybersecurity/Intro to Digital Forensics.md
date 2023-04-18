
*Digital Forensics* is a process that can be applied to desktop computers, laptops, digital computers, music players. It also involves investigating data on digital media such as CDs, DVDs, USB flash drives, and external drives.

- **Digital Forensics** is the application of computer science to investigate digital evidence for a legal purpose. It is used in two types of forensic investigations:
	- Public-sector investigations: refers to investigations carried out by government and law enforcement agencies.
	- **Private-sector investigations** refer to investigations carried out by corporate grousp through a private investigator, and are caused by corporate policy violations.


- In both cases, the **chain of custody** must be preserved, which is a long-standing documentations of who and what has interacted or come into contact with digital evidence related in any way to the investigation.

- Without trained digital forensics investigations, it wouldn't be possible to properly process or analyze digital evidence.


**DFIR Process**

1. Acquire Evidence via Memory Acquisition Tools
2. Establish a Chain of Custody (filling out related form)
3. Security Quarantine the Evidence
4. Transport the Evidence to your DFIR lab

Generally, DFIR involves:

- Proper search authority
- Chain of Custody preservation
- Validation with hash functions
- Use of validated memory acquisition and memory forensic tools
- Repeatability and Reporting


![[Pasted image 20230204123222.png]]

The `pdfinfo` tool can be used to view metadata about a pdf.

- Similarly, the `exiftool` can be used to view metadata of various image pased file types such as **png** and **jpg.** 

- ![[Pasted image 20230204123600.png]]

- Exiftool provides us valuable information such as the GPS coordinates that correspond to the picture.