
- Subdomain Enumeration involves finding valid subdomains for a domain, which if found, allows us to find more points of vulnerability in the web application.

- Three Subdomain Enumeration Methods:
		- Brute Force
		- OSINT
		- Virtual Host


**OSINT - SSL/TLS Certificates**

- Certificate Authorities create an SSL/TLS certificate for a valid domain.
- In this process, they create Certificate Transparency (CT) logs.
	- These are publicly accessible logs of every SSL/TLS certificate that was created for a given domain.

- Certificate Tranparance logs is to stop malicious and accidentally crafted certificates from being used. 
		- But, it can also be used to discover subdomain belonging to a domain.

- https://crt.sh and https://ui/ctsearch.entrust.com/ui/ctsearchui offer searchable databases of certificates that shows current and historical CT logs.

- ![[Pasted image 20230224193104.png]]

As you can see, we can input the domain name and we will get a bunch of results for certificates that were provided at given dates.


**DNS Bruteforcing**

- Bruteforce DNS enumerations involves trying hundreds of different possible subdomains from a pre-defined list of commonly used subdomains.
- `dnsrecon` can be used to perform this.
- `dnsrecon -t brt -d <domain.<rootdomain>`

**Sublister**

Combines checking SSL certificates, Search Engine queries, and OSINT sites such as Virustotal and PassiveDNS to automatically find unique subdomains for a domain.
[[Sublist3r]]

**Virtual Hosts**

- Some subdomains aren't hosted in publicly accessible DNS results- could be development versions of a web application or adminstration portals.

- If DNS records are kept on a private DNS server or recorded on a developer's machine in the /etc/host file, we need to find another way to find subdomains.

- We move our attention to the Host Header, which is requested by the client when the client tries to connect to one of many websites hosted on a web server.
		- Can utilize the **host header** by making changes to it and monitoring the response to see if we've discovered a new website.
		- This process can be automated usign wordlists.

- We can use FFuF to automatically brute force check host headers:

`ffuf -w /usr/share/wordlists/seclists/Discovery/DNS/namelist.txt -H "Host: Fuzz.acmeitsupport.thm" -u http://MACHINE_IP` 

Read more here: [[FFuF]]