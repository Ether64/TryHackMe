
- Content Discovery can be performed on web applications in three manners:
		- Manually
		- Automatically
		- OSINT

- This describes how we find files, videos, pictures, backups, administrative panels, etc. in a public hosted webpage.

**Manual Discovery**

- We can look for documents such as the Robot.txt file being present on the website directory.
	- This file tells search engines what pages are allowed to show on the search engine.
	- Gives us a list of locations on the website that the owners wouldn't want us to discover as pentesters.

![[Pasted image 20230224171623.png]]
- As can be seen in this example, the staff-portal directory is noted to be disallowed.

![[Pasted image 20230224171745.png]]

**Favicons**

- Favicons are part of the installation that is left over after a framework is used to build a website.

- OWASP hosts a database of common framework icons that you can use to check against found favicons.
https://wiki.owasp.org/index.php/OWASP_favicon_database

- In this sense, we are able to determine the framework stack, and then use external resources to discover more about it.
		- certain versions of vulnerable webapps will use favicons; if nothing is leaking what the website (lets say its Wordpress) is, the favicon might leak that.

Ex: ![[Pasted image 20230224173619.png]]

This webpage includes a favicon which we can download using `wget` or `curl` and piping that into `| md5sum` .

- f276b19aabcb4ae8cda4d22625c6735f

If we get an md5 such as this, you can then google it to find out what framework it belongs to. 
https://github.com/andresriancho/w3af-module/blob/master/w3af-repo/w3af/plugins/infrastructure/favicon/favicon-md5

![[Pasted image 20230224174453.png]]

**Sitemap.xml File**

- The **sitemap.xml** file gives a list of every file the website owner wishes to be listed ona search engine.
		- Sometimes contains areas of the website that are difficult to navigate or are deprecated but still working behind the scenes.

- ![[Pasted image 20230224174931.png]]

- This is what the /sitemap.xml directory looks like.

**Using HTTP Headers to Manually Discover**

- When requests are sent to the web server, the server returns HTTP headers, which can oftentimes contain useful information about the software the webserver is running, and the programming language it engages.

Ex:
![[Pasted image 20230224175120.png]]

We can see a `curl` command can be ran with the `-v` verbose mode tag in order to see some rudimentary HTTP headers regarding the 10.10.254.155 destination.

**Locating the Framework's Website**

- Once we know what the framework used to create the website was, we can locate the framework's website in order to learn more about the software that was used in the development process.

- ![[Pasted image 20230224175502.png]]
- We can see through these comments that the framework used to build the page was THM Framework v1.2, which is navigable at /thm-web-framework.

- ![[Pasted image 20230224175624.png]]
Viewing the Documentation portal gives us the path to the login page and the credentials used to login.
![[Pasted image 20230224175658.png]]

![[Pasted image 20230224175822.png]]
We can login here with the provided credentials.


**OSINT - Google Hacking/DORKING**

- Dorking is an OSINT technique that uses Google's search engine features, allowing us to sift out custom content.

	Ex: `site:tryhackme.com admin` is a query that returns results only from the specified website (tryhackme.com) which contain the word admin in their content.

**Filters**

![[Pasted image 20230224180058.png]]
![[Pasted image 20230224193608.png]]

**OSINT - Wappalyzer**

- https://www.wappalyzer.com/  **Technology Profiler** 

- Wappalyzer is an online tool/browser extension that helps identify what frameworks, CMS, payment processors, and functions a website uses... and much more.

Extension Function: ![[Pasted image 20230224180342.png]]

**OSINT - Wayback Machine**

- The **Wayback Machine** is a historical archive of websites that preserves their versions and iterations all the way back to the late 90s. 
	- Helps uncover old pages that may still be active on the current website.
	- Also shows how many times a service scraped the web page and saved the contents when given a valid domain name.

- [https://archive.org/web/](https://archive.org/web/) 

**OSINT via S3 Buckets**

S3 Buckets are a storage service provided by Amazon AWS, allowing people to save files and even static website content in the cloud accessible over HTTP and HTTPS. The owner of the files can set access permissions to either make files public, private and even writable. Sometimes these access permissions are incorrectly set and inadvertently allow access to files that shouldn't be available to the public. The format of the S3 buckets is http(s)://**{name}.**[**s3.amazonaws.com**](http://s3.amazonaws.com/) where {name} is decided by the owner, such as [tryhackme-assets.s3.amazonaws.com](http://tryhackme-assets.s3.amazonaws.com/). S3 buckets can be discovered in many ways, such as finding the URLs in the website's page source, GitHub repositories, or even automating the process. One common automation method is by using the company name followed by common terms such as **{name}**-assets, **{name}**-www, **{name}**-public, **{name}**-private, etc.

**Automated Discovery**

- Automated discovery is the process of using tools to discover content using thousands of requests to a web server.
		- The requests check whether a file or directory exists on a website, by checking with wordlists.

**Automation Tools**

-` ffuf`: fast web fuzzer written in Go which allows you to discover directories.
- `dirbuster`: multi-threaded java application designed to brute force directories and file names.
- `gobuster`: tool used to brute force URLs including directories, files, and DNS subdomains.


FFuF Syntax:
`ffuf -w /usr/share/wordlists/SecLists/Discovery/Web-Content/common.txt -u http://10.10.254.155/FUZZ`

Dirb Syntax:
`dirb http://10.10.254.155/ /usr/share/wordlists/SecLists/Discovery/Web-Content/common.txt`

Gobuster Syntax:
`gobuster dir --url http://10.10.254.155/ -w /usr/share/wordlists/SecLists/Discovery/Web-Content/common.txt`