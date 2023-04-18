***System Configuration*** ![[Pasted image 20230223152605.png]]

**System Information**

Command to open: `msinfo32.exe`

- The System Information `msinfo32` tool is viewable per the System Configuration panel in windows.

- It gathers information about the computer to display a comprehensive  view of hardware, software, and system componenets. 
		- Can be used in the diagnosis of computer issues.

- ![[Pasted image 20230223144230.png]]

As, can be seen there are **Hardware Resources**, **Components**, and **Software Environment** panels.


- Within the Hardware Resources panel, one can obtain in depth information about hardware components and their specifications.    

- The **Components** tab displays information about hardware dvices insatlled on the computer, such as the Display and Keyboard/Mouse devices.

**Software Environment**


- Shows information about services and softwar baked into the operating system.
	- Can view things such as Environment Variables, Network connections, and Drivers.

**Viewing Environment Variables**

- Can be viewed in System Information -> Software Environment -> Environment Variables
- Can be viewed in `Control Panel > System and Security > System > Advanced system settings > Environment Variables` **OR** `Settings > System > About > system info > Advanced system settings > Environment Variables`

![[Pasted image 20230223144758.png]]

**Using the Search Bar in `MSInfo32`**

- We can select a dropdown category, and then input a search in the Searchbar to more easily find a specific field we are looking for.


![[Pasted image 20230223144957.png]]

**Resource Monitor `resmon`**

- Resource Monitor is another tool under the System configuration panel that displays per-process aggregated CPU, memory, disk, and network usage information. 

- Allows users to isolate data related to one or more processes, and can alloow them to start, stop, pause, and rsume services.

- User can also close unresponsivle applications from the user interface.
	- Geared towards advanced users who need to perform advanced troubleshooting on the computer system.

![[Pasted image 20230223151343.png]]

- We can see that it has four sections:

**CPU**

- The CPU tab will show additional information about processes, services, and handles associated with the processes. 

 **Memory**

![[Pasted image 20230223152107.png]]

Shows memory usage on a process by process level, as well as physical memory availability.

**Disk**

- Shows disk availability across drives, as well as read write times of specific processes running on the drive.

**Network**

- Shows network activity that the current machine is part of, as well as TCP connections and listening ports. 
	- Shows the processes that are related with network activity.

![[Pasted image 20230223152326.png]]

***Volume Shadow Copy Service***

- The Volume Shadow Copy Service (VSS) coordinates actions to create a shadow copy, which acts as a snapshot of data that is to be backed up on a drive.

- In most cases, VSS is enabled via System Protection being turned on in Windows.
		- VSS is used to create a restore point
		- Perform system restore
		- configure restore settings
		- delete restore points


- Knowing this, malware writers can include code in their malware to look for file related to VSS, and delete them. This would make it impossible for a victim to recover from a ransomware attack unless we had other external backups.

![[Pasted image 20230223155832.png]]


- Can configure shadow copies by right clicking a drive.