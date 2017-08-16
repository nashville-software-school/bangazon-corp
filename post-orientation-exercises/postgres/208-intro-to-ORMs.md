<!-- Review-ready -->

# ORMs

According to wikipedia, ORM (object-relational mapping) "is a programming technique for converting data between incompatible type systems in object-oriented programming languages." In plain English, we use ORM tools to get our databases to talk to our code, even though the database speaks SQL (or Mongo) and our code is writted in JavaScript.

Sequelize is a powerful Node.js module that makes it easier to work with relational data. It provides easy access to MySQL, MariaDB, SQLite or PostgreSQL databases by mapping database entries to objects and vice versa.

### Queries

So far, we've only used raw SQL to query our database. With the addition of Sequelize we now have another option for how to query the database.  In general, your preference should be to write your queries in Sequelize, only using raw SQL when Sequelize won't accomplish what you need to do.

The next few exercises will get you up and running with using Sequelize to interact with PostgreSQL in a Node.js app.

### Resources
[ORM Wikipedia page](https://en.wikipedia.org/wiki/Object-relational_mapping)
[Sequelize](http://sequelize.readthedocs.io/en/1.7.0/)
[Sequelize: Getting Started](http://docs.sequelizejs.com/manual/installation/getting-started.html)
