# Word Search
Want to go wading a little deeper into streams? Here's a fun challenge that requires a couple of pieces we haven't covered, but see if you can research and find the answers yourself. If not, come ask for help. We won't give you a hard time. Much.

## Requirements

The purpose of this program is to list English words that begin with a search
term.

Use a JavaScript file to act as a Node.js program named `word-search.js`.

This program should read a file `/usr/share/dict/words`. This file is used by
the operating system for spell checking. If this file does not exist on your
system, you can use one from GitHub such as: https://github.com/atebits/Words

Allow an argument to be passed to this program which should be used as a search
term. The case of the search term should not matter. If no argument is passed,
respond only with a simple [usage message][usage]. The search term should be
used to find words that begin with the search term.

Limit the results to only ten words. Case should not matter.

Start with a `createReadStream` and use the [event-stream][es] module's `split`
and `map` methods to manipulate the stream. Yes, that's right. You can also pipe directly to methods like these.

You will also create your own `Transform` stream as a module called
`limit-ten.js`. This `Transform` should limit the number of words to ten before finally `pipe`ing
the results to stdout via the callback.

Be sure to handle cases where no match is found.

In the end, you will have something like this
```
myReadStream
.pipe(split())
.pipe(map( (word, done) => do something to compare the word to my list and call done() ))
.pipe(myImportedTransformStreamModuleMethod)
.pipe(myWriteStream)
``` 

Expected:

```bash
$ ./word-search.js
Usage: word-serch.js [searchterm]
$ ./word-search.js JAVA
Java
Javahai
javali
Javan
Javanee
Javanese
$ ./word-search script
script
scription
scriptitious
scriptitiously
scriptitory
scriptive
scriptor
scriptorial
scriptorium
scriptory
$ ./word-search node
node
noded
```

## Bonus

-   Optionally accept a custom wordlist as stdin
    `cat /usr/share/dict/words | ./word-search.js flex`
-   Allow for regex searches `./word-search.js /ception$/`
-   Write another `Transform` module `hackerTyper` to stream each letter to the
    terminal every 50 ms
-   Try another popular stream library: [Highland.js][highland]
-   If you search for something at the beginning of the alphabet, like 'a' or
    'aal', there is a slight delay before the program exits while the stream
    continues through the wordlist even though the app has printed the 10
    results or will not find any more results. End the stream as soon as
    possible to remove this delay.

## Links

-   [event-stream library][es]
-   [Highland.js library][highland]
-   [Usage messages][usage]
-   [Words (Unix)][words]

[es]: https://github.com/dominictarr/event-stream
[highland]: http://highlandjs.org/
[usage]: https://en.wikipedia.org/wiki/Usage_message
[words]: https://en.wikipedia.org/wiki/Words_(Unix)
