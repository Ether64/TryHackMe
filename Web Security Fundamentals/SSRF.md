
-  **Server-Side-Request-Forgery** is a vulnerability that allows a malicious user to cause a webserver to make additional or edited HTTP request to whatever resource an attacker wants to access

![[Pasted image 20230301202618.png]]

- One of the implementations is where an attacker forces the webserver to request a server of the attackers choice.
		-  This allows us to capture request headers that are setn to the attacker's specified domain, which could  contain authentication credentials or API keys sent by website.thm.

**Spotting SSRF**

- When a full URL is used in a parameter in the addresss bar.
![[Pasted image 20230301204254.png]]
- When there si a hidden field in a form that allows input of URL
![[Pasted image 20230301204300.png]]
- when there is a partial URL that includes just the hostname (api)![[Pasted image 20230301204152.png]]

![[Pasted image 20230301204305.png]]

**Blind SSRF**

- Case where no output is reflected back to us; requires us to use an external HTTP logging tool to monitor requests (requestbin.com), or your own HTTP server or Burp Collaborator

