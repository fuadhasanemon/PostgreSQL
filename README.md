# PostgreSQL

![PostgreSQL](https://zdnet2.cbsistatic.com/hub/i/r/2018/04/19/092cbf81-acac-4f3a-91a1-5a26abc1721f/thumbnail/770x578/5d78c50199e6a9242367b37892be8057/postgresql-logo.png)
<h1> What is PostgreSQL</h1>

<p>PostgreSQL which is also known as Postgres, is a free and open-source relational database management system developed by a worldwide team of volunteers. It is claimed to be the most advanced open source database solution. It provides an implementation of the SQL querying language. It runs on several platforms including Linux, UNIX, Mac OS X, Windows, True64 and Solaris.

This documentation demonstrates the installation of Postgresql on Windows 10 Pro and instructions for basic database administartion along with mentioning the key basics. The possible problems you can face while executing the administration, will also be mentioned.</p>

<h1> Installation of PostgreSQL on Windows 10 pro</h1>

At first,download the PostgreSQL from [here](https://www.postgresql.org/) For downloading, follow the below steps:

1. Click on Download.
2. Click on Windows.
3. Click on Download the Installer.
4. Download the desired version as per your System Type(64 bit or 32 bit).

<P>After downloading, complete the setup of the downloaded .exe file just like any other programs. Provide a password for Superuser while doing the setup and remember this password must. Finish the setup. A SQL Shell(psql) and a graphical user interface pgAdmin4 will be installed for operating the database. In this demonstartion, using of SQL Shell(psql) will be in focus. Click the psql application to launch it. The psql command-line program will display. Enter all the necessary information such as the server, database, port, username, and password. To accept the default, press Enter. Provide the password that you entered during installing the PostgreSQL.</p>


```> Server [localhost]:
> Database [postgres]:
> Port [5432]:
> Username [postgres]:
> Password for user postgres:
> > psql (12.3)
> WARNING: Console code page (850) differs from Windows code page (1252)
         8-bit characters might not work correctly. See psql reference
         page "Notes for Windows users" for details.
> Type "help" for help.

> postgres=#
```

<h1>Adding PostgreSQL to Path variable</h1>

To add PostgreSQL to the path we have to copy the path to PostGreSQL installation directory to the path variable.
* First we have to copy the path to the bin folder in the installation directory.

![Copy this folder url](\Users\User\OneDrive\Pictures\Screenshots\1.png)

#Starting postgresSQL
* To start/stop/control the server I'll be using pg_ctl.

#Using the CLI
<p> psql is the PostgreSQL interactive terminal. To run the interactive shell, type </p>

#psql postgres 
<p> It will promt a new cli. The postgres is a default database that came with the installation. 

To check if everything is alright, we can check the version by typing </p>

#select version();
#and the output should be similar to

<p> PostgreSQL 12.3 on x86_64-apple-darwin19.5.0, compiled by Apple clang version 11.0.3 (clang-1103.0.32.62), 64-bit
(1 row)
Creating a user
syntax

# CREATE USER 
name [ [ WITH ] option [ ... ] ]
options are </p>
```
    SYSID uid 
    | CREATEDB | NOCREATEDB
    | CREATEUSER | NOCREATEUSER
    | IN GROUP groupname [, ...]
    | [ ENCRYPTED | UNENCRYPTED ] PASSWORD 'password'
    | VALID UNTIL 'abstime'
```   
Lets create a user who can login and create database and interact with them.

CREATE ROLE $username WITH LOGIN PASSWORD = 'password-goes-here';
example

CREATE ROLE plaban WITH LOGIN PASSWORD = 'secret';
Creating a database
create DATABASE $dbName;

# example 
create DATABASE test_db;
```
the output should be
```

# CREATE DATABASE
Creating a database with specific owner
syntax

# CREATE DATABASE 
```
$DB_NAME WITH OWNER $username;
```
# example

``` CREATE USER project_db WITH OWNER Fuad;
Listing all databases
To list all database in the interactive shell, type \l, it is not a SQL command so no need to use semi colons.
```
#  Listing all databases

Switching databases
To swithc to database, type \c $dbname to start using the database. For example \c test_db. The output should be similar to

You are now connected to database "test_db" as user "prophecy".
Again, this is not a SQL command, so no need to add semi-colons.

So now we are in our practice database, let's create some tables. We'll start with colours table.

Deleting a database
DROP DATABASE $database_name;
Connecting to database
The command to connect to a database is

psql -h $host_ip_or_domain -p $port -U $username -W $dbname

# using -W (capital W) will ask for a password, if you don't have any password use -w 
So in my case, (password is ommited as my default password blank/I don't have a default password.)

psql -h localhost -p 5432 -U username dbname
Creating a table
To create a table, our syntax is


`code` CREATE TABLE $tableName (
	$column_1 $data_type $optional_meta_information, 
	$column_2 $data_type $optional_meta_infromation,
	$column_N $data_type $optional_meta_information,
	$table_constraints
);
Lets start simple, So our command will be

CREATE TABLE colours (
	id int PRIMARY KEY NOT NULL, 
	name varchar(20) NOT NULL, 
	hex varchar(7)
);
`code`
To know if it worked, we can check tables, by typeing \dt while we are in the database. List of all tables

Inserting data
As of now, we have our table and we can store some data in it. To store data, we use a insert query. The insert query is structured as follows

INSERT INTO $table_name ($column_1, $column_2, ... $column_N) VALUES ($value_of_column_1, $value_of_column_2, ... $value_of_column_N);
This will insert a single row. If we want to insert multiple rows at once, we can append to the values.

INSERT INTO $table_name ($column_1, $column_2, ... $column_N) VALUES ($row_1_column_1, $row_1_column_2, ... $row_1_column_N),($row_2_column_1, $row_2_column_2, ... $row_2_column_N),($row_N_column_1, $row_N_column_2, ... $row_N_column_N);
Example
So for creating a single row

INSERT INTO colours (id, name) VALUES (1, 'red');
For inserting multiple rows

INSERT INTO colours (id, name, hex) VALUES (2, 'green',''), (3, 'blue','3399ff');
The output should be INSERT 0 2 or similar.

To see if everything worked correctly, we can list all our records from our table by using the SELECT command.

SELECT * FROM $table_name;
So our command will be,

SELECT ALL FROM colours;
The result should be a list data. Selecting all records from our colours table. Now, with the * , we have selected all the columns. To select a specific column, we can specificy them in place of asterisk.

SELECT column_a, column_b FROM $table_name;
So our example will be

SELECT name, hex from colours;
Returning specific columns

Selecting record/s
syntax

SELECT * FROM $table_name;
# or, if you want to be specific with column names.
SELECT $column_name_a, $column_name_b,... $column_name_n FROM $table_name;
example

SELECT * FROM cars;
result enter image description here

Updating a record
syntax
