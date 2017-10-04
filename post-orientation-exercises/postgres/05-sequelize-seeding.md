### Seeding

You've seeded data before, but Sequelize is going to make the process a bit easier (although official documentation for this process is severely lacking as of this writing).

Just like migrations, creating seed files with Sequleize requires the CLI tool. And, similar to migrations, you can create a seed file by running `sequelize seed:create --name my-seed-file`

Sequelize will create a seed file with a timestamp and a `.js` extension. Open it up and it looks like this:
```js
'use strict';

module.exports = {
  up: function (queryInterface, Sequelize) {
    /*
      Add altering commands here.
      Return a promise to correctly handle asynchronicity.

      Example:
      return queryInterface.bulkInsert('Person', [{
        name: 'John Doe',
        isBetaMember: false
      }], {});
    */
  },

  down: function (queryInterface, Sequelize) {
    /*
      Add reverting commands here.
      Return a promise to correctly handle asynchronicity.

      Example:
      return queryInterface.bulkDelete('Person', null, {});
    */
  }
};
```

Update the commented-out example code with the table name you want to populate (change 'Person' to 'Orders', for example). Then pass in an array of objects that you want to insert into the db as the second argument.
```js
const { shows } = require('./data/shows'); //some random json file
return queryInterface.bulkInsert('Shows', shows, {});
```

To seed your database, use `sequelize db:seed:all` to run all the seed files or `sequelize db:seed --seed seed_file_name` to seed one file. With `all`, the migrations are run by timestamps in ascending order. If you rename the files without the timestamps, you can name them in alphabetical order and they will run from a - z.

When working with foreign key relationships, you will want to make sure that your seed fields are named appropriately so that tables that do not require foreign keys get seeded first, and the tables that rely on them are seeded afterward.

## Exercise

Back to the database we made in the previous exercise. Go ahead and run those migrations so that our tables exist, and let's seed them with data.

1. Create a seed file for your beaches and add at least three beaches to your database.
1. Create a new seed file for your sandcastle tools and add some of those to the database.
1. Confirm your seeded data has made it into the database by checking pgAdmin or psql.
1. Create a new table called "castles" that includes a unique id, a description, a tool id (foreign key), and a beach id(foreign key).
1. Create and run a seed file to seed your new castles table.
1. Have fun at the beach.

### Bonus
1. Seed your lifeguards table.
1. Create a simple Express.js app (No need to setup controllers, router modules, etc. Just build some routes right in app.js with `app.get('/beaches', ( req, res, next)), etc )
1. After creating your Express server, fire it up and create at least three new beaches and four new lifeguards, and add them to your database with Postman
1. Using a beach's id, console.log its data along with the data of any lifeguards who work there
