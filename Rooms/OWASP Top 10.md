# OWASP Top 10 Vulnerabilities

- Represents the 10 most critical web security risks, as of 2022.

- In general, the top 10 can be broken down into:
	- Injection
	- Broken Authentication
	- Sensitive Data Exposure
	- XML External Entity
	- Broken Access Control
	- Security Misconfiguration 
	- Cross-site Scripting
	- Insecure Deserialization
	- Components with Known Vulnerabilities
	- Insufficient Logging & Monitoring 

**Injection**

- Very common in applications today. These flaws occur because user input ends up being interpreter by commands by an application.
	- SQL Injection: User-controlled input is passed to SQL queries, allowing an attacker to pass in SQL queries to manipulate backend databases and such.
	- Command Injection: This occurs when user input is passed to system commands. Allows the attacker to execute arbitrary system commands on application servers.

- Preventing Injection attacks: 
  - Use an allow list: compare input to a list of safe input/characters.
  - Strip input: remove dangerous characters within input before it is processed.

- Ultimately, cmmand injection opens up many options for the attacker to exploit a machine. Can spawn a reverse shell to become the user that the web server is running as.
	- Ex: ;nc -e/bin/bash\

**Broken Authentication**

- **Authentication** and **session management** are core aspects of modern web applications, and are just as well exploitable components of web applications.

- Authentication begins with a user entering a username and password, in which if correct, the server verifies them and provides the user's browser with a session cookie. Session cookies are needed to make sure the server knows who is sending HTTP(s) data to communicate with a web server. 

- Attacks against Authentication:
	- Brute force attacks
	- Weak credentials (weak password policies for web applications)
	- Weak session cookies: how sever keeps track of users; if session cookies contain predictable values, attacker can set their own session cookies and access the users' accounts.

**Sensitive Data Exposure**

- When a webapp accidentally divulges sensitive data, we call this **sensitive data exposure**.
- Often involves techniques such as a Man in The Middle Attack, where an attacker forces user connections through a device which they control, then take advantage of weak encryption on any transmitted data to gain access to the intercepted information.
- Databases can be also stored as files; can be referred to as flat-file databases. 
- This can lead to a problem, as the database can be downloaded from a directory of a website, and an attacker can query it on their own machine with tons of data exposure.
Accessing a file database: 
![[Pasted image 20220626193258.png]]

Exfiltrating and damping table information:

![[Pasted image 20220626193356.png]] 

Here we can see key extracted fields in the order of custID, custName, creditCard, and password (hashes).

- These password hashes can then be cracked using a tool such as CrackStation.

**XML External Entity**







