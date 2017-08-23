# Third party modules

## Introduction
Open up the [Node.js][node_repl] [REPL][repl] and copy-paste the following line:

```js
console.log('It is \u001b[31mnot\u001b[39m easy to use \u001b[32mhardcoded \u001b[1mANSI\u001b[39m\u001b[22m codes!')
```

The [escape prefix][esc] we are using is `\u001b[` or the ANSI code that follows
adjusts how your terminal presents the text. You should see the word "not" in
red, "hardcoded" in green and "ANSI" both bolded and in green.

It is extremely difficult to read code like this much less to actually write it.

The solution is probably to extract the ANSI code logic details to functions:

```js
const bold = string => `\u001b[1m${string}\u001b[22m`
const green = string => `\u001b[32m${string}\u001b[39m`
const red = string => `\u001b[31m${string}\u001b[39m`
console.log(`It is ${red('easier')} to use ${green('functions for')} ${green(bold('ANSI'))} codes!`)
```

Either way, looking up ANSI codes every time you want to add color to the
terminal output isn't ideal. Also, there might be bugs in the code written
above. However this is a problem likely solved by others so it would be nice to
utilize an open source module for this.

### Node modules

In the browser, to use code written by others we need to download the JavaScript
files through bower or use a CDN. We then add a `<script>` tag and receive a
global variable that we can use to access the code contained in those files.

In Node.js we can use code written by a third-party, but we are not required to
use global variables like the browser. In addition, we cannot use CDNs so we
must download the code. Node comes bundled with [npm][npm], the Node package manager. 
It allows you to effortlessly pull in 3-party libraries hosted on github and save 
them to your specific project, or save them globally when the module is something you need to 
access from anywhere on the command line.

#### Installing packages locally
The command to download the code we want to use, in its most basic form, is
`npm install modulename`

This pulls down the requested package and installs it in the directory where the command was run.
A folder called `node_modules` is added to the directory to hold the package, as well as any other pacakages your requested code depends on to run. Sometimes, there are a surprising number of these additional libraries that come along for the ride. It's all good. Modules that require other modules that themselves depend on other modules is good practice, and is a pattern we will strive to use in our own code.  

That said, `node_modules` can get quite full of code you didn't write. This is why we add `node_modules` to our `.gitignore` files as soon as we create a project. We don't need to push up all of those packages to our repos.
"But", you might be saying, "how does someone else work on my code if they can't access all the libraries the project needs?" That's where the `package.json` file comes into play.  

This oh-so-helpful string o' JSON plays a couple of roles, but for now it's enough to know that it holds the key to sharing your project with collaborators without passing a giant `node_modules` folder back and forth. Inside most `package.json` files you will see something like this:
```
  "dependencies": {
    "gulp": "^3.9.1"
  },
  "devDependencies": {
    "gulp": "^3.9.1",
    "gulp-jshint": "^2.0.0",
    "gulp-watch": "^4.3.5",
    "jshint": "^2.9.2",
    "jshint-stylish": "^2.2.0"
  }
  ```
This particular project leverages six npm modules. What they are isn't important right now. What is key is that anyone who pulls down this project can install and use the same modules as the developer who created the project. All it takes is a `cd` to the directory where `package.json` lives and the command `npm install`. npm will pull down each of the dependencies and install them locally in a `node_modules` folder. 

A `package.json` file is created one of two ways: Manually, or by running `npm init` when you are setting up your project for the first time. This command will run you through a set of options and then create a bare-bones version of `package.json` in the diretory where you issued the command. Either way, it's important to have this file in place before you begin installing packages, or they won't be added as dependencies.  

The two dependency types you see above make the distinction between packages needed only while the app is in development ( tools for testing, linting, minifying, etc), and packages the app will depend on to run in any environment, whether it's development, testing, or production. The difference isn't critical at this stage in your learning. Both will work fine and all modules will install with `npm install`. 

What _is_ important is that you add either `--save` or `--save-dev` to your `npm install` command. Without one or the other, the package will install just fine, but it won't be included in the `package_json`, and therefore won't be installed on a teammate's machine when she runs `npm install` after pulling down your code.  

For example, installing the gulp task runner and adding it to your `package.json` dependencies looks like this: 

`npm install gulp --save-dev`.

#### Installing packages globally
Some npm packages are tools we want to use from the command line, and have access to sytem-wide, not just on a project-by-project basis. Task runner command line interfaces like `grunt-cli`, dev servers such as `http-server`, and even other package managers like Bower are examples of modules we would install globally. To do so, just add `-g` to your install command: `npm install -g grunt-cli`.

> NOTE: Mac users may get errors when attempting global installs. The quick fix is to add `sudo` to the beginning of your command. This is not ideal for security reasons outside the scope of this lesson. But if you find you can't install globally without sudo, here's a [fix][sudo fix] you can try. Not for the faint of heart.

### The require function
So, how do we actually use these modules if we can't insert them via `<script>` tags? Enter `require()`. Require is another Node.js global, available wherever we need to pull in a module. The syntax is simply: 

`const myFoo = require('foo')`

where 'foo' is a module residing in 'node_modules'. `myFoo` now has access to all the public properties and methods contained in the `foo` code. This happens because `require()` reads a javascript file, executes the file, and then proceeds to return whatever `foo` exports. We will look at the 'exports' object very soon when we start writing our own modules. But you know enough now to try your hand at installing, requiring, and using a third-party node module.

## Requirements

For this exercise we are going to use a popular Node.js module: [chalk][chalk]

Create a JavaScript file to act as a Node.js program named `flag.js`. This program
print out a red, white, and blue American flag in the terminal. The stars should
be white bold text with a blue background, the red stripes should be spaces with
a red background, and the white stripes should be spaces with a white
background.

Because of the limitations of the terminal, don't worry about getting all 50
stars exactly as they are on the official flag. However, make sure all 13
stripes are on the flag. Additionally make the flag 50 characters wide and have
1 space of padding before and after the stars.

Use the following format below.

Expected:

```bash
$ ./flag.js
```

![Terminal Flag](http://i.imgur.com/DOMxrXU.png)

## Bonus

-   Use ES6 Object Destructuring to access specific colors needed from the
    module
-   Use a Unicode or emoji star instead of an asterisk
-   Use two variables names `STAR_MARGIN` and `STARS_PADDING` which define the
    separation character or characters between the stars and around the stars.
    Use this variable to quickly change both the margin and the padding to two
    or three spaces. Make sure the output is still aligned and 50 characters
    wide.

## Additional Reading

-   [ANSI escape code][ansi]
-   [Escape characters][esc]
-   [Node.js REPL][node_repl]
-   [Read Eval Print Loop][repl]
-   [What is npm?](https://docs.npmjs.com/getting-started/what-is-npm)

[ansi]: https://en.wikipedia.org/wiki/ANSI_escape_code
[chalk]: https://www.npmjs.com/package/chalk
[esc]: https://en.wikipedia.org/wiki/Escape_character
[repl]: https://en.wikipedia.org/wiki/Read%E2%80%93eval%E2%80%93print_loop
[node_repl]: https://nodejs.org/api/repl.html
[npm]: https://www.npmjs.com/
[sudo fix]: https://johnpapa.net/how-to-use-npm-global-without-sudo-on-osx/
