# Direct Execution

## Introduction

In the `hello-world` exercise, we created a program that could execute in the Node.js
environment. By typing `node` followed by the filename, the filename is used as
an argument to the `node` program. `node` recognizes this parameter and then
retrieves the contents of the file and executes it line-by-line in the Node.js
environment.

There are many programs that you can access in the terminal other than `node`
such as [`pwd`](https://en.wikipedia.org/wiki/Pwd),
[`cal`](https://en.wikipedia.org/wiki/Cal_(Unix)), and
[`vi`](https://en.wikipedia.org/wiki/Vi). But most of these programs are written
in a language other than JavaScript.

### Compilers and Interpreters

A program written in a
[compiled language](https://en.wikipedia.org/wiki/Compiled_language) such as C++
must be [compiled](https://en.wikipedia.org/wiki/Compiler) into a
[binary file](https://en.wikipedia.org/wiki/Binary_file) before it can be
executed. Binary files can be executed directly by your operating system without
additional intervention. For example `cal` may be written in a language like
C++, but it is consequently stored on your system in its compiled binary state.
Thus, you can execute `cal` directly rather than passing it into another
program.

JavaScript, Python, and Ruby are popular
[interpreted languages](https://en.wikipedia.org/wiki/Interpreted_language)
which means the execution statements must be passed to an
[interpreter](https://en.wikipedia.org/wiki/Interpreter_(computing)) each time
the program needs to be executed. Programs written in these languages cannot
normally be executed directly. To write a program like `cal` in JavaScript
without requiring the `node` prefix, one would need to a way to tell the
operating system to parse your file through the Node.js interpreter.

We do this with a [Shebang](https://en.wikipedia.org/wiki/Shebang_(Unix))
statement.

### Shebang

By beginning a file with a shebang declaring the interpreter the program
requires, the operating system can handle the direct execution. For all Node.js
programs, use the following shebang as the first line of your file to allow
direct execution:

```js
#!/usr/bin/env node

// rest of application
```

For reference, lets look at a popular command-line program written in Node.js
that you should be familiar with called
[HTTP-Server](https://github.com/indexzero/http-server). `http-server` is a
simple command-line program that creates quick a static web server with Node.js.
You can see a bulk of the source code on Github at this link:

[https://github.com/indexzero/http-server/blob/master/bin/http-server](https://github.com/indexzero/http-server/blob/master/bin/http-server)

We will come back to this source code later, but notice how the first line of
the file begins with the shebang and the rest is JavaScript code:

```js
#!/usr/bin/env node

'use strict';

var colors     = require('colors'),
    os         = require('os'),
// rest of file ...
// Above copied from commit: https://github.com/indexzero/http-server/blob/b456b77c6c476fed0724800ac2c07fa31c647d75/bin/http-server
```

This line allows you to type `http-server` on the command-line once it is
downloaded and installed. Every command-line program you commonly install
through npm, like Grunt, Gulp, Bower, JSHint and even npm itself, will have this
line.

### `./`

While having a file with the proper shebang is needed for the interpreter, many
times the operating system won't look in the current directory for executable
files.

Given a file `executable`, if you attempt to execute it you will get a command
not found error:

```bash
$ executablefile
bash: command not found: executable
```

The places that can include executables are normally defined in a
[PATH variable](https://en.wikipedia.org/wiki/PATH_(variable)). We can get
around this by using a
[dot-slash (`./`) prefix](http://www.linfo.org/dot_slash.html). The dot-slash
prefix should not be confused with a
[dot file](https://en.wikipedia.org/wiki/Hidden_file_and_hidden_directory) which
Unix-like operating systems use to signify a hidden file. The prefix simply
adds the current relative path in front of the file allowing for execution.

```bash
$ ./executablefile
bash: ./executablefile: Permission denied
```

While the dot-slash prefix may seem confusing and even tedious to new users, it
is a safety and security mechanism. If you truly want to execute
`executablefile` without the dot-slash prefix similar to `cal`, you must place
the file or a [symlink](https://en.wikipedia.org/wiki/Symbolic_link) in your
[PATH](https://en.wikipedia.org/wiki/PATH_(variable)). However, that is beyond
the scope of this exercise.

### File Permissions

As noted above, the executable file did attempt to execute but a "Permission
denied" error occurred. This is not related to the security features of
requiring the dot-slash prefix, but due to the
[file system's permissions](https://en.wikipedia.org/wiki/File_system_permissions).

The [file system](https://en.wikipedia.org/wiki/File_system) of your hard drive
defines the rules with regards to the read, write, or execution permissions of
files.

Modern operating systems default to secure file systems such as NTFS, HFS+, or
EXT4 which do not allow direct execution of files unless given explicit
permission to do so.
[Some file systems](https://en.wikipedia.org/wiki/Comparison_of_file_systems)
like FAT which is primarily used today for external flash drives do not have
these restrictions. A simple way to check a file's permissions is with the
[`ls -l`](https://en.wikipedia.org/wiki/Ls) command.

Adding the `-l` flag to the `ls` program changes the output to a use a long list
format. This format, among other things, includes the permissions of each file.

```bash
$ ls -l
-rw-r--r-- 1 user 123 Dec 31 12:34 regularfile
```

The first section of the output shows the file type and its permissions in a
[unique format](https://en.wikipedia.org/wiki/File_system_permissions#Symbolic_notation)

The default permissions of a file are `-rw-r--r--` which allow read
permissions to everyone, write permission to the owner, and no execution
permissions. A file with the additional execution permission would be notated:
`-rwxr-xr-x`. Using the [`chmod`](https://en.wikipedia.org/wiki/Chmod) command,
one can change a file's permissions. Specifically, `chmod +x` all add execution
permissions.

```bash
$ ls -l
-rw-r--r-- 1 user 123 Dec 31 12:34 regularfile
-rw-r--r-- 1 user 123 Dec 31 12:34 executablefile
$ ./executablefile
bash: ./executablefile: Permission denied
$ chmod +x executablefile
$ ls -l
-rw-r--r-- 1 user 123 Dec 31 12:34 regularfile
-rwxr-xr-x 1 user 123 Dec 31 12:34 executablefile
$ ./executablefile
```

## Requirements

For this exercise you will create another Hello World program. However, unlike
the last exercise, this program must be able to be executed directly by the
operating system.

Create a JavaScript file to act as a Node.js program named `direct-exec.js`. The file
should contain JavaScript code to output the phrase "Hello, World. I was executed without the 'node' command!" to the terminal through the stdout stream.

Expected:

```bash
$ ./direct-exec.js
Hello, World. I was executed without the 'node' command!
```
