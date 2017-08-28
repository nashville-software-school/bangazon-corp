# Streaming File IO

## Introduction

While writing async code is advantageous over synchronous code, it still
requires an action to fully complete before executing a single callback. Using
streams allows for callbacks to execute many times with smaller chunks of data.
The biggest advantage here is to begin performing work on a later action before
an earlier action is complete.

### stdout

Before now, we have been using `console.log` to create output to the terminal.
In Node.js, `console.log` is a familiar abstraction to directly interfacing with
stdout. If you tried to compare the `07.js` or `08.js` exercises to the `cat`
command, you may have noticed that their outputs weren't exactly the same. Using
`console.log` results in an extra new line character at the end. The reason for
this is that `console.log` automatically adds a new line character after every
execution.

Example:

```js
console.log('line1')
console.log('line2')
```

results in the raw text: `line1\nline2\n` being passed to the stdout stream.

In addition, there is a difficulty with continuing to use `console.log` for
streams. The reason is that a callback will be executed multiple times and not
necessarily when you are able to have a new line. So if we were to stream
every character to the terminal one at a time, we would need a way to do that
without getting a new line after every character.

Directly interfacing stdout fixes both of these issues and Node.js gives us this
interface through a `process.stdout.write` function.

```js
process.stdout.write('line1\n')
process.stdout.write('line2\n')
```

The above code results in the same raw `line1\nline2\n` being passed to the
stdout stream. In essence, the `console.log` implementation for Node.js is
approximately:

```js
console.log = msg => process.stdout.write(`${msg}\n`)
```

One important caveat to using stdout directly is that it is important to end any
stdout stream with a new line character. If not it results in a "Partial Line".
Depending on your shell and its version (BASH, ZSH, etc...), you may notice that
the next command prompt may be added on the same line or an odd inverse+bold
percent (%) character would appear at the end of the output.

Example:

```js
process.stdout.write('This is some text without a newline')
```

```bash
$ ./example.js
This is some text without a newline%
$ ./example.js # next command
```

or

```bash
$ ./example.js
This is some text without a newline$ ./example.js # Next command
```

### Streams

A stream is a mechanism for moving data between two points via a 'pipe'. Three types that we will use are a `Readable` stream, which retrieves data, a `Transform` stream to manipulate the data, and a `Writable` stream to give the manipulated data a destination.

1. A readable stream might grab an scss file and pipe it to...
1. A transform stream, which runs the 'sass()' coversion function on it, then pipes the converted css files to...
1. The writable stream that sticks them in the css directory

Since the terms we're talking about include 'streams' and 'pipes', water is the typical analogy you will see when explaining how this works. A stream can be thought of as a garden hose, or a kitchen sink. The hose or faucet is connected to a water source. When the source pushes water into the hose/pipes, it flows through it. At this point, the water can be used by a sprinkler or used to fill a pitcher.

Most importantly, remember that streams **emit events** to control the flow of water...er...data.

### `Readable` Streams

`Readable` streams produce events with data that can be listened to.

```js
const { Readable } = require('stream')
const readStream =  Readable()
readStream.push('foo ')
readStream.push('bar\n')
readStream.push(null)
```

We created a stream and `push`ed two "chunks" of data to it.
The `readStream.push(null)` function is used to signify that the stream is
finished.

There are several ways of listening to this stream in Node.js. We are going to
demonstrate the "streams1" pattern. While this is an older pattern, it is less
flexible but easier to grasp.
[More information about streams2 pattern][streams2]

`Readable` streams fire events like "data" and "end" that can be listened to.
The "data" event fires whenever a "chunk" of data is received. The size of this
chunk depends on the stream. The "end" event fires an end of stream signal is
received such as an [EOF character][eof], [EOT character][eot], or
[FIN packet][fin].

The following code demonstrates those two callbacks:

Example 1:

```js
const { Readable } = require('stream')
const readStream = Readable()
readStream.push('foo ')
readStream.push('bar\n')
readStream.push(null)

readStream.on('data', buffer => (
  process.stdout.write(`Received chunk: ${JSON.stringify(buffer.toString())}\n`)
))
readStream.on('end', () => process.stdout.write('End of stream\n'))
```

Results in:

```bash
$ node stream.js
Received chunk: "foo "
Received chunk: "bar\n"
End of stream
```

Notice how the "data" event calledback twice because it receive two "chunks" of
data.

Readable streams have a `_read` method that that happens every time a chunk of data is received. We can override this method to tell it how to handle the data -- what to push into the stream and deliver to whatever will be consuming our read stream.

Pushing the strings "1" to "100"
```
let i = 0

readStream._read = () => {
  if (i > 100) {
    readStream.push(null) // when null is pushed to the stream, it emits an 'end' event
  }
  else {
    readStream.push(`${i}`)  // have to make i a string. Streams only handle strings and buffers
    i++
  }
}
```

### `.pipe`

