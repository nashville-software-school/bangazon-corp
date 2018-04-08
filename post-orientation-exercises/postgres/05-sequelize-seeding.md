### Seeding with BulkCreate()

You've seeded data before, but Sequelize is going to make the process a bit smoother.

Inside of the function that we just used to create the tables, we can now insert the necessary method to populate them with some data. This is the bulkCreate method on queryInterface.

As before, you will need a json file where the keys match exactly with the column names of your db table. Import these from wherever they live, then you can bulkCreate them right into the tables.

```
let sequelize = require('sequelize');
let queryInterface = require('sequelize/lib/query-interface');

const { users } = require('./data/users.json')
const { computers } = require('./data/computers.json')

let createdb = (queryInterface) => {
  const app = require('./app');
  const models = app.get('models'); //you will have an 'app.set()' in your app file that sets up the path for the models variable.
  return models.sequelize.sync({ force: true })
    .then((queryInterface) => {
      return models.User.bulkCreate(users);
    })
    .then((queryInterface) => {
      return models.Computer.bulkCreate(computers);
    })
    .catch((err) => {
      console.log("ERRRR", err);
    })
}

createdb(queryInterface);
```


## Exercise

Back to the database we made in the previous exercise. Using the same file where we run `sync`, we'll also fill our tables with some seed data.

1. Create json files for your beaches, lifeguards, and whatever else you are using with your sandcastles. Add at least three beaches to your database.
1. Update and then re-run your db generation file.
1. Confirm your seeded data has made it into the database by checking pgAdmin or psql.
1. Create a new table called "castles" that includes a unique id, a description, a tool id (foreign key), and a beach id(foreign key).
1. Update your db generator to create and seed your new castles table.
1. Have fun at the beach.

### Bonus
1. Create a simple Express.js app (No need to setup controllers, router modules, etc. Just build some routes right in app.js with `app.get('/beaches', ( req, res, next)), etc )
1. After creating your Express server, fire it up and create at least three new beaches and four new lifeguards, and add them to your database with Postman
1. Using a beach's id, console.log its data along with the data of any lifeguards who work there
