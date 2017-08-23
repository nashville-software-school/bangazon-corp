# Sequelize Relationships, or how to deal with pivot tables and foriegn key relationships
<!-- TODO: Sequelize -->
Remember way back in the mists of time, three weeks ago or so, when you learned about creating many-to-many relationships with bridge/associative/pivot/join/thingamajig tables? (One of those terms is not in common use. Extra credit if you guess which one)

Here's how to set up a many-to-many relationship using Knex/Bookshelf. Let's pretend we have a user who can select from a list of TV shows and add them to a collection of favorites. We would represent that process with `User` and `Show` models, as well as a `Favorite` model. `User` and `Show` would have a many-to-many relationship, as a show can have many users and users can have many shows.

```
const Show = bookshelf.Model.extend({
  tableName: 'shows',
  fans: function() { return this.belongsToMany(User).through(Favorite)}
});

const User = bookshelf.Model.extend({
  tableName: 'users',
  favoriteShows: function() { return this.belongsToMany(Show).through(Favorite) }
});

//The pivot / associative / junction / bridge table
const Favorite = bookshelf.Model.extend({
  tableName: 'favorites',
  user: () => belongsTo(User),
  show: () => belongsTo(Show)
});
```

With Bookshelf we describe that relationship as key value pairs on the models, with the key being the other model's name or a word that describes how the other model relates. In this example we think of a show as having `fans` and a user as having `favoriteShows`. The value of those keys is a function that calls Bookshelf methods `belongstoMany()` and `through()`.

We pass `Favorite` into `through()` to link `User` and `Show` to the `Favorite` model. Each instance of `Favorite` will represent one favorite show of a single user.

Note that in this case we gave our join table model a unique name, `Favorite`, but you will often see the name as simply a combination of the two related models. So, instead of `Favorite` you would possibly see `UserShow` or `User_Show`.

Part two involves the creating of a schema in knex that allows us to save our `Favorite` instances in a `favorites` join table in our db.
```
exports.up = function(knex, Promise) {
  return knex.schema.createTable('favorites', (table) => {
    table.increments().primary();
    table.integer('user_id').unsigned().references('users.id')
    table.integer('show_id').unsigned().references('shows.id')
  });
};
```
`primary()` is a method that is equal to using `('id' int unsigned not null auto_increment primary key)` in SQL. And `unsigned()` makes sure the integer is not a negative number. It's also a required method when creating foreign keys.

### Exercise
+ Create a many-to-many relationship in your code for one of the earlier exercises. Maybe a Hero can fight in many Battles, and a Battle can be fought by many Heroes. Or Heroes can use many Weapon types and Weapons can be used by many Heroes.
+ Use knex to create a schema for a join table.
+ Use Bookshelf to define models that have a many-to-many relationship through a new model for the join table schema.