All streams have a `.pipe` method which allows you to seamlessly connect streams
together. It handles the "data" and "end" events itself.

It uses the format of:

```js
src.pipe(dest)
```

`pipe(dest)` returns the `dest` stream so that you can chain the `pipe`s
together.

Thus,

```js
a.pipe(b)
b.pipe(c)
c.pipe(d)
```

becomes:

```js
a.pipe(b).pipe(c).pipe(d)
```

Since `process.stdout` is a `Writable` stream, Here is our code now:

Example 2:

```js
const { Readable } = require('stream')
const readStream = Readable()
readStream.push('foo ')
readStream.push('bar\n')
readStream.push(null)

readStream.pipe(process.stdout)
```

### `Writable` Streams

Directly `pipe`ing to `process.stdout` may cause unexpected results with special
characters like "\n". I'd prefer the raw output to be the similar as example 1
above, but still be able to use `pipe`. We can create our own custom `Writable`
for this purpose.

Writable streams have a `_write` method to access the streaming data.

Example 3:

```js
const { Readable, Writable } = require('stream')

const readStream = Readable()
readStream.push('foo ')
readStream.push('bar\n')
readStream.push(null)

const writeStream = Writable()
writeStream._write = (buffer, _, cb) => {
  process.stdout.write(`${JSON.stringify(buffer.toString())}\n`)
  cb()
}

readStream.pipe(writeStream)
```

The first argument to the `_write` function is the "chunk" of data received by
the stream.
The second argument is an optional encoding parameter, like 'UTF-8'. The underscore is passed in here as a placeholder that says "we don't want to pass anything in".
The third argument is a callback to execute when the callback is complete. If this callback doesn't fire, the second chunk will never be received. If there is a delay in this callback being executed we call that
"back pressure".

### `Transform` Streams

Transform streams both read and write to the stream. Receiving data and
transforming it before sending it back. Rather than creating a special write
stream, lets create a `Transform` stream to manipulate the data before the
`process.stdout` receives it.

Similar to `Writable` streams, `Transform`s have have a `_transform` method to
access the streaming data.

Example 4:

```js
const { Readable, Transform } = require('stream')

const readStream = Readable()
readStream.push('foo ')
readStream.push('bar\n')
readStream.push(null)

const JSONStringify = Transform()
JSONStringify._transform = (buffer, _, cb) => {
  cb(null, `${JSON.stringify(buffer.toString())}\n`)
}

readStream.pipe(JSONStringify).pipe(process.stdout)
```

## Requirements

Create a JavaScript file to act as a Node.js program named `streams.js`. This program
should read a file `languages.json` and output to a file `languages_caps.json`. Use the second
command-line argument to declare the destination. You will not need to make your own `Readable` for this exercise. Simply use
[`fs.createReadStream`][createreadstream] instead. In between, all letters
should be capitalized with your own `Transform` stream. Then the data should be passed to your own `Writable` stream. The [`fs.appendFile`][appendfile] will be helpful
for this task.

In addition, use some other method in the `fs` module make sure executing the
program multiple times does not continue making a larger and larger
`09.Caps.json` file.

Expected:

```bash
$ cat 09.json
{
  "languages": {
    "JavaScript": "Awesome",
    "C++": "Difficult",
    "BASIC": "Old"
  }
}
$ ./09.js 09_Caps.json
$ cat 09_Caps.json
{
  "LANGUAGES": {
    "JAVASCRIPT": "AWESOME",
    "C++": "DIFFICULT",
    "BASIC": "OLD"
  }
}
```

## Bonus

-   ES6 Object Destructuring

## Additional Reading

[EOF][eof]  
[EOT][eot]  
[FIN][fin]  
[Node.js Streams Handbook][streamhandbook]  
[Node.js streams][node.js streams]  
[`pipe`][pipe]  
[`createReadStream`][createreadstream]  
[Streams2][streams2]  
[Buffers][Buffers]  
[`writeFile`][writefile]  
[`appendFile`][appendfile]  

[eof]: https://en.wikipedia.org/wiki/End-of-file
[eot]: https://en.wikipedia.org/wiki/End-of-Transmission_character
[fin]: https://en.wikipedia.org/wiki/Transmission_Control_Protocol#Connection_termination
[node.js streams]: https://nodejs.org/api/stream.html
[pipe]: https://nodejs.org/api/stream.html#stream_readable_pipe_destination_options
[createreadstream]: https://nodejs.org/api/fs.html#fs_fs_createreadstream_path_options
[streamhandbook]: https://github.com/substack/stream-handbook
[streams2]: https://nodejs.org/en/blog/feature/streams2/
[Buffers]: https://nodejs.org/api/buffer.html
[writefile]: https://nodejs.org/api/fs.html#fs_fs_readfile_file_options_callback
[appendfile]: https://nodejs.org/api/fs.html#fs_fs_appendfile_file_data_options_callback
