# Using Sequelize.Sync()

Next, we're going to make a function that will build our database tables based on the models we have generated with Postgres. With this, we can sidestep the migrations.

First we'll create a file that will manage the db build process. Then, we want to require in both sequelize and queryInterface (which is part of sequelize).
```
let sequelize = require('sequelize');
let queryInterface = require('sequelize/lib/query-interface');
```

Now, we can write a function that will dig through the models and create the tables we've defined there.
```
let createdb = (queryInterface) => {
  const app = require('./app'); //make sure you are exporting app after attaching the necessary pieces in app.js!
  const models = app.get('models');
  return models.sequelize.sync({ force: true })
    .then((queryInterface) => {
        //soon we will include how to add stuff to the tables here!
    })
    .catch((err) => {
      console.log("oh noes!", err);
    })
}
//don't forget to actually call the function. Or, bonus, rewrite it as an iife.
createdb(queryInterface);
```

That method, `models.sequelize.sync({ force: true })` is a powerful tool that will take care of all the steps of dropping, creating, and (coming soon) populating tables with seed data that you have so far been hand-rolling with various levels of success. It forces the process to happen in order and for each step to wait for the appropriate time. 

In the terminal where you run this db building file, you'll see a log of the steps the method goes through.

When we check the tables in the terminal where we are running `psql`, we will see that they exist with the properties we laid out in the model, along with the properties added for us (the PK id, and a property for `created at` and `updated at`).

## Exercise
We're finally ready to create a database and fill it with useful tables.
Tailor your beach and lifeguard models to have the properties you want, and to have a relationship to each other. Feel free to create a few more related entities as well.
Avoid many-to-many relationships for the moment; we'll add those after we seed our data tables.
Run the file that builds your tables, then use psql to check to see that they have been generated (and are empty). Check especially for their foreign keys!



[Sequelize Sync Options](http://sequelize.readthedocs.io/en/latest/api/sequelize/#sync)
