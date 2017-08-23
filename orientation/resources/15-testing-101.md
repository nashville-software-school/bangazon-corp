# TDD -- Test-driven development

## Mocha

While there are several frameworks for running tests in Node.js and the browser,
Mocha seems to be the most popular. Mocha can work in the browser, but it
focuses on Node.js.

Mocha provides functions like `describe` for grouping your tests
and `it` for running your tests. However, Mocha does not provide
any assertions. Assertions are functions that throw `Error`s given some
conditions.

```js
assert.equal('foo', 'foo') // => Does nothing
assert.equal('foo', 'bar') // => throws "AssertionError: 'foo' == 'bar'"
```

The implementation is approximately:

```js
assert.equal = (a, b) => {
    if (a != b) {
        throw new AssertionError(`${a} == ${b}`)
    }
}
```

Node.js includes some [basic assertions](https://nodejs.org/api/assert.html),
including the `assert.equal` above, but it is limited in its scope. The most
popular assertion library is Chai.

## Chai

Chai has three "flavors" of assertions. Assert is similar to Node.js's built in
assert and is considered a TDD style. Expect and Should are considered
[BDD](https://en.wikipedia.org/wiki/Behavior-driven_development)
styles and read more like English. All flavors include the the same
functionality, but we will focus on using the `assert`s.

## Red Green Refactor

The above phrase is the mantra of test-driven development (TDD). It sums up the process of writing a test _before_ there is even any code to run aginst it, then writing just enough code to make the test pass, followed by refatoring of that code when the bare-minumum logic you initally wrote is no longer sufficient to meet the app's requirements. This, of course, might make the test fail again, which kicks off the whole process again. Sounds fun, right? It is, actually.

## Types of tests
Testing falls into a number of categories, such as 'unit', 'integration' and 'functional'. You can read all about it [here](https://www.sitepoint.com/javascript-testing-unit-functional-integration/).
But here we will focus only on unit tests, which -- simply put -- verify inputs and outputs. If a particular value is passed into a function, does it output what we expect. If you write a function that adds two numbers and returns the result, it's simple to predict the output, especially if the two numbers are hard-coded:

```js
function add() {
	let num1 = 3;
	let num2 = 2;
	return num1 + num2;
}
```

But that function is pretty useless. It's more helpful to us if it adds _any_ two numbers we give it.

```js
function add(num1, num2) {
	return num1 + num2;
}
```
Again, it seems pretty easy to predict the outcome. But once you start taking outside values into a function, all bets are off as to how it will behave.

`add(2, 3)` Easy to predict.
`add("2", 3)` Not as easy, because I can't remember which direction JS does type coercion
`add(null, 6)` uhh, undefined? No wait, OK, some kind of error?
`add(-236, "monkey")` Oh, come one. No one would do that, would they?  

Testing allows you try multiple inputs, simple, bizarre and everything in between, without having to call the function manually yourself, over and over. Plus, a passing test gives you confidence that if something happens in your code that makes a function start outputting unexpected values, you'll know because the test will fail the next time you run it.

So, unit testing is all about targeting individual functions to make sure they give us back what we want them to. This means you don't need to worry about testing functions that don't return some kind of value. And you don't need to test small, helper functions that larger units of your code leverage to make their calculations. In other words, only worry about the public methods of your modules.

## Adding tests to working code

You will dip your toe into the testing waters by writing tests for functions that already exist. 

## Requirements

Using the app we built for the last exercise, we are going to add tests for all
of the exported functions.

Create a `\test` folder and add a test file for the math, parseargs, and dice
modules. Then add at least one unit test for every exported property. Next
create a cli test file for integration tests.

Remember, this is not technically TDD as we aren't testing first then writing code to make the test pass, so make sure you create a failing test before making it pass.

[Assertion](https://en.wikipedia.org/wiki/Assertion_(software_development))
[Mocha](https://en.wikipedia.org/wiki/Mocha_(JavaScript_framework))
[Framework comparison](https://en.wikipedia.org/wiki/List_of_unit_testing_frameworks#JavaScript)
