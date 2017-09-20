# Creating a database

## Introduction
Now that you have some experience with writing ERDs for, and querying, an existing db, and you've tried your hand at the commands for initializing a new db from scratch, it's time to design a db you will actually use. The first product you will build with your teammates a few days from now will be an API to be used as an internal tool here at Bangazon, corp. Of course, the API will need data to pull from and add to, and this exercise is your first step toward building that database by creating an ERD for it. Once your team is assembled to build the API the team will choose which of your ERDs best maps out the structure of the database. (_Don't tell anyone, but I think yours will be the winner_)

## Instructions
### ERD

Build an ERD to define the properties of the following resources and the relationships between them. There are some properties defined for each resources, but they are the bare minimum. Use your own life skills and knowledge to add appropriate properties to each one where you see fit.

> **Technical Note:** These are the main resources that are needed in the database and is *not* an all-inclusive list of tables that should be created.

#### Employees
* An employee can only be assigned to one department
* An employee can sign up for one, or more, training programs

#### Departments
* A department has a supervisor, which is a specific kind of Employee
* A department has an expense budget

#### Computers
* Track when a computer was purchased by the company
* Track when a computer was decommissioned by the company
* A computer can only be assigned to one employee at a time

#### Training Programs
* A training program must have a start date
* A training program must have an end date
* A training program must have maximum attendees specified

#### Product Types
* These are categories of Products

#### Products
* A product can only have one type
* A product has a price
* A product has a title
* A product has a description
* Products will be created by customers, so make sure that you have the appropriate column on the Product table

#### Orders
* A customer can only have one active order at a time

#### Payment Types
* A customer can have multiple payment types (Visa, Amex, bank account, etc)
* A payment type must have an account number
* An order must be given a payment type before it is complete, but it not needed when order is created

#### Customers
* A customer's first and last name should be recorded separately
* The date that a customer created an account must be tracked
