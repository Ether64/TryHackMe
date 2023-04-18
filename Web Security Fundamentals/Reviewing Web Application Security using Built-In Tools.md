
- We can use our built-in browser tools to look for security vulnerabilities in websites:
	- **View Source**
	- **Inspector**
	- **Debugger**
	- **Network**

Lets begin with the following page:

![[Pasted image 20230223160954.png]]

In general...

- `/` represents the home directory of the webpage.
- `/news` would direct to a page that contains a list of recently published news articles.
- `/news/article?id=1` is a web query that would display the individual news article with an id of 1.
- `/contact` would direct the user to a page that contains a form for customers to contact the company. Contains name, email, and message input fields most likely.
- `/customers` is a page that redirects customers to `/customers/login`.
- `/customers/signup` is a page that contains a user sign up form.
- `/customers/account` would present a page that allows the user to edit their account credentials.


**Viewing The Page Source** 

- The **Page source** is the human-readable code that is returned to our client from the web server whenever we make a request to a webpage.

- Of course, the code that is returned to our browser or client is in the form of HTML, CSS, and JavaScript, and it is what tells our browser what content to display, and how to do it with elements of interactivity.

Right-click on the page -> View Page Source in order to view the html of the page and more.
		- You can also place `view-source` in front of the URL, like so: `view-source:https://www.google.com/`.

Ex: Right-clicking the above page and selecting `View page source` presents the following html:

![[Pasted image 20230223161956.png]]

If we look closely, we can see many `<a href>` attributes which denote a page link. There are links to pages 'Contact', 'Customers', 'News', etc.
	- Knowing this, we can assume we can simply navigate to these in our browser by adding the `/contact` directory to our full search.

- ![[Pasted image 20230223162408.png]]

- We can even see there is a secret-page included as a relative link.


- Another thing to look out for in html code, is if the **framework** that the webpage was generated using has details provided about it.

![[Pasted image 20230223162834.png]]
From viewing the page source of the exaple page, we can see we are told the THM Framework v1.2 was used.

**Looking out for external file locations**

- In the example above, we find out that all CSS, JS, and html elements are all bundled into an `/assets` directory.

- When viewing this directory, there should be a blank or **403 Forbidden Page** error displayed, but in this case, the directory listing feature has been enabled, so we can see all source code, confidential information, and potentially backup information in a real-world scenario.

![[Pasted image 20230223163533.png |Its Important to make sure proper directory permissions are set]] 


***Developer Tools - Inspector***

- Every modern browser includes developer tools that are tool kits used in debugging web aplpications.
	- Obviously, as a pentester we can use these tools to provide us with a better understanding of a web application, and how we may exploit it.

- The **Inspector** developer tool allows us to view whats being displayed in the browser window in terms of what is currently on the website.

- Press F12 or Right-Click -> Inspect to open up **Inspector**

![[Pasted image 20230223164113.png]]

We can hover over elements and see the html correlation be highlighted on the page in real-time.
![[Pasted image 20230223164320.png]]
As we can see here, we can even highlight elements that would otherwise be covered by some html element (which would be removed in the case of you buying into a premium version to remove the content blocker).

Hovering over the "premium-customer-blocker" while in Inspector allows us to see what html elements make up this blocker. 
![[Pasted image 20230223164512.png]]
![[Pasted image 20230223164522.png]]

- If we look at the CSS element `display: block;`, we can assume that changing this CSS to `none` will unblock the premium blocker element.

![[Pasted image 20230223164640.png]]

This works, after the changing the CSS like so:

![[Pasted image 20230223164655.png]]

So, as we can see, **Inspector** can enable us to change html and CSS elements in realtime.

***Developer Tools - Debugger*** 

- The **Debugger** panel is used for debugging javascript, and can afford us a deepr insight into JavaScript code.

- ![[Pasted image 20230223165223.png]]

Allows us to see the directories and resources that the current page uses in terms of JavaScript.

![[Pasted image 20230223165247.png]]

Clicking the `{}` button near the bottom right of the debugger panel will make the javascript more readable.

![[Pasted image 20230223165325.png]]

Now we can have a better idea of what the JavaScript is doing and how it is structured.

**Breakpoints**

- Clicking the line number that a specific javascript code is located on allows us to place a breakpoint.
	- Breakpoints can be used to force the browser to stop processing the JavaScript and pause the current execution at a certain specified point.

Ex:
![[Pasted image 20230223165523.png]]

I have placed a breakpoitn at line 108, which contains the Javascript `flash()` function.

![[Pasted image 20230223165551.png]]

And so, when we refresh the page, this happens:

![[Pasted image 20230223165614.png]]

As we can see, the execution of the javascript stopped up until line 108, so we see the flag before it is removed by the `flash['remove']();` code.

***Developer Tools - Network***

- The **Network** tab is used to keep track of every external request a webpage makes. 

- We can use this to see where our input is sent after clicking a `Send Message` or `Submit` button.

![[Pasted image 20230223165938.png]]

We can see the webpage reaching for the contact, jquery.min.js,site.js, etc. elements whenever sesnding a GET request to the domain that hosts the website (10-10-228-71.p.thmlabs.com).

- Clicking on the element gives us more granular data regarding the requests headers and the relevant response headers:
![[Pasted image 20230223170149.png]]

Interestingly, filling in the input fields located on the /contact page and then clicking the 'Send Message' button produces the following external request:

![[Pasted image 20230223170308.png]]

This is displayed as an XHR request.
	- Stands for HMLHttpRequest which is a Javascript API to create AJAX requests.

- `AJAX` is a method for sending and receiving network data in a web application background, without changing the current web page.

Using Network, we can see where this messagte was sent to, and what data was included in the response:

![[Pasted image 20230223170701.png]]

Sent to https://10-10-228-71.p.thmlabs.com/contact-msg

Response:

![[Pasted image 20230223170738.png]]

Request:

![[Pasted image 20230223170801.png]]

- We can navigate to where the request was sent, to check out what would otherwise be some confidential information:

![[Pasted image 20230223171123.png]]