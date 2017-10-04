# Migrations

Although Sequelize represents a new way for you to create and interact with an SQL db, up to this point it most likely has seemed familiar in its use of models and associations, which you've had a chance to experiment with in SQLite. Now comes a new concept: migrations.

According to [Wikipedia](https://en.wikipedia.org/wiki/Schema_migration)"A schema migration is performed on a database whenever it is necessary to update or revert that database's schema to some newer or older version. Migrations are performed programmatically by using a schema migration tool." (BTW, a schema indicates which tables or relations make up the database, as well as the fields included on each table)

In other words, migrations allow us to create our database tables and then track changes to the database structure in our javascript files via timestamped JavaScript files that automagically execute in the proper order in order to maintain order in the universe. :) Need to change the structure of a table? Or add a many-to-many relationship that wasn't there in the early stages of development? No worries. A new migration file with the revised table information does the trick. A quick command via the Sequelize CLI tool to execute the migration files will seamlessly reconfigure the database.

To use migrations with Sequelize, you will again to the Sequelize CLI tool you installed earlier

#### Creating a migration

The migration file(s) Sequelize creates for you include two functions that look like this:

```js
module.exports = {
  up: (queryInterface, Sequelize) => {
    // logic for transforming into the new state
    // If created at the same time as a model, this block will contain the properties defined in the model,
    // plus 'createdAt' and 'updatedAt' properties
  },

  down: (queryInterface, Sequelize) => {
    // logic for reverting the changes
  }
}
```
Migrations are pretty awesome because they run two ways. Each migration you write should do something (in `exports.up`) that will happen when you run your migrations, and also UNDO that same thing (in `exports.down`) that will run when you rollback your migration.

When you run `sequelize db:migrate`, a table is added to your database for each migration file, and then when you run `sequelize db:migrate:undo:all`, that table is removed from your database.

By tracking the changes of your migration files in github, you can keep track of the changes you've made to your database over time.

## Exercise
We're finally ready to create a database, fill it with useful tables, and then knock the whole thing down again.









`sequelize migration:create --name create_beaches`, define a `beaches` table in the exports.up, and then drop the table in exports.down. Beaches have names, unique id's, locations, and any other descriptive columns you feel like adding.

Now- run `sequelize db:migrate` and check to see if your "beaches" table has been added to your database, using pgAdmin or your psql terminal.

It looks awesome? Let's get rid of it! `sequelize migrate:undo:all` and check to make sure that table is gone.

All done!
Just kidding...

Try it again. Make another migration to add a second table to your database. How about some tools to build sandcastles with? Run `sequelize db:migrate` and `sequelize migrate:undo:all` a couple of times to get the hang of it.

### Bonus
1. Create another migration, but instead of a whole new table, this migration should add a new column to your existing beaches table.
1. Create yet another migration that adds a lifeguards table to your database. The lifeguards should have names and should have a one to many relationship with your beaches.

[Sequelize Migrations](http://docs.sequelizejs.com/manual/tutorial/migrations.html)
