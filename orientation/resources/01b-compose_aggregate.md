# Composition, and Aggregation
Before diving into the details of Node.js, let's take another look at ways to define and construct objects in JavaScript in general. Very early in your JS learning we tallked about avoiding repetitive code by leveraging already-defined behavior in an object when creating new objects, known as behavior delegation. We have the ability to connect objects to each other via the prototype chain, which allows for the sharing of properties.

As a refresher example, take a look at this:
(From Kyle Simpson's awesome 'You Don't Know JS' series)

```js
const Task = {
	setID: function(ID) { this.id = ID; },
	outputID: function() { console.log( this.id ); }
};

// make `XYZ` delegate to `Task`
const XYZ = Object.create( Task );

XYZ.prepareTask = function(ID,Label) {
	this.setID( ID );
	this.label = Label;
};

XYZ.outputTaskDetails = function() {
	this.outputID();
	console.log( this.label );
};
```

First an object called `Task` gets created. It has a couple of nifty methods defined on it, that another object, `XYZ`, leverages to help define its own unique methods. This sharing of access to properties is made possible by the prototype chain, which JS uses to link `XYZ` to `Task`. If we call `XYZ.outputTaskDetails`, it works despite that fact that `outputID` has never been set as a property of `XYZ`. JavaScript will look for `outputID` on the `XYZ` object, but it won't be there. But because `TASK` is linked to `XYZ`, JavaScript can "travel" up the prototype chain and look for the property there. 

With all of that fresh in your brain again, let's look at a couple of helpful, but distictly different ways to build objects.

## Composition

Composition is for things that make up other things. If the container object is destroyed, so will all of its composing parts. Composition is the pattern for `part-of` data relationships, which we will discuss further once we get to mapping out how our data's various entities connect to each other in a relational database. For now, think about it in simple human terms: A pancreas is part of a body; a room is part of a building; etc.

```js
const Pancreas = Object.create({});
Pancreas.init = function() {
    this.filtering = true;
};

const Liver = Object.create({});
Liver.init = function() {
    this.poisoned = false;
}

const Body = Object.create({});
Body.init = function() {
    this.the_pancreas = Object.create(Pancreas);
    this.the_liver = Object.create(Liver);
};
```
In this example we create brand new objects, linked to the `Pancreas` and `Liver` objects, inside the init function. They would go away if Body is destroyed, since they are part of the body.

## Aggregation

Aggregation is for adding objects that are needed for the operation of another object, but should exist independently of it should it be destroyed. Aggregation is the pattern for `has-a` relationships. So, a bank has a customer, like so:

```js
const Customer = Object.create({});
Customer.init = function(first, last, acct) {
	this.accountNumber = acct;
	this.firstName = first;
	this.lastName = last;
};

const Bank = Object.create({});
Bank.init = function(infoObj) {
	this.name = "";
	this.address = "";
	this.assets = "";
	
	this.customers = [];
};

// Create the bank
const FirstTennessee = Object.create(Bank);
FirstTennessee.init({
	name: "First Tennessee Bank",
	address: "123 Sesame Street",
	assets: "one Beeeelion dollars"
});

// Create some customers
const steve = Object.create(Customer);
steve.init("Steve", "Brownlee", "000000000");

const greg = Object.create(Customer)
greg.init("Greg", "Korte", "000000000");

const brenda = Object.create(Customer);
brenda.init("Brenda", "Long", "000000000");

//Add the customers into the aggregate instance variable of the bank
FirstTennessee.customers.push(steve);
FirstTennessee.customers.push(greg);
FirstTennessee.customers.push(brenda);
```

In this last example, the Customer you created would not cease to exist should the Bank in which it had an account went bankrupt and closed. And the bank would still be around if a customer was removed.
