## Using Sequelize for migrations

### Migrations

Have you noticed yet how wonderful github is? And have you also noticed how difficult it is to track your database in github? Migrations to the rescue! Migrations allow us to create our databases and then track changes to the database structure in our javascript files.

According to [Wikipedia](https://en.wikipedia.org/wiki/Schema_migration)"A schema migration is performed on a database whenever it is necessary to update or revert that database's schema to some newer or older version. Migrations are performed programmatically by using a schema migration tool." (BTW, a schema indicates which tables or relations make up the database, as well as the fields included on each table)

In order use migrations with Sequelize, you need to install its command line interface globally `npm install sequelize-cli -g`. Since this is a global install, you will only need to so once.

Like with our dear friend npm you can run `sequelize init` to create some boilerplate files to get you up and running. <!--TODO. More sqlize stuff --> The XXX will connect our app to our database and will sepcify any settings we want to use. The XXX assumes three different environments for your app: testing, development, and production. We will primarily be working with testing and development. Why use a different environment for testing? Well, this way we can have a seperate databse used for testing that we can keep clean so that our tests are run the way we expect. (i.e. no users enter extra data between the time we write our tests and when we run them)

After that, you can create migrations <!--`sqlize command blah`-->, run migrations <!--`sqlize stuff` -->, and rollback migrations <!-- `sqlize rollback stuff`-->.

So, what does all that stuff *mean*?

#### Creating a migration

The migration file(s) Sequelize creates for you include two empty functions.
Looks like this:

```
module.exports = {
  up: (queryInterface, Sequelize) => {
    // logic for transforming into the new state
  },

  down: (queryInterface, Sequelize) => {
    // logic for reverting the changes
  }
}
```
Migrations are pretty awesome because they run two ways. Each migration you write should do something (in exports.up) that will run when you run your migrations, and also UNDO that same thing (in exports.down) that will run when you rollback your migration.

For example, I could create  a simple table in my exports.up  like so:
```
<!-- SQLize it -->
<!-- exports.up = function(knex, Promise) {
  return knex.schema
    .createTable('monsters', function(table){
      table.increments('monster_id').primary();
      table.string('monster_name').notNullable();
    })
}; -->
```

and then my exports.down would look like so:
```
<!-- SQLize it -->
<!-- exports.down = function(knex, Promise) {
  return knex.schema
    .dropTable('monsters')
}; -->
```

When I run <!-- `knex migrate:latest` -->, a monsters table is added to my database, and then when I run <!-- knex migrate:rollback -->, that table is removed from my database.

By tracking the changes of my migration files in github, I can keep track of the changes I've made to my database over time.

## Exercise

Let's create a database, fill it with useful tables, and then knock the whole thing down again.

First create a directory for your knex sand castle and cd into it
```mkdir knex-sandcastle && cd $_```

then `npm init` and install some dependencies
`npm install --save pg knex`

using your psql console, `CREATE DATABASE sandcastledb;`
HINT: don't forget the ";" at the end of the statement, and avoid capital letters.

`knex init`, and update your knexfile.js to use your sandcastledb.

`knex migrate:make add_first_table`, put a table in the exports.up, and then drop the table in exoprts.down. What kind of table? How about some monsters (monsters like attacking castles). Monsters have names, unique id's, and any other descriptive columns you feel like adding.

Now- run `knex migrate:latest` and check to see if your "monsters" table has been added to your database using pgAdmin or your psql terminal.

It looks awesome? Let's get rid of it! `knex migrate:rollback` and check to make sure that table is gone.

All done!
Just kidding...

Try it again. Make another migration to add a second table to your database. How about some heroes to fight off those monsters? Run knex migrate:latest and knex migrate:rollback a couple of time to see how the two different migration intereact with each other and the databse.

***now and forever*** whenever you make a change to your database, you can migrate:latest, you can migrate:rollback, or you can migrate:make new_migration. Do not change a migration file that has already been made, or you will be sad 😢.

### Bonus
1. Create another migration that adds a new column to your hero table
1. Create yet another migration that adds a weapons table to your database. The weapons should have names and should have a many to many relationship with your heroes.

[Sequelize Migrations](http://docs.sequelizejs.com/manual/tutorial/migrations.html)
