# SQL Injection
Here in LEKIR, we have a challenge that vulnerable to SQL injection. We can manipulate the input fields to execute arbitrary SQL code on a database.

![image 1](./img/image1.png)

## Identifying Vulnerabilities
First, we need to identify if this page is vulnerable to sql injeciton.
In this case, we have the source code. View the source code and search for the line that have variable $query or $q.
