# Bangazon Orientation API

## Requirements

1. Using Express you will be building the initial, open API for Bangazon.
1. Expose the following resources in your API.
    1. Customer
    1. Order
    1. Product
    1. Department
    1. Computer
    1. Training Program
    1. Product Type
    1. Payment Type
    1. Employee

The initial version is for internal consumption only, so it does not require permissions to access, or manipulate, any resources.

## Resources

As you progress through the project, your manager will discuss some of these topics with you.

* Seeding your database with [fixtures](\)

# Creating a database
To build the API, you will need a database for it to read from and write to, with customer information, payment data, a collection of products, and a record of customer orders.

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


