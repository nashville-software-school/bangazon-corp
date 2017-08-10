# Asynchronous File IO

## Introduction
Congrats on completing that exercise on loading files synchronously. Now never do it again. There are most likely some cases for using `readFileSync`, but we're going to focus on asynch from now on.  

As you know, any time you write asychronous code in JavsScript, you need a way to handle what happens when the asyc operation is done. Say hello again to callbacks. Just as we listened for events to fire in front end JS, we will do the same thing here in Node.js. Let's give the last exercise a refactor, and this time you will use the async version of reading a file with the `fs` module: `readFile`. This method takes a path to file and an optional argument for setting the file format, just like its synchronous sibling, but it also takes a third argument of a callback to run once the read operation is done. 

`readFile` is also notable here as your first exposure to a critical key to writing robust, maintainable Node.js code -- error handling. Note that the callback function in this method takes two arguments

```
fs.readFile('/etc/passwd', (err, data) => {
  if (err) throw err;
  console.log(data);
});
```

This "error-first callback", as they are known, will be a common sight in asynchronous Node methods. The non-blocking nature of Node.js means that your code continues on its merry way after an async operation enters the event loop. If something goes wrong with that operation, other parts continue to run, oblivous to the problem. It's like getting a nail in your tire but continuing to drive. A ten dollar patch can turn into a very expensive blowout. An error argument is added to the callback so you can grab it and handle it at the source. Ignoring an error makes debugging much more difficult. (It's much harder to figure out where that nail came from 20 miles down the road). 

On a more practical level, Node.js is built to halt the running process when it encounters [uncaught exceptions](https://shapeshed.com/uncaught-exceptions-in-node/). Remember, Node runs on a single process, so that pretty much means your whole program stops. This is actually a good thing, because you can then handle the error and restart the process. Error handling is an art and science unto itself. It should eventually be a vital part of your Node.js skillset. For now, just get used to not ignoring errors, and handle them with a `console.log()`. When we start building our own servers, we will add a slightly more elegant response.  

Check the [docs](https://nodejs.org/api/fs.html#fs_fs_readfile) for details about `fs` then give the exercise a try. 

## Requirements

Create a JavaScript file to act as a Node.js program named `async.js`. This program
should work exactly the same as the previous exercise. However you cannot use a
synchronous method.

Optional: create a second file named `08.json` for your program to read.

Example:

```json
{
  "languages": {
    "JavaScript": "Awesome",
    "C++": "Difficult",
    "BASIC": "Old"
  }
}
```

Expected:

```bash
$ ./08.js 08.json
{
  "languages": {
    "JavaScript": "Awesome",
    "C++": "Difficult",
    "BASIC": "Old"
  }
}

```

Note: Make sure with `pwd` before executing that you are in the directory that
file is in.

## Bonus

-   ES6 Object Destructuring
-   Avoid `.toString`. Callback with a String primitive rather than a Buffer object
    from `readFile`

## Additional Reading
[Understanding error-first callbacks](http://fredkschott.com/post/2014/03/understanding-error-first-callbacks-in-node-js/)
[Async programmingin Node](https://blog.risingstack.com/node-hero-async-programming-in-node-js/)
