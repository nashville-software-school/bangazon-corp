# SQLite Practice

In this exercise you will create a SQLite database, create tables, insert records to the table, and query the database. We will be using the npm module [sqlite3](https://www.npmjs.com/package/sqlite3).

This exercise is based on the [SQLite Intro Exercise](12-SQLite_create_db.md), so make sure that you have completed it before attempting this exercise.


## Setup

```
cd workspace/node/exercises
mkdir sqlite101
cd sqlite101
npm install sqlite3 --save
touch sqlite3.js
```

## Instructions

A friend of yours owns a small family business and wants to start moving all of their business records into a database. Using your NodeJS skills, they want you to create a SQLite database to store information about their employees.

1. Create a database that is saved on disk.

1. Create a table titled `employees` with the following columns:
  - id, firstName, lastName, jobTitle, address

1. Create an array of at least 6 objects. Each object should have a key value pair matching each column name in the `employees` table.
  ```js
  eg: let array = [
    { id: 0, firstName: 'Fred', lastName: 'Smith', jobTitle: 'Cashier', address: '500 Somewhere Lane' },
    ...,
  ]
  ```  

1. Insert each of the employee objects into the database.

1. Write a statement to query the database and `console.log()` all employee records.

1. Write a statement to query the database and `console.log()` each employees `jobTitle`.

1. Write a statement to query the database and `console.log()` each employees `firstName`, `lastName` and `address` only.

## Bonus Features

1. Instead of using an array in the .js file, create a JSON file of employees to require into the js file. Use this to populate the table.

1. Your friend has decided that they want to add a salary column to the employees table. Make sure to add a salary key value pair to each of the employee objects. Then drop the existing employees table, update the schema to accept a salary column, and repopulate the table.

1. Write a statement that returns all employees of a specified `jobTitle`.
