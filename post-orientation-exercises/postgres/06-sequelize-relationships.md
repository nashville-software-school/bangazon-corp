# Sequelize Associations, or how to deal with bridge/associative/pivot/join/thingamajig tables and foreign key relationships

Remember way back in the mists of time, last month, when you learned about creating many-to-many relationships
with a join table? Here's how to set up that relationship using Sequelize. Let's pretend we have a user who can select from a list of TV shows and add them to a collection of favorites. A show can have many users select it as a favorite, and users can have many favorite shows. We would represent that process with `User` and `Show` models that each declare a `belongsToMany` relationship in their `associate` methods.

```js
// models/user.js
...
User.associate = (models) => {
  User.belongsToMany(models.Show, {
    as: 'Favorites', // declares an alias for a Show when used in the context of a User's list of favorites
    through: 'UserFavorites' // This is required, as it defines the join table name you will use
  });
};
...
```
A very similar setup is required in the Show model
```js
// models/show.js
...
Show.associate = (models) => {
  Show.belongsToMany(models.User, {
    through: 'UserFavorites'
  });
};
...
```

This will automatically create the tables when you run your db generation file, but if you want to seed the join table, it will require an extra step.
The bulkCreate method can only run on tables that have a model defined in the models directory, so in order to seed it, we'd have to create a model for it.

If we want to allow duplicates of the same relationship, we'll also need to specify `unique: false` and `constraints: false`. This isn't useful with users and favorite shows, but it might come in handy with other things later...

```
Show.belongsToMany(models.User, { 
      through: {
        model: 'UserFavorites',
        unique: false
      },
      foreignKey: 'show_id',
      constraints: false
    });
```

### Exercise
+ Create a many-to-many relationship in your sandcastle code. Maybe update your Lifeguard model so that lifeguards can work at many beaches around town. Or add sandcastle types, and many kinds of tools can be used to build many kinds of sandcastles. Don't be afraid to commit. Dive into a new relationship!
