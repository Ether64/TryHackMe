
- First asks us to find how many ports are open.

`sudo nmap <IP> -F`

![[Pasted image 20230204162906.png]]

We can see there are 2 open ports.

- Find version of Apache:

`nmap -sV 10.10.200.176`

![[Pasted image 20230204163010.png]]

**Find directories running on the web server using gobuster**

- `gobuster dir -w <wordlist> -u <url/IP>`

Once this completes, we find


![[Pasted image 20230204164014.png]]

- Found directories:
	- /css
	- index.php
	- /js
	- /panel
	- /uploads

**Creating Reverse Shell to upload php**

- ![[Pasted image 20230204164305.png]]
We decide to use the php reverse shell included with the system utilities. 


We go inside that php file and edit the parameters
	- ![[Pasted image 20230204164602.png]]

Now that we have our php reverse shell, we need to upload it using Burp.

**Php Reverse Shell Upload** 

- Turn on Proxy

- Turn on Listener

![[Pasted image 20230204165109.png]]


- Used burpsuite to change upload extension to *.phtml*

- THis allows ux to bypass the filtering of certain file extensions that were being filtered out, which was php

![[Pasted image 20230204165734.png]]

![[Pasted image 20230204165744.png]]

As you can see, this upload is permitted, and `phtml` will still execute php code.


**Creating a Web Shell**
![[Pasted image 20230204171801.png]]


- We upload a created `reverse.phtml` shell to get a web shell through the discovered /uploads directory.

![[Pasted image 20230204171614.png]]

Once we have done this, we can navigate to the http://10.10.200.176/uploads/reverse.phtml and can perform command executions using the `?cmd=` syntax.

- THe `?` is used to add a web parameter.

![[Pasted image 20230204172031.png]]

Theoretically, we will now be able to access the reverse shell on our machine by performing a bash execution through the added `?cmd='` web parameter.



