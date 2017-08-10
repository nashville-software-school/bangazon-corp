# SQLite

SQLite is a software library that implements a self-contained, serverless, zero-configuration, transactional SQL database engine.

What does that mean?

1. SQLite does not require a separate server process or system to operate.
1. Creating a SQLite database instance is as easy as opening a file.
1. The entire database instance exists in a single cross-platform file.


## Setup

Feel free to follow along with this exercise, or you can read through it and use it as a reference for the [corresponding exercise](13-SQLite_create_db_02.md). Make sure to run the following commands if you are coding along.

```
cd workspace/node/exercises
mkdir sqlite101
cd sqlite101
npm install sqlite3 --save
touch sqlite3Intro.js
```


## sqlite3

sqlite3 is an npm module that provides a software interface with a SQLite database. With sqlite3 and NodeJS you have the ability to create tables, drop tables, make inserts, query a SQLite database, and more.


### Creating a SQLite Database

```js
// Require in the Database method from the sqlite3 module
// We will be using the verbose execution mode, which will help with debugging errors.
const { Database } = require('sqlite3').verbose();

// Returns a new database object and automatically opens the database
// Database method accepts a callback function for successful connection
const db = new Database('example.sqlite', () => console.log('Connected!'));
```


### Creating a database

```js
// Creates a database which will be written to a file on disk
// Changes will persist once connection closes
const db = new Database('db/example.sqlite');
```


### Creating a Table

Now lets create a table with some columns. We will use `db.run()` to execute a SQL statement.

```js
// Passing in IF NOT EXISTS after CREATE TABLE will check to make sure there are no tables named 'employees'
// If it does exist, this line will not run
db.run("CREATE TABLE IF NOT EXISTS employees (id INT, first TEXT, last TEXT)");
```

The above statement will do the following:

1. Create a table named `employees`
1. Create three columns named `id`, `first`, and `last`
1. Specify the data-type of the value for each column. Even if there is a specified data-type, a value of any data-type can still be stored in the column.
  - Constraints can be passed in to restrict certain data-types
  - eg: `first TEXT NOT NULL` will accept any value not equal to null


### Simple Insert Statement

Lets insert out first records into the `employees` table.

```js
db.run("INSERT INTO employees (id, first, last) VALUES (1, 'Michael', 'Scott')");
// employees TABLE
// id |  first    |   last
//  1 | 'Michael' | 'Scott'

db.run("INSERT INTO employees VALUES (2, 'Jim', 'Halpert')");
// employees TABLE
// id |  first    |   last
//  1 | 'Michael' | 'Scott'
//  2 | 'Jim'     | 'Halpert'
```

The above statements may use different syntax, but they will both insert a record with `id`, `first`, and `last` values into the table. Omitting the `(id, first, last)` from the first statement will not change the outcome, but make sure that the values (eg: `(0, 'Michael', 'Scott')`) are in the **same order as they were defined when the table was created.**


### Dynamic Inserts with JavaScript

First lets create an array of objects. Each object will hold information about an employee.

```js
const employeeArray = [
  { id: 3, firstName: 'Dwight', lastName: 'Schrute' },
  { id: 4, firstName: 'Andy', lastName: 'Bernard' },
  { id: 5, firstName: 'Pam', lastName: 'Beesly' }
];
```

Now we can iterate over the `employeeArray` and insert each employee to the database.

```js
employeeArray.forEach((obj) => {
  // Using ES6 string templating, we can create an insert statement for each object
  db.run(`INSERT INTO employees VALUES (${obj.id}, '${obj.firstName}', '${obj.lastName}')`);
});

// employees TABLE
// id |  first   |   last
//  1 | 'Michael'| 'Scott'
//  2 | 'Jim'    | 'Halpert'
//  3 | 'Dwight' | 'Schrute'
//  4 | 'Andy'   | 'Bernard'
//  5 | 'Pam'    | 'Beesly'
```


### Querying the Database

Now that the table we created has records, let's retrieve the data and view it. We can use `db.all()` to do this. There are other ways to retrieve data, check out the [sqlite3 API docs](https://github.com/mapbox/node-sqlite3/wiki/API) for more information.

```js
// db.all() first runs the query
// then calls a callback function which it passes all resulting rows
db.all("SELECT * FROM employees", (err, allRows) => {
  // allRows is an array containing each row from the query
  allRows.forEach(each => {
    console.log(each.id, each.first + ' ' + each.last);
  });
});
// OUTPUT =>
// 1, 'Michael Scott'
// 2, 'Jim Halpert'
// 3, 'Dwight Schrute'
// 4, 'Andy Bernard'
// 5, 'Pam Beesly'
```


### Error Handling

Each of the database methods can receive an optional callback function, and this is where we can check for errors. Uncaught errors will be logged to the console, but so will the entire stack trace, which can be difficult to read.

We can create a custom function which can console log custom errors.

```js
// errorHandler is a function which accepts an error object
const errorHandler = (err) => {
  if (err) { // If there is an error obj, it will be console logged
    console.log(`Msg: ${err}`);
  };
};
```

Now we can pass our `errorHandler` function into the database methods as a callback to check for errors.

```js
// If an error is thrown when this statement is ran, our function
// will log it to the console
db.run("INSERT INTO employees VALUES (2, 'Jim', 'Halpert')", errorHandler);


db.all("SELECT * FROM employees", (err, allRows) => {
  errorHandler(err); // We can also check for errors here

  // <-- Do something with results from database -->
});
```


### Closing the Database

Once all statements have been executed, the database connection should be closed. We can do that with `db.close()`. Errors can also be listened for as well as a successful close event with a callback function.

```js
db.close(err => {
  errorHandler(err); // Use custom error handling function
  console.log('Database closed'); // Will only log on successful close
})
```
_____
### Additional Resources

###### [SQLite Website](https://www.sqlite.org/)
######  [SQLite Tutorial | Language Reference](https://www.tutorialspoint.com/sqlite/index.htm)
##### [sqlite3 API docs](https://github.com/mapbox/node-sqlite3/wiki/API)
