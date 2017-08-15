# Global object

## Introduction

### `global`
We mentioned in the Node 'hello world' exercise that "there is a global `document` object that has methods for interfacing the Document Object Model (DOM). Without a visual interface, the DOM is not available so this object does not exist."

Fortunately, Node _does_ have a similar global object, called....wait for it....`global`. Setting a property to this namespace makes it available throughout your Node code. But! Don't do it. It undercuts the whole module system that so nicely encapsulates our code. 

The concept of global objects in Node is tricky at first, because there are, for lack of better terms, 'true' _global_ globals and 'pseudo' globals that are only global at the module level. For now, keep in mind that we have left the DOM behind, but have carried over some familar faces like `console.log()` and `setTimeout()` that are available globally, even if they aren't attached to the same objects we are used to from the front end. And we have new toys to play with, such as `process` and its many helpful properties and methods.

`process` is another Node global. It contains a number of extremely useful and important properties that we will leverage for passing arguments from the command line, gleaning information about the current running process (more on that later), and using what are called the STDIO streams. Streams allow us to read, write, and change data, and pass it from one place to the next via UNIX 'pipes'. 

>(OK, breathe. It's going to be alright. This gibberish will sort itself out once we write some code).

A couple of examples: `process.exit()` is one way to terminate your running program; `process.env` returns an object containing the user environment. This will come in very handy when we want to tell Node whether we want to execute code in a testing, devlopment, or production environment. Look at the [docs](https://nodejs.org/api/process.html) for a full list, or better yet, log it out and dig around in the object itself, then complete the exercise below.

## Requirements

Create a JavaScript file to act as a Node.js program named `global.js`. The file
should contain JavaScript code to output detailed `version` information of Node.js
and the underlying V8 JavaScript engine to the terminal using the `process` 
global object. In addition, the program should be able to directly execute.

Expected:

```bash
$ ./global.js
Node.js version: v6.3.1
V8 version: 5.0.71.57
```

## Bonus

-   Use an ES6 Template Literal and its string interpolation feature
-   Use a single call to `console.log`
-   Use ES6 Destructuring Assignment to extract the two variables from the
    `versions` object

## Additional Reading

-   [V8 JavaScript engine](https://en.wikipedia.org/wiki/V8_(JavaScript_engine))
