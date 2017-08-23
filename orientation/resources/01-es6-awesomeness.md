# ES6 Awesomeness

## Introduction
You were introduced to some of the latest JS syntax, defined in the ECMAScript specification most often referred to  as ES6, during your front-end course. We will be using ES6 extensively in the Node.js portion as well.
Let's review some of the features you are familiar with, as well as some that may be new to you.

Way back in the olden days of 2014, we had but one measly way to identify and hold a value in JavaScript: the `var` keyword. Whatever the data type or scope, whether the value was fleeting or permanent, var was your single option. With the latest ECMAScript spec, we now have three ways to define variables, with the introduction of `let` and `const`

### [const](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/const)
#### What MDN says
> "The const declaration creates a read-only reference to a value. It does not mean the value it holds is immutable, just that the variable identifier cannot be reassigned. For instance, in case the content is an object, this means the object itself can still be altered."

#### In action
```
const MAX_CAT_SIZE_KG = 3000;
MAX_CAT_SIZE_KG = 5000; // Oops. SyntaxError. Can't be reassigning the value of a const like that.
```
Note that the all-caps variable name is just a naming convention some people use. It can help you remember
that you're dealing with a constant when you see it pop up in another area of your codebase

Now, if `MAX_CAT_SIZE_KG` were defined as an object, don't get cocky and think you have a built an impenetrable fortress by using `const`. You can't change it from an object to another type...
```
const MAX_CAT_SIZE_KG = { weight: 3000};
MAX_CAT_SIZE_KG = "3000" // Uncaught TypeError: Assignment to constant variable.
```
...but you can change how fat your cat can get.
```
MAX_CAT_SIZE_KG.weight = 3500;
```
Now logging our const will show `Object {weight: 3500}`.

### [let](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let)
#### What MDN says
> "let allows you to declare variables that are limited in scope to the block, statement, or expression on which it is used. This is unlike the var keyword, which defines a variable globally, or locally to an entire function regardless of block scope."

#### In action
Our old friend `var`:
```
function func2() {
  if (true) {
      var tmp = 123;
  }
  console.log(tmp); // returns 123
}
```
Same code, but using `let`
```
function func1() {
  if (true) {
      let tmp = 123;
  }
  console.log(tmp); // ReferenceError: tmp is not defined
}
```
While `var` is global to any top level scope in which it is defined, `let` is less social. It prefers to keep to itself and be available only in the block of code where it is defined. That means that a `let` variable inside
an `if` statement, or a `for` loop, or even just inside another set of curly brackets cannot be accessed outside of that scope. We will look at other examples of block scope in class.

### [Object literal shorthand](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Object_initializer)

MDN doesn't have a lot to say about this new lazy way of creating object literals, but it's worth pointing out here because with Node.js we will be exporting a lot of objects from modules, and the ability to use shorthand property names will save you a lot of keystrokes over time. Take a look:

#### In action
```
let wow = "Hi there";
let es6 = "ES6";
myNum = () => {console.log("howdy")};
```
Here, we have defined three random variables; a couple of strings and an anonymous function. If we wanted to save
these as properties of an object, in ES5 and earlier, we would have done some repetitive typing.
```
let myOldObj = {
  wow: wow,
  es6: es6,
  myNum: myNum
};
```
But with the new new shorthand way, the JavaScript engine looks in the containing scope for a variable with the same name as the property. If it is found, that variable’s value is assigned to the property.
```
let myNewObj = {
  wow, es6, myNum
};
```

### [Destructuring assignment](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)
### What MDN says
> "The destructuring assignment syntax is a JavaScript expression that makes it possible to extract data from arrays or objects into distinct variables."

Well, that's kind of lame, honestly, considering how useful and powerful this feature is. We will be using destructuring assignments on a daily basis, so make sure you wrap your brain around it.
Essentially, it's another shorthand syntax that allows you to grab specific items from arrays or objects and place them right into a variable with no middle man involved. It's like buying variables wholesale! We pass the keystroke savings on to you!

#### In action
Here's a POJO (plain old JavaScript object. Seriously. Look it up) that holds values we want to save into some variables.
```
const dog = {
  name: "Murphy",
  breed: "Australian Shepherd",
  speak: function() { return 'woof!' }
};
```
You are familiar with the following syntax. If you're not, just fake it. We won't notice.
```
var breed = dog.breed;
var speak = dog.speak;
```
Here's the new look:
```
const { name, speak } = dog;
console.log( `${name} says ${speak()}` ); // returns "Murphy says woof!"
```
Don't like being stuck with the property names as your variables? We gotcha covered:
`const { name: dogMoniker, speak: sayWoof } = dog;`

Here's how it looks with arrays
```
let [a,b] = [5,10];
console.log(a, b); // returns 5 10
```

Extract only what you need:
```
let x = [1, 2, 3, 4, 5];
let [y, z] = x;
console.log(y); // 1
console.log(z); // 2
```

