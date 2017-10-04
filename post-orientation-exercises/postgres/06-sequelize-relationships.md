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

Part two involves the creating of a schema in a migration file that allows us to save our `Favorite` instances.
This migration file needs to be created using `sequelize migration:create --name user-favorites`. Unlike a migration created automagically by Sequelize when a model is created, this file will contain only the empty `up` and `down` functions. You will need to fill them in with the schema properties for the join table:

```js
// xxxxxx-user-favorites.js
module.exports = {
  up: function(queryInterface, Sequelize) {
    return queryInterface.createTable('UserFavorites', {
      id: {
        allowNull: false,
        autoIncrement: true,
        primaryKey: true,
        type: Sequelize.INTEGER
      },
      UserId: {
        type: Sequelize.INTEGER,
        allowNull: false,
        references: {
          // Sequelize is referencing the table here, which it capitalizes by default, so use the plural of User
          model: 'Users',
          key: 'id'
        },
        onUpdate: 'cascade',
        onDelete: 'cascade'
      },
      ShowId: {
        type: Sequelize.INTEGER,
        allowNull: false,
        references: {
          model: 'Shows',
          key: 'id'
        },
        onUpdate: 'cascade',
        onDelete: 'cascade'
      },
      // these are optional, but you have to tell Sequelize you don't want them in the model. See the docs
      createdAt: {
          allowNull: false,
          type: Sequelize.DATE
      },
      updatedAt: {
        allowNull: false,
        type: Sequelize.DATE
      }
    })
  },
  down: function(queryInterface, Sequelize) {
    return queryInterface.dropTable('UserFavorites');
  }
};
```

You are now ready to run your migration command to create the join table.

### Exercise
+ Create a many-to-many relationship in your sandcastle code. Maybe update your Lifeguard model so that lifeguards can work at many beaches around town. Or add sandcastle types, and many kinds of tools can be used to build many kinds of sandcastles. Don't be afraid to commit. Dive into a new relationship!
