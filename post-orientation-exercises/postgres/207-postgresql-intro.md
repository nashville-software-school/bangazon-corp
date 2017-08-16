# PostgreSQL

PostgreSQL is a powerful, open source object-relational database system. As a database server, its primary function is to store data securely, and to allow for retrieval at the request of other software applications.


# psql

The primary front-end for PostgreSQL is the `psql` command-line program, which can be used to enter SQL queries directly, or execute them from a file. We will use `psql` to get started.

To start using the program, make sure the PostgreSQL server is running. If it is, there should be an elephant icon in your toolbar. To open the `psql` interface, click on the elephant icon and select `Open psql`. This will open a new window in your terminal with `psql` running.

You should see something like this:
```
psql (9.6.0)
Type "help" for help.

username=#
```

Helpful hints/commands for `psql`:
- \h : list of all SQL commands available
- \? : list of all psql commands available
- \q : quit and exit psql
- be sure to end all SQL statements with a semi-colon


### Creating a Database with psql

First, let's get familiar some basic commands. Run the following command in your prompt: `username=# \l`

This will output a list of all of your PostgreSQL databases.

```
                        List of databases
  Name    |  Owner   | Encoding | Collate | Ctype |   Access privileges   
-----------+----------+----------+---------+-------+-----------------------
postgres  | postgres | UTF8     | C       | C     |
template0 | postgres | UTF8     | C       | C     | =c/postgres          +
          |          |          |         |       | postgres=CTc/postgres
template1 | postgres | UTF8     | C       | C     | =c/postgres          +
          |          |          |         |       | postgres=CTc/postgres
(3 rows)
```

Let's create a new database and call it `testdb`. Run the following command:

`username=# CREATE DATABASE testdb;`

Now that `testdb` has been created, running the `\l` command should output a list of databases with the newly created database `testdb` included.


### Connecting to a Database

In order to query a database, first you will need to connect to it. `testdb` will be the database that will be connected to for this example. Run the following command to connect to it:

```
username=# \c testdb;
// OUTPUT => You are now connected to database "testdb" as user "yourusername".
```

Now that there is a connection, tables can be created.


### Creating Tables

Once a database has been created and you are connected, it is as simple as writing raw SQL statements to create tables, insert rows, and to query.

For example, the following statement with create a table named `foods`:

`username=# CREATE TABLE foods (id INT, foodgroup TEXT, name TEXT);`

To list all tables associated with the connected database run:

```
username=# \d

// OUTPUT =>
        List of relations
Schema | Name  | Type  |     Owner      
--------+-------+-------+----------------
public | foods | table |    username
(1 row)
```

You can also list a specific table:

```
username=# \d foods

// OUTPUT =>
      Table "public.foods"
Column   |  Type   | Modifiers
-----------+---------+-----------
id        | integer |
foodgroup | text    |
name      | text    |
```

Using `psql` is as simple as that! Now that you have the power to create tables, try inserting rows, updating columns, and querying data.


### Additional Resources

[PostgreSQL Tutorial](https://www.tutorialspoint.com/postgresql/index.htm)
[PostgreSQL Exercises](https://www.pgexercises.com/)
