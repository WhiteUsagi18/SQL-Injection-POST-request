# SQL Injection
Here in LEKIR, we have a challenge that vulnerable to SQL injection. We can manipulate the input fields to execute arbitrary SQL code on a database.

![image 1](./img/image1.png)

## Tools
- Burp Suite
- SqlMap

## Identifying Vulnerabilities
First, we need to identify if this page is vulnerable to sql injeciton.<br><br>
In this case, we already have the source code. View the source code and search for the line that handle the SQL query. Usually the developer will named the variable with `$query` or `$q`.

![image 2](./img/image2.png)

Based on how the code wrote, we know that the page is vulnerable to sql injection.
Another way to identify is by inserting a single quote `'` in the input field. Then you will see there is an error respond on the page.

![image 3](./img/image3.png)

> [!NOTE]
> There will be some situation that will return the blind error which will not giving us any error respond on the page after you inserted the character. This will make it difficult for us when we want to identify if the page is vulnerable or not.

## Intercept the POST Request
Open your burp suite and turn on the intercept in proxy tab. Go back to your target site and enter anything in the input field. After you click on the submit button, burp will catch the **POST request** and wait.

![image 4](./img/image4.png)

Copy the POST request to a text file. Create your text file manually or using the following commands: <br>

Creating the text file<br>
`
touch post-request.txt
`

Open text editor and paste the copy<br>
`
nano post-request.txt
`

> [!NOTE]
> Don't forget to save before you exit.

## Implementing SQL Injection

Run sqlmap as shown below; the option `-r` tells sqlmap to read the `post-request.txt` file to get the information to attack in the POST request. `-p` is the parameter we are attacking. `--dbs` will show the databases that are available. <br>The parameter that we are attacking is `user_id`.<br>

`
sqlmap -r post-request -p user_id --dbs
`

![image 5](./img/image5.png)

![image 6](./img/image6.png)

We can see that there is a database named lekir which refers to my target site. So now, I want to look for the tables that avaible in this database. Since we know the name of the db, we can use this option `-D` follow by the name of the db to select the db. `--tables` is to show the tables that are avaible in this db
<br>

`
sqlmap -r post-request -p user_id -D lekir --tables
`

![image 7](./img/image7.png)

The `secret` table looks suspicious. let's take a look inside it.

`
sqlmap -r post-request -p user_id -D lekir -T secret --columns
`

![image 8](./img/image8.png)

Now, we can see that there are two columns in this table which are `secret_id` and `secret_data`. Lets dump the `secret_data` column to see what the data is.<br>

`
sqlmap -r post-request -p user_id -D lekir -T secret -C secret_data --dump
`

![image 9](./img/image9.png)

Oh no! the text was encrypted. But it's okey, we can decrypt this if we know the type of encryption. Copy the encrypted text and paste it in any online cipher detector.<br>

`
https://www.boxentriq.com/code-breaking/cipher-identifier
`

![image 10](./img/image10.png)

We now know the text was encrypted using base64. Go to any online base64 decoder and paste the encrypted file.

![image 11](./img/image11.png)

YAY! Now we can read the text.
