# Synchronous File IO

## Introduction
In the previous exercise, you got to know the `require()` function and how Node.js is build around the idea of modularized packages of code. Now we turn our attention inward and take a look at the modules built into Node that we will access using the same `require` syntax.

Node comes bundled with a small, but vital set of bare minimum functionalities known as the Node.js core modules, or just 'core' by all the cool kids. We are going to tap into some of those as you begin to master manipulating data.

On the server side of things, accessing the file system is a regular occurrance. Opening, reading, and writing files is made possible with the [fs](https://nodejs.org/api/fs.html) core module. `fs` stands for file stystem, and with it we are given three different ways of interacting with a file  
1. synchronously
1. asynchronously
1. as a stream

You were doing fine until that last one, right? simply put, streams allow you to make the data available as soon as chunks of it are ready. You've seen this in action when...ahem...streaming a movie or a song. You get a couple seconds of loading, then the file starts playing back while the rest of director's cut extended platinum edition of _The Two Towers_ keeps loading its 50 googlebytes of data in the background.

Before we get ahead of ourselves, let's start small with a synchronous reading of a couple of small files.

The first step is always to make the module available: `const fs = require('fs')`  

Now you can use fs's `readFileSync` method to access your file. Be sure to read the docs, linked above, for how to implement this method.

## Requirements

Create a JavaScript file to act as a Node.js program named `file-io.js`. This program
should accept a single argument which should be a file path. Executing the
program will print the contents of that file onto the terminal though the stdout
stream. Executing the program without an argument should simply return without
printing anything. This functionality to be similar to the `cat` command.

Optional: create a second file named `file-io.json` for your program to read.

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
$ ./file-io.js 07.json
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
-   Avoid `.toString`. Return a String primitive rather than a Buffer object
    from `readFileSync`

## Additional Reading

[Read Files with Node.js][http://stackabuse.com/read-files-with-node-js/]
[Loading modules with require()][https://bytearcher.com/articles/loading_modules_with_require/]