### [(Fat) arrow functions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions)
#### What MDN says
> "An arrow function expression has a shorter syntax than a function expression and does not bind its own _this_ ... Arrow functions are always anonymous."

Once again, MDN really knows how to sell it, don't they? Arrow functions (or 'fat arrows', since you use a
`=` instead of a `-` to type their shape) are another ode to laziness. Is the `function` in your anonymous function declaration too much work? Use `=>` instead.

#### In Action
ES5
```
var sum = function(num1, num2) {
  return num1 + num2;
};
```
ES6
```
let sum = (num1, num2) => num1 + num2;
```
That's right, you also don't need the curly braces when you just need to return a single line (known as an _expression body_). Only have a single argument? You don't need the parentheses either.
```
let greeting = msg => msg
```
effectively equivalent to:
```
let greeting = function(msg) {
  return msg;
}
```
The cleaner syntax really pays off in callback-riddled code
```
var numbers = [1,2,3,4,5];

//messy
var timesTwo = numbers.map(function (number) {
  return number * 2;
});

// cleaner
var timesTwo = numbers.map((number) => number * 2);
```
But the arrow function's beauty isn't just typeface deep. It also behaves very differently from an old-school anonymous function. If you haven't quite grokked how the `this` keyword works, back slowly away and get that down before tackling how `this` behaves in an arrow function. Otherwise, check this out. Imagine passing an Angular factory to a controller...

```
// This code doesn't work (value of this.foo isn't updated), because the scope changes when you
// go down a level into the nested function
function myCtrl (MyFactory) {
  this.foo = 'a helpfule string';
  MyFactory.doSomeStuff(function (response) {
    this.foo = response;
  });
}

// This would fix it because we pass the top level foo into `.bind()` which preserves the context
// inside the nested function
function myCtrl (MyFactory) {
  this.foo = 'Hello';
  MyFactory.doSomeStuff(function (response) {
    this.foo = response;
  }.bind(this));
}

// Or, an old 'hack' to deal with the issue is to declare a global (to the function, at least) var to hold
// the value of 'this'
function myCtrl (MyFactory) {
  var that = this;
  that.foo = 'Hello';
  MyFactory.doSomeStuff(function (response) {
    that.foo = response;
  }
}

// The arrow function allows us to “inherit” the scope we’re in if needed. Which means if we changed
// our initial example to the following, the `this` value would be bound correctly:
function myCtrl (MyFactory) {
  this.foo = 'Hello';
  MyFactory.doSomeStuff( (response) => {
    this.foo = response;
  });
}

// And since it's a single line block, we can refactor it to get this beauty
function myCtrl (MyFactory) {
  this.foo = 'Hello';
  MyFactory.doSomeStuff( (response) => this.foo = response);
}
```

#### [Spread syntax and rest operator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_operator)
### What MDN says
> "The spread syntax allows an expression to be expanded in places where multiple arguments (for function calls) or multiple elements (for array literals) or multiple variables  (for destructuring assignment) are expected.
The rest parameter syntax allows us to represent an indefinite number of arguments as an array."

Makes perfect sense, no? Yeah, let's just go to the code.

#### In Action
Let's combine two arrays with the spread syntax:
```
let countries = ['Moldova', 'Ukraine'];
let otherCountries = ['USA', 'Japan'];

let meldedCountries = [...countries, ...otherCountries];
console.log("melded", meldedCountries ); // => ['Moldova', 'Ukraine', 'USA', 'Japan']

// or...
countries.push(...otherCountries);
console.log(countries); // => ['Moldova', 'Ukraine', 'USA', 'Japan']
```
The rest parameter in action, used in a destructuring assignment (This will come in very handy in Node. Stay tuned)
```
let nums = [1,2,3,4,5,6,7,8,9];
let [,,,,...numsJr] = nums

console.log("numsJr", numsJr); // returns, [5,6,7,8,9]
```

#### [Sets](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set)
#### What MDN says
> Set objects are collections of values. You can iterate through the elements of a set in insertion order. A value in the Set may only occur once; it is unique in the Set's collection.

Sets act a lot like arrays, in that they are a container for a bunch of values. You can even run a `.forEach` on them. But that guard against duplicate values is certainly different from how arrays work. Sets also have some very handy methods, such as `.delete`, which simply takes the item you want to remove as an argument, and `.has`, which checks if an item exists in the set. 

#### In Action

```js
const awesomeSet = new Set();

awesomeSet.add(10); // Set { 10 }
awesomeSet.add(6); // Set { 10, 6 }
awesomeSet.add(6); // Set { 10, 6 } No dupes allowed!
awesomeSet.add("set 'em up, knock 'em down"); // Set { 10, 6, "set 'em up, knock 'em down" }

var tinyObj = {a: 1, b: 2};
awesomeSet.add(tinyObj);

awesomeSet.add({a: 1, b: 2}); // Each object is unique. tinyObj is referencing a different object so this is okay

awesomeSet.has(10); // true
awesomeSet.has(35); // false, 35 has not been added to the set
awesomeSet.has(Math.sqrt(36));  // true
awesomeSet.has("Set 'em up, Knock 'em Down".toLowerCase()); // true

// This is pretty interesting. It retains a reference to the object we added, but when we log out
// awesomeSet below, there's no mention of the tinyObj variable name.
awesomeSet.has(tinyObj); // true

awesomeSet.size; // 5

awesomeSet.delete(10); // removes 10 from the set
awesomeSet.has(10);    // false, 10 has been removed

awesomeSet.size; // 4, we just removed one value
console.log(awesomeSet);// Set {1, "some text", Object {a: 1, b: 2},
```

