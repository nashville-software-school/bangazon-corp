# Creating a database
<!-- TODO describe that this is an intro to the first sprint project -->
## Introduction
Now that you have some experience with writing ERDs for, and querying, an existing db, and you've tried your hand at the commands for initializing a new db from scratch, it's time to design and build a db you will actually use. The group project for this milestone will need to read and write to a database of customer information, payment data, a collection of products, and a record of customer orders.

Start with a robust ERD that clearly establishes the relationships between your tables, and populate each entity with the rows needed to organize your data efficiently. Only then should you move on to creating your tables in a SQLite document.

## Requirements
Use SQLite and the sqlite3 module to create your tables. These tables are required:

**customers**
This table will store the following information
+ A unique customer id (integer).
+ All information collected about the user. In the group project you will prompt the user for the folowing info:
    + customer name
    + street address
    + city
    + state
    + postal code
    + phone number

**payment_options**
This table will contain the following information
+ A unique payment option id (integer)
+ Payment option name
+ Payment option account number

**products**
This table will store the following information
+ A unique product id (integer)
+ Product name
+ Product price

**orders**
This table will store the following information
+ A unique order id (integer)
+ The order's customer id
+ The order's payment option id
+ Whether the order has been paid in full

**order_line_items**
This table will store the following information
+ A unique line item id (integer)
+ The order id
+ The product id

Plan to leave enough time to test drive your db with as many queries as you can. Soon we will dive into writing tests that confirm that we can read and write to our databases properly. For now, just write out a series of queries like the ones you worked through in the previous exercises and use the DB Browser app to put your tables through their paces.

