
- In a scenario in which our host client is attempting to connect to *tryhackme.com*... 

	- 1. Request for *tryhackme.com* is sent by browser.
	- 2. Our host checks Local  Cache for the respective IP address of *tryhackme.com*.
	- 3. Our host checks its respective recursive DNS Server for the IP address of *tryhackme.com*.
	- 4. If can't be found through steps #2 and #3, host machine queries the root server to find the authoritative DNS Server. 
	- 5. Once located, the Authoritative DNS Server advises the IP address for the website (ex: 192.168.202.23)
	- 6. The GET Request first passes through a Web Application Firewall (WAF).
	- 7. Then it passes through a Load Balancer that controls the web traffic that goes through the web application by diffusing it to multiple servers.
	- 8. Finally, a conection is established with the webserver on port 80 or 443.
	- 9. Then, the web servers receives the HTTP GET request.
	- 10. Then, the Web application will perform whatever ooperations it is directed to perform on the database.
	- 11. Host client receives the retrieved HTML (from a database or such) to render the website.