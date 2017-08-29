# Modularity

Modularlity is very important in programming and that is no different for
Node.js apps. The Node.js ecosystem encourages developers to create many very
tiny modules, sometimes with only a single exported function.

Example:

```js
// https://github.com/then/is-promise/blob/v2.1.0/index.js
module.exports = isPromise;

function isPromise(obj) {
  return !!obj && (typeof obj === 'object' || typeof obj === 'function') && typeof obj.then === 'function';
}
```

List of other ["awesome"](https://github.com/sindresorhus/awesome) tiny
Node.js modules: https://github.com/parro-it/awesome-micro-npm-packages

## Requirements
Create a program that performs a dice roll. You will need a folder `dice-roll` with at least 5 files to accomplish this task.

```
dice-roll/
    bin/
        diceroll
    lib/
        cli.js
        dice.js
        math.js
        parse-args.js
README.md
package.json
```

This library should be globally installed or linked such that you can execute it
anywhere on your system. ( In other words, use a shebang )

The file without an extension should be the only file that has permissions to
execute. It should also include only single line of code that requires the
`cli.js` lib file and a shebang.

The math file should export an `Object` with a method called `randomInt` that
accepts two arguments, a lower bound and an upper bound. This function
should return a random integer inclusive of the lower bound and the upper bound.

This parse-args file should export a single function to parse your command line
arguments. The function should accept an array containing the arguments passed
on the command line. Convert these arguments to an object with a count and sides
property.

The dice file should export an object with at least two methods called
`roll` and `toDiceNotation`. The `toDiceNotation` method should accept
an object with a sides and count property and convert it to a `String`
with the dice notation i.e. "1d6" or "2d40". The `roll` method should
accept a dice notation `String` and produce a random `Number` which is
the result of the dice roll.

The `cli.js` file should work like a controller. It should have no logic
on its own. Use this sample code to get you started:

```js
'use strict'

process.title = 'Dice Roll'

const { argv: [,, ...args] } = process
const { count, sides } = require('./parse-args')(args)
const { roll, toDiceNotation } = require('./dice')

const dice = toDiceNotation({count, sides})
const total = roll(dice)

console.log(total)
```

Expected:

```bash
$ diceroll
3 # Result of a single roll of a standard 6 sided dice: i.e. random integer 1 - 6
```

```bash
$ diceroll 20
10 # Result of a single roll of a 20 sided dice: i.e. random integer 1 - 20
```

```bash
$ diceroll 2 100
100 # Result of 2 rolls of a 100 sided dice: i.e. random integer 2 - 200
```
