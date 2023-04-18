- Web applications are programs that run persistently on a remote server.
- Most of the time, they are joined with a database server that serves data about products that corresponded to the hosted website.
- Web applications may even have multiple databases to split corresponding sets of data between functional queries.

- ![[Pasted image 20230203180100.png]]

- Web applications are accessed through a browser.

- Web applications can be exploited at the login phase using password attacks.
- - Can also be exploited by command injection into the search query that will be executed when looking for a product in the backend databases.
- Payment details can be uncovered by man in the middle attacks or by cracking payment details sent in weak encryption.

**Insecure Direct Object Reference**

- This type of attack occurs when an object or page can be accessed by any user typing in the respective item's id in the browser search bar.

Ex: `https://store.tryhackme.thm/products/product?id=52` 
will allow just any user to access and display an item that is currently unavailable, confidential, or out of stock.

This can be translated into accessing other user accounts.

Ex: `https://store.tryhackme.thm/customers/user?id=16`

Now we can assume the users will have sequential iD numbers, so a user can try to access many other accounts until they stumble upon, say, id=73, who is an admin user.