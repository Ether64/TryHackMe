
- Attack on web application database servers that cause malicious queries to be executed.

**Relational vs. Non-Relational Databases**

- Relational database stores information in tables in which the tables often have shared information between them. 
- Tables often contain a column with a unique ID primary key, that will be used to reference tables together and cause **relationships** between them.

- **Non-relational databases** are any sorts of databases that don't use tables, columns, or rows to store data.
	- They don't have a specific database layout so each row of data contains different information which provides more flexibility over relational databases, but less structure.
			- Ex: MongoDB, Cassandra, ElasticSearch

**Practical Example**

- Let's say a website contains many blogs, and has the following URL query for every single blog entry:

	`https://website.thm/blog?id=1`

- The related SQL statement to retrieve the article from the database and display it on the user's page could be something like this:

	`SELECT * from blog where id=1 and private=0 LIMIT; `

Knowing this, we could input into the URL an injection like this:


`https://website.thm/blog?id=2;--`

- Here, the two dashes cause everything after to be treated as a comment, so the backend SQL query would look like this:

	`SELECT * from blog where id=2;-- and private=0 LIMIT; 1`

This will return the article by executign the SQL query `SELECT * from blog where id=2;--`


**Gathering information about database**

- `0 UNION SELECT 1,2,database()` 

This will display a database name if the database table we are accessing has 3 columns.

![[Pasted image 20230301233337.png]]



**Gather list of tables that are in database**

![[Pasted image 20230301233443.png]]

`0 UNION SELECT 1,2,group_concat(table_name) FROM information_schema.tables WHERE table_schema= 'sqli_one``

- The **group_concat()** method gets the specified column (table_name in this case) from multiple returned rows and puts it into one string.

- The **information_schema** database is something that all users have access to, and it contains information about all the databases and tables a user has access to.
		-  We are listing all the tables in the `sqli_one` database.
			- We find that there are tables `article` and `staff_users`
- 