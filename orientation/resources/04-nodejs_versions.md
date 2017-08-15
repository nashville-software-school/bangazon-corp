# Node.js versions

## Introduction
Node.js has several actively maintained versions. The current version of Node.js
is considered "Stable" and there are other prior versions which are still being
maintained. The most significant of these are considered "LTS" or in a long
term support plan.

### [Node.js Long Term Support][lts]

While we will primarily using the most current version of Node.js for these
exercises, not everyone has this luxury. On the surface, the current version of
Node.js should work the same as a prior version, this is not always the case.
The deeper the integrations or customizations of Node.js that your codebase
uses, the more effort is required to upgrade to a newer version. For this
reason, the Node.js maintainers are committed to fixing bugs, updating security,
and adding small backwards compatible improvements for 3 full years.

#### Release Schedule

![Node.js Release Schedule](https://github.com/nodejs/LTS/raw/master/schedule.png)

For example, Node.js version 6 was released in April of 2016 and will be
maintained until April of 2019. Thus, if you write your code to this version,
you can be sure that it will work and be secure at least until April 2019. At
that point, if the current schedule is maintained, you can either continue to
run version 6 without official support, upgrade your Node.js to version 8 (the
next LTS release), or upgrade all the way to version 12 which is scheduled to be
released in April of 2019. However, you can expect upgrade from version 6 to
version 8 will be simpler than from version 6 to version 12.

Learning and adopting a new technology is an investment, and this support
schedule encourages more people to look at Node.js for their projects. It allows
some users to use the latest features and performance enhancements while
allowing other users the assurance that the code they write today will continue
to work.

### [Node Version Manager][nvm]

TODO: Using nvm

## Topics Covered

-   Command-line applications
-   Modern Node.js version management

## Requirements

Create a JavaScript file to act as a Node.js program named `node-version.js`. The file
should be similar if not exactly the same as the previous exercise. However,
run `nvm use` and change the Node.js version and make sure it works correctly in
multiple versions.

Expected:

```bash
$ nvm use node
Now using node v6.3.1 (npm v3.10.5)
$ ./node-version.js
Node.js version: v6.3.1
V8 version: 5.0.71.57
$ nvm use v4
Now using node v4.4.7 (npm v3.10.5)
$ ./node-version.js
Node.js version: 4.4.7
V8 version: 4.5.103.36
```

## Bonus

-   Use [ES6 Destructuring][destructuring] to extract the two variables from the
    `versions` object
-   Destructuring is a SyntaxError in Node.js by default in versions earlier
    than v6. Manually enable destructuring inside the program. Hint: edit the
    shebang

## Additional Reading

-   [Node Version Manager][nvm]
-   [ES6 Destructuring Assignment][destructuring]

[nvm]: https://github.com/creationix/nvm
[destructuring]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment
[lts]: https://github.com/nodejs/LTS