#### [Maps](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map)
#### What MDN says
> The Map object holds key-value pairs. Any value (both objects and primitive values) may be used as either a key or a value. 

Sounds just like a regular old Object. What's the difference?  

Both let you set keys to values, retrieve those values, delete keys, and detect whether something is stored at a key. Basically, you have been using Objects as Maps most of the time in your work so far;But, there are some..ahem..._key_ differences that make using a Map preferable sometimes. Some examples:

+ The keys of an Object are strings, whereas they can be **any value** for a Map, including functions, objects, and any primitive. Whoah. A function as a key? Mind melting. (It's beyond the scope of this intro to get into _why_ you would do this, but if you're curious, checkout out this [SO answer](https://stackoverflow.com/questions/45040871/es6-map-why-when-to-use-an-object-or-a-function-as-a-key) 
+ You can get the size of a Map easily with the `size` property, while the size of an Object must be determined manually. 
+ It's easy to empty a Map with `clear()`

When do you use one over the other? Good question. Here's one way to look at it: Consider a Map when you need a key-value _collection_. A good indicator that you need a collection is when you add and remove values dynamically from the collection, and especially when you don't know those values beforehand (e.g. they're read from a database, input by the user, etc).

Consider using objects when you know which and how many properties the object has while writing the code - when their shape is static, ie when you need a _record_. You will recognize this is the case when you don't need to use bracket notation to access the keys, for example, meaning you know the keys and can use dot notation to access those keys' values.

```js
// instantiate a new Map object
const fooMap = new Map();

// define some random variables
const keyString = "Check this out",
      keyObj = {color: "red", hexVal: "#FF0000"},
      keyFunc = function() {};

// setting the values: the keys are the variables from above and each value is some kind of value associated with that variable

// Setting a description of what `keyString` is
fooMap.set(keyString, 'This is part of a catch phrase');
// Lets make an array of `keyObj`'s keys and associate them with each other
fooMap.set(keyObj, Object.keys(keyObj));
// Maybe we want to keep track of how many times this function gets called
fooMap.set(keyFunc, {callCount : 0});

//Setting a value kind of like you're used to doing with an object, just a key / value pair -ish
fooMap.set("name", "Frederica");

fooMap.size; // 3

// getting the values with the Map's `get` method
fooMap.get(keyString);    // 'This is part of a catch phrase'
fooMap.get(keyObj);       // ["color", "hexVal"]
fooMap.get(keyFunc);      // {callCount: 0}
fooMap.get(name);         // "Frederica"

fooMap.get("Check this out.");   // 'This is part of a catch phrase'
                                // because keyString === "Check this out."

fooMap.get({});           // undefined, because keyObj !== {}. Two objs never equal each other
fooMap.get(function() {}) // undefined, because keyFunc !== function () {}. Because ditto

fooMap.values()           // {"This is part of a catch phrase", Array(2), {…}, "Frederica"}
fooMap.entries()          // {"Check this out" => "This is part of a catch phrase", {…} => Array(2), ƒ => {…}, "name" => "Frederica"}
```

Some day soon you may see `Map` and `Set` in use quite a bit, but for now they will be fairly rare birds. Think of them as two new friends you can invite to the party but don't have to chat with much for the time being. Just stroll over to them every now and then when you think of it and see if you get along. 


On to the exercise!

## Requirements
Create a file called `cheer.js` that performs a cheer, using a named passed to it. Use as many of these as you can, even if the code you write isn't as compact or 'efficient' as you would like. This is more about trying out some ES6 than being succinct.

1. const and/or let
1. Object literal shorthand
1. Destructuring assignments
1. Fat arrow function(s)
1. Spread operator
1. Template literals

Execute the file by typing `node cheer.js` inside the file's directory.

Expected:

```bash
$ node cheer.js
Give me a J!
Give me an O!
Give me an H!
Give me an N!
Give me a D!
Give me an O!
Give me an E!
What does that spell?
JOHN DOE!
```
Note: the 'a' vs. 'an'

## Bonus

-   Delay each line by 1 second

## Additional Reading

-   [Destructuring and parameter handling in ECMAScript 6 ](http://www.2ality.com/2015/01/es6-destructuring.html)
-   [How Three Dots Changed JavaScript ](https://rainsoft.io/how-three-dots-changed-javascript/)
-   [Getting Started With ES6 (skip section 1) ](http://www.2ality.com/2015/08/getting-started-es6.html)
