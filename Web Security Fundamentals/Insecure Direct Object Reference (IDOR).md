
- Insecure Direct Object Reference is a vulnerability that occurs when a web server receives a user-supplied input to retrieve objects (files, data, documents).
		- In this case, too much trust has been placed on the user's inputted data, and it is not validated on the server-side to confirm that the object the user requested actually belongs to them.

- Ex: A wepage allows the user to see the query produced for their information:

**http://online-service.thm/profile?user_id=1305

- The user could then infer to change the user_id value to 1000, which wold result in them being swithced to another users account where they can view their information. 

This is an IDOR vulnerability. There should have been a check on the website to confirm that the user information belongs to the user that was seen requesting it.

![[Pasted image 20230225162952.png]]

**Side-Channel Scenario/Bypassing Encoding**

Additionally, user data passed through post data will often be encoded. (usually base64 for websites)

This too, can be exploited by attackers by decoding the encoded post data, altering it, and then encoding it again for submission to the web server.

![[Pasted image 20230225163306.png]]

**Finding IDORs using Unpredictable IDs**

- Another way to detect IDORs is to create two accounts the swap the Id numbers between them.

- If we are able to view the other users' content using their ID number while we are still logged into another account, we can say we've found an IDOR vulnerability.

**Where are IDORs found**

- IDORs are not always visible in the address bar; can be caused
	- via loaded in content via an AJAX request or something that is referenced in a JavaScript file.
	- by unreferenced parameters that have been deprecated yet still reveal user information during calls to certain sites.

***Viewing a Practice IDOR Example**

- Navigating to a mock website and created an account allows us to see the following developments that occur in teh background.

![[Pasted image 20230225180438.png]]

Opening up developer tools and viewing the network traffic allows us to see the page returnning in JSON format our assigned ID, username, and inputted email address. This occurs when clicking on the 'Your Account' tab.

This information is taken from the query string ID parameter GET request:


![[Pasted image 20230225180618.png]]

Viewing the 'Header' tab, we can click the Resend button to open up a panel to send another request wherein we alter the 'id' attribute to test for IDOR:

![[Pasted image 20230225181257.png]]
![[Pasted image 20230225181306.png]]

![[Pasted image 20230225181333.png]]
Then click send button at bottom to send this message.

Now, looking at the network flow in developer tools shows another GET request, formed with the `customer?id=1` query string.

![[Pasted image 20230225181427.png]]
Viewing the response to this query shows the following:

![[Pasted image 20230225181448.png]]

We just found out that the user with customer id 1 has a username of **adam84** and email of **adam-84@fakemail.thm!

