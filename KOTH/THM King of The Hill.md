
***2/22/2023***


- Once we receive our IP address, we begin by scanning for active services and their versions.


![[Pasted image 20230221164836.png]]

We see it is running ports 139, 445, 80, 22, 3306, 8080, 53...

![[Pasted image 20230221165709.png]]

**Running Apache 7.0.92 on port 8080**

![[Pasted image 20230221165739.png]]

**Goofy ahh webpage on port 80**

![[Pasted image 20230221170438.png]]

Knowing this, lets run a dirsearch on this:

`dirsearch -w /usr/share/seclists/Discovery/Web-Content/raft-small-words.txt -u 10.10.247.142`

Found:

![[Pasted image 20230221170555.png]]

Flag was found encoded in /flag directory:

![[Pasted image 20230221170858.png]]

Plug this into cyberchef and click the wand thing. Tells us this is hex and base64 encoded.

![[Pasted image 20230221170929.png]]

**Found Wordpress Directory**

- ![[Pasted image 20230221171125.png]]

**WPscan found interesting stuff** 

- ![[Pasted image 20230221171207.png]]

- Pwnkit
		- Vulnerability which allows users to run the PolicyKit executable pkexec, passing it a specific set of environment varialbes can allow unprivileged local users to get full root privileges.

make script that is downloadable which automatically kills a certain process every coupel of seconds.

```
#!/bin/bash

#Check if Process ID is provided 
if [$# -ne 1]; then
	echo "Usage: $0 <Process ID>"
	exit 1
fi

#Set the Process ID to a variable
PID=$1

#Check if the process with the given PID exists
if ps-p $PID > /dev/null; then
	echo "Process with PID $PID exists."

#Loop to kill the process every minute
	while true; do
	  kill $PID
	  echo "Process with PID $PID was killed."
	  sleep 60 #kills every 60 seconds
	done
else
	echo "Process with PID $PID does not exist."
fi
```

***2/24/23***

- Ports 22,111,80,21

![[Pasted image 20230224232746.png]]

***Executing Sudo -l***

- Shows us we have sudo privs on the `pico` command.

![[Pasted image 20230225003250.png]]


![[Pasted image 20230225002127.png]]

![[Pasted image 20230225003022.png]]

We found we were able to view the sudoers file by executing 

`sudo pico /etc/sudoers`

This allowed us to see the sudoers file above, where I then added group `%fortuna ALL=(ALL:ALL) ALL` in order to give myself permission to execute any command as root.

![[Pasted image 20230225010200.png]]

We can see in this file that we are given sudo privileges with /usr/bin/pico (through the pico binary)
Then, we can execute `sudo su` and we are root!

![[Pasted image 20230225010140.png]]

OR

We could have done this, and specified that we can run ALL commands.

![[Pasted image 20230225010649.png]]

![[Pasted image 20230225010656.png]]

**Found NFS on port 2049**


`sudo showmount -e 10.10.98.167` 

![[Pasted image 20230225003840.png]]

shows us a mount

Then we could create a directory `mnt`...

`sudo mount -t nfs 10.10.98.167:/ ./mnt -o nolock`

This command would run for us to be able to download the a share, which contained a SSH key.

![[Pasted image 20230225004051.png]]

This is encrypted with ROT13, so we throw it into cyberchef and get the actual key.

If receive error 'bad permissions', chmod 600 the file.



![[Pasted image 20230225005632.png]]

Run the ssh command with the unencrypted key once you have the password.

**Getting password from ssh key** 

- `ssh2john <keyname>`

- This will give you an encrypted  password

Then...

`john w=/usr/share/wordlists/rockyou.txt <decryptedpassword.key>`

 ![[Pasted image 20230225005814.png]]

We will then get the password needed to login using ssh.

![[Pasted image 20230225005954.png]]

***2/25/23 Hogwarts***


10.10.147.61

- Found webiste port 9511

![[Pasted image 20230225192613.png]]]

/usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt]

Foudn this base64 encoded perm on port 22:index=5:

ZW5iaTJsYWszdg== 


- Told we can submit a resume of max file size 1000000
![[Pasted image 20230225200516.png]]

we abused magic bytes in order to add magic bytes to the first thing the backend would check, allowing us to run a php shell.

1. Downloaded a random pdf
2. did `head -n1 <pdfname> > magicpdf.pdf`
3. cat the produced file to get  some magic bytes
4. Place this at the start of the payload
5. Make sure the content type is php so that the shell executes

- PHP CODE CANNOT BE EXECUTED UNLESS IT IS CALLED BY A PHP EXTENSION OR PHP-ADJACENT EXTENSION

