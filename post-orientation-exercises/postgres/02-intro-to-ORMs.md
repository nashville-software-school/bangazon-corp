# ORMs

According to wikipedia, ORM (object-relational mapping) "is a programming technique for converting data between incompatible type systems in object-oriented programming languages." In plain English, we use ORM tools to get our databases to talk to our code, even though the database speaks SQL (or Mongo) and our code is written in JavaScript.

Sequelize is a powerful Node.js ORM that makes it easier to work with relational data. It provides easy access to MySQL, SQLite or PostgreSQL databases by mapping database entries to objects and vice versa. Moving forward, we will be using Sequelize with PostgreSQL.

### Queries

So far, we've only used raw SQL statements -- either in DB Browser (oops, can't use that anymore) or wrapped in sqlite3 methods -- to query our databases. With the addition of Sequelize we now have another option that, in most cases, abstracts away the SQL. For example, `SELECT * FROM orders WHERE id = 27` using Sequelize would look like this: `Order.findById(27)`.

Plus, Sequelize gives us other super powers, like the ability to create custom query methods and add validation to our models that help sanitize the data before it's added to the database.

In general, your preference should be to write your queries in Sequelize, only using raw SQL when Sequelize won't accomplish what you need to do.

The next few exercises will get you up and running with using Sequelize to interact with PostgreSQL in a Node.js app. So, let's get Sequelize ready to roll on your machine.

### Setup
First, install the CLI tool globally: `npm install -g sequelize-cli`

Then, let's escape all the pressure and stress of NSS by building some sandcastles on a sunny beach. Create a directory called `sequelize_sandcastles` and cd into it. Check this out: `mkdir sequelize_sandcastles && cd $_`
Oooh, nifty. Learn something new every day, right?

Then `npm init` and install some dependencies
`npm install --save sequelize`
While you're at it, throw an installation of PostgreSQL ( `pg` ) in there too. ( As of this writing, you may or may not need an additional installation of the `pg-hstore` module. Your intrepid instructors are keeping abreast of the latest from Sequelize Central. Doesn't hurt to include it for now )

Launch postgres, then using psql in a new terminal window/tab, `CREATE DATABASE sandcastledb;`
HINT: don't forget the ";" at the end of the statement, and avoid capital letters.

Next, a quick `sequelize init` at the root of your project will create the basic folder/files you'll need. Your file structure should like something like this

```
├── config
│   └── config.json
├── migrations
├── models
│   └── index.js
└── package.json
└── seeders
```

Open up `config/config.json` in your editor. Here, you will see three configurations for three different environments: development, testing, and production. That's cool. Now you can mess around with building db in development mode, run tests against a whole other version in test mode, and keep a clean version, free from fake data and db-breaking experiments, when running in production.

For simplicity's sake, just worry about changing the default settings in `development` to match your project's info. You won't need to worry about `username` or `password`. Feel free to delete those.

Example
```js
{
  "development": {
    "database": "sandcastledb", //the database you created with psql or in the PostgreSQL GUI
    "host": "127.0.0.1",
    "dialect": "postgres"
  },
  ... //this ellipse basically means etc, BTW. You'll see it a lot in code examples. Remember, we're lazy, and proud of it
}
```
Now you're ready to
+ Define the entities your db will store, as models
+ Configure the tables that will hold instances of those models by creating migration files
+ Seed the db with some placeholder data

The next resource files will walk through those steps.

### Resources
[ORM Wikipedia page](https://en.wikipedia.org/wiki/Object-relational_mapping)
[Sequelize](http://sequelize.readthedocs.io/en/1.7.0/)
[Sequelize: Getting Started](http://docs.sequelizejs.com/manual/installation/getting-started.html)
