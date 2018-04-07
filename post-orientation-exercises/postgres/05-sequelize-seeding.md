### Seeding

You've seeded data before, but Sequelize is going to make the process a bit smoother.

When working with foreign key relationships, you will want to make sure that your seed fields are named appropriately so that tables that do not require foreign keys get seeded first, and the tables that rely on them are seeded afterward.

## Exercise

Back to the database we made in the previous exercise. Using the same file where we run `sync`, we'll also fill our tables with some seed data.

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
