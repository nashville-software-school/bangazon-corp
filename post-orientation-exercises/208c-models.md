# Models

Models are a representation in our code, of how our data is structured in our databse.

you: But, my migration files give me all that information!
me: you can create queries based on your models
you: I can run queries without them!
me: but you shouldn't

We will use [Bookshelf](http://bookshelfjs.org/) to create  our models.

So far, in order to run our migrations and seed our databse, we've been using the knex cli to interact with our database, so this will be our first time actually requiring knex and bookshelf in our code.

Since bookshelf relies on knex, we need to do the following:
1. require knex, 
1. pass it our knexfile as the config, 
1. and save it to a variable
1. pass our knex variable to bookshelf, and save *that* as a variable

```
const env = process.env.NODE_ENV || 'development';
const config = require('./knexfile');
const knex = require('knex')(config[env]);
const bookshelf = require('bookshelf')(knex);
```

When you create a model, you use the following format. The only required  property is tableName, so this super-simple model is valid.

```
const Monster = bookshelf.Model.extend({
  tableName: 'monsters'
});
```

But how can this model be useful? We can use the Monster model created with bookshelf to easily create and add properties to a new monster without writing raw sql.

you: I've been waiting months to use constructor functions again!

```
let monster = new Monster();  
monster.set('monster_name', 'Sully');  
monster.set('variety', 'movie character');  

monster.save().then(function(m) {  
    console.log('Monster saved:', m.get('monster_name'));
});

```

You can also add methods and relationships to your models. For example:
```
let Battle = bookshelf.Model.extend({
  tableName: 'battle',
  monster: function() {
    return this.belongsTo(monster);
  },
  hero: function() {
    return this.belongsTo(hero);
  }
},
{
  byLocation: function(location) {
    return this.forge().query({where:{ location: location }}).fetch();
  }
});
```
Note that `forge()` is a Bookshelf method that is shorthand for instantiating a `new fooModel()`  

Now we can run sweet code like this, using our custom method, to see which monster and hero battled at Rhodes
```
Battle.byLocation('Rhodes').then(function(u) {  
    console.log('Got battle:', u.get('monster_id'), u.get('hero_id'));
});
```

and, since we defined the relationship that Battle has with Monster, we can get our monster along with all related battles in a single query, like so:

```
Monster.forge({monster_name: 'Minotaur'}).fetch({withRelated: ['battles']})  
.then(function(monster) {
    console.log('Got monster:', monster.get('monster_name'), monster.get('monster_id'));
    console.log('Got battles:', monster.related('battles').toJSON());
});
```

### Collections

In Bookshelf you also need to create a separate object for collections of a given model. So if you want to perform an operation on multiple Monsters at the same time, for example, you need to create a Collection.

```
//creating collections is easy peasy
var Monsters = bookshelf.Collection.extend({  
    model: Monster
});

//finds all monsters and turns them into JSON
Monsters.forge().fetch().then(function(monsters) {  
    console.log('Got a bunch of monsters!');
    monsters = monsters.toJSON()
    console.log("My awesome json monsters", monsters)
});
```

### Exercise

1. Create an index.js in which you require knex and bookshelf and link them together as shown above
1. Add models for Hero, Monster, and Battle, making sure to specify their relationships
1. Create at least one new hero, one new monster, and one new battle and add them to your database using the models
1. Add a "findByName" method to your Hero model, and use it to query the databse and console.log your hero's id.
1. Using a hero's id, console.log a list of all the battles that your hero has fought.
1. Create a collection for your monsters and then turn them all into JSON before printing their names to the console.

### Bonus

1. Create a Weapon model with relationships and add the weapon relationship to your hero table.
1. Write a query that returns all weapons associated with a particular hero.
1. Write a query that returns all of the heros that have used a particular weapon.


### Resources
[Knex](http://knexjs.org/)
[Bookshelf](http://bookshelfjs.org/)
[Helpful Bookshelf walkthrough](http://stackabuse.com/bookshelf-js-a-node-js-orm/)
[Wikipedia List of Monsters](https://en.wikipedia.org/wiki/List_of_Greek_mythological_creatures)
