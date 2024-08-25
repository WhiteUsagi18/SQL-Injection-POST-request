# SQL Injection
Here in LEKIR, we have a challenge that vulnerable to SQL injection. We can manipulate the input fields to execute arbitrary SQL code on a database.

![image 1](./img/image1.png)

## Identifying Vulnerabilities
First, we need to identify if this page is vulnerable to sql injeciton.<br><br>
In this case, we already have the source code. View the source code and search for the line that handle the SQL query. Usually the developer will named the variable with $query or $q.

![image 2](./img/image2.png)

Based on how the code wrote, we know that the page is vulnerable to sql injection.
Another way to identify is by inserting a special character like ' or " in the input field. Then you will see there is an error respond on the page.

![image 3](./img/image3.png)

> [!NOTE]
> There will be some situation that will return the blind error which will not giving us any error respond on the page after you inserted the character. This will make it difficult for us when we want to identify if the page is vulnerable or not.
