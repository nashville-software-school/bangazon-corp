## TDD First Steps

Now it's time to try building something by starting with only a single, failing test. Sounds like something Morpheus would say in The Matrix, doesn't it?

The app will sound familiar. You may have built something just like it in the front end. But the process for building it will not be familiar. 

## Requirements

Construct a simple calculator, using modular structure. Create modules for each operation: add, subtract, multiply, divide, plus a module for pulling in all of those operations, just as you did in the diceroll exercise.

Build the math modules using TDD with Mocha and Chai assertions. Remember, with unit tests your concern is with individual functions that return some kind of value. Start with an empty module and a single test. Something like...

```js
describe('add', () => {
  it('should be a function', () => {
    isFunction(add);
  });
});
```  

It will fail, because _this is the first code you have written for you app_. There is no `add` method. Make that test pass by writing a function that does nothing, then rinse and repeat with every module until you have the skeleton of a project. Return to each method and write new tests for number of arguments, the type of data the function returns, whether the method returns anything at all, etc. Test multiple edge cases. What if no arguments are passed in? What if a string gets passed in where you need a number, or vice versa? What happens with negative numbers? Decimals? Leave no stone unturned!

## Resources

### package.json
You will need to run `npm init -y` to create a package.json. Then run `npm install --save-dev mocha chai`

Also, add the following to your package.json:
```js
  "scripts": {
    "test": "mocha -R list --watch --recursive"
  }
```
This will tell mocha to run the tests every time you update a test file, to use the "list" reporter (look at the docs to pick another style if you'd like), and to look into any nested test folders for additional test files (not necessary at this point, but nice to know you can do that), when you run `npm test` on the command line.

### test folder
Create a `test` directory to store your test files in. Mocha will default to looking there for your tests. Can I type 'test' too many times? testtesttest. Looks like I can't.
