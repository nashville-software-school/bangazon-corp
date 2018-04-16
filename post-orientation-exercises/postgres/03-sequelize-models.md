# Models
Models are a representation in our code of how our data is structured in our database.

When using Sequelize, creating a model with the CLI tool using `model:create` will actually generate 2 files for you: a model file and a corresponding migration file. We will deal only with the model and ignore the heck out of that migration file.

For our sandcastles project we will need some sandy locations to choose from, so start with a beach model:

`sequelize model:create --name Beach --attributes name:string,location:string,sand_rating:integer`. This will create `beach.js` in the models folder for us.

```js
'use strict';
module.exports = (sequelize, DataTypes) => {
  var Beach = sequelize.define('Beach', {
    name: DataTypes.STRING,
    owner: DataTypes.STRING
  }, {});
  Beach.associate = function(models) {
    // associations can be defined here
  };
  return Beach;
};
```

Awesome, right? 

That empty object right before the `.associate` property is where you can include options like `timestamps: false` and define your preferred table name (`tableName: "beaches"`) if you want to keep it lowercase.

```js
'use strict';
module.exports = (sequelize, DataTypes) => {
  var Beach = sequelize.define('Beach', {
    name: DataTypes.STRING,
    owner: DataTypes.STRING
  }, {timestamps: false, tableName: "beaches"});
  Beach.associate = function(models) {
    // associations can be defined here
  };
  return Beach;
};
```

Now that `associate` has been added as a property of a Beach, what the heck is it? `associate` is your key to defining relationships, or associations, between models. So, if a `Beach` is related to a `Lifeguard`, you define that here. If a beach has many lifeguards, and a lifeguard works at only one beach, then that relationship can be defined like so:

```js
// models/beach.js
...

Beach.associate = function(models) {
  Beach.hasMany(models.Lifeguard, {
    foreignKey: 'beach_id' //the same foreign key must be specified when you define the association on the lifeguard model.
  });
};

...
```
(When naming `foreignKey`s in your models, think of the 'hasMany' as sending the foreignKey, and the 'belongsTo' as receiving it.)
( For insight into how and when `associate` gets called, look at models/index.js )

How can this model be useful? We can use the Beach model to easily create and add properties to a new beach instance without writing raw sql. Sequelize models are build using ES6 Class syntax. Don't worry that you haven't written Classes before. A JS Class is just syntactic sugar on top of javascript objects that delegate values and behavior via the prototype chain. Just keep in mind that every time you save a new user, song, todo, beach, or lifeguard to a database, you are creating a new instance of a User/Song/Todo/Beach/Lifeguard model.

```js
let awesomeBeach = new Beach();
awesomeBeach.set('name', 'Papohaku Beach');
awesomeBeach.set('location', 'Molokai, HI, USA');

awesomeBeach.save()
.then(function(beach) {
  console.log('Beach saved:', beach.name);
});
```

Or, if you have an object already created that you want to save as an instance of your model, you can do this, using a shortcut method, `create()`, which builds a new model instance and calls `save()` on it for you:
```js
let awesomeBeach = {
  name: "Papohaku Beach",
  location: "Molokai, HI, USA",
  sand_rating: 8
}

Beach.create(awesomeBeach)
.then(function(beach) {
  console.log('Beach saved:', beach.name);
});
```

Notice that all of these methods return promises.

In addition to a host of built in [query methods][query methods], You can also add custom methods and relationships to your models. If a Lifeguard model has a `first_name` and a `last_name` property, grab a Lifeguard instance's full name like so:

```js
// models/lifeguard.js
...

Lifeguard.prototype.getFullName = function() {
  return `${this.first_name} ${this.last_name}`
}

...
```
Note this method is defined on the model's prototype, which makes the method available to all instances of a Lifeguard. This is referred to as an instance method. Learn more [here](https://stackoverflow.com/questions/1635116/javascript-class-method-vs-class-prototype-method)

Since we defined the relationship that a Beach has with a Lifeguard, we can get a beach's lifeguards in a single query, like so:

```js
Beach.getOne({name: 'Papohaku Beach', include: [{model: Lifeguard}] })
.then(function(beach) {
    console.log('Got beach:', beach) //Will include the beach's properties and a list of all Lifeguards who have that beach's id on them
});
```
### Next Steps
Before moving on to the next exercise, create a Beach and a Lifeguard model with the CLI tool and update each in the current required syntax, as mentioned above

### Resources
[Sequelize data types](http://docs.sequelizejs.com/variable/index.html#static-variable-DataTypes)
[Your first sequelize model](http://docs.sequelizejs.com/manual/installation/getting-started.html#your-first-model)
[Sequelize associations](http://docs.sequelizejs.com/manual/tutorial/associations.html)

[query methods]: http://docs.sequelizejs.com/manual/tutorial/models-usage.html
