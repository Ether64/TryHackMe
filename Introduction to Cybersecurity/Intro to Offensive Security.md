
- `Gobuster` is a tool that can be used to find hidden pages and directories within a website.

- With this first room, we are presented with a mock website called fakebank.com.

`gobuster -u http://fakebank.com -w wordlist.txt dir` 


- The `-u` switch in gobuster is used to specify the website we intend to scan, while the `-w` specifies the wordlist we will use to iterate a variety of hidden pages. 

- Gobuster will display status codes related to 200 of pages that it has found.
![[Pasted image 20230203165413.png]]

- As we can see, GoBuster, has found the /bank-transfer page.

- ![[Pasted image 20230203171137.png]]

- Cool we found what looks like an admin portal; THM tells us to transfer  $2000 from a bank account 2276 to our bank account (8881) as a proof of functionality.

![[Pasted image 20230203172617.png]]

![[Pasted image 20230203172728.png]]

Peakington. We can see this worked as we have a $2000 transaction posted to our bank account ending 8881.

- This was a short exercise to explain what offsec is; *offensive security is the process of breaking into computer systems, exploiting software bugs, and finding loopholes in applications to gain unauthorized access to them*. 



