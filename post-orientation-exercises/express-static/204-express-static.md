# Express Static

The Express docs tell us that "To serve static files such as images, CSS files, and JavaScript files, use the `express.static` built-in middleware function in Express." Great! What's a middleware function? You will learn more about middleware in the next exercise, but in short, middleware is a JavaScript function that runs between the client request and the server answer. If you want to take the request from the client, inspect it, learn what it is asking for, then make an appropriate response, middleware is your new tool of choice in an Express app.

With that in mind, you can tell Express where to find files you want it to serve directly by passing the name of the directory as an argument. For example, use the following code to serve files in a directory named public: 

_from http://expressjs.com/en/starter/static-files.html_
```js
const express = require('express')
const app = express()

app.use(express.static('public'))

app.listen(3000)
```

The code above will start a server that will serve up any file in a folder called public. _Magic!_

i.e.

`/public/index.html will be served as http://localhost:3000/index.html`
`/public/images/logo.jpg will be served as http://localhost:3000/images/logo.jpg`

Pay attention to `app.use()`. That's our key to using middleware functions in our app. `use()` tells Express to load whatever middleware you pass to it. In this example, it loads a built-in middleware function called `static` that configures the server to look into the folder passed into it ( 'public' ) first when looking for files to serve up.  

Now look at this slightly more complicated version  

```js
const express = require('express')
const app = express()

app.use(express.static('public'))

app.get('/', (req, res) => {
    res.send('Hello World')
})

app.listen(3000)
```

In the example above, if a request comes in to the root `/` route, express will
first look in the public folder for `public/index.html`. If it finds a file at
that path, it will send the file to the browser and end the stream. If it
doesn't find a file at that path, the request gets sent down the pipeline to the
`app.get('/', ...)` route handler. The handler will execute its callback because
the request url matches '/'. The callback ends the pipeline after sending the
text "Hello World" to the browser by calling `res.send()`.

vs.

```js
const express = require('express')
const app = express()

app.get('/', (req, res) => {
    res.send('Hello World')
})

app.use(express.static('public'))

app.listen(3000)
```

In the example above, if a request comes in to the root `/` route, the
`app.get('/', ...)` route handler will receive it first. The handler will
execute its callback because the request url matches "/". The callback ends the
pipeline and will never look in the public folder for an `index.html` file. If a
request comes in to the `/test` route, the `app.get('/', ...)` route handler
will receive the request, but pass it down the pipeline because it doesn't match
the route. Next the `app.use(express.static(...))` handler will check if a file
at the path `/public/test/index.html` exists. If so, it will send the file
contents to the browser. If not, it will pass it further down the pipeline.
However, at this point we do not have anything at this stage so the request will
hang.

## Requirements

Create a Express server that uses static files. Place an `index.html`, an image
and a css file in a directory named `public`. Use the index.html to display the photo and
some caption text in red or another color using css.

```
┌───────────────────────────────────────────────────┐
│┌───────┬─────────────────────────────────────────┐│
││ ◀ ▶ ◌ │ https://mycat.com                       ││
│└───────┴─────────────────────────────────────────┘│
├───────────────────────────────────────────────────┤
│                                                   │
│                                                   │
│             ┌───────────────────────┐             │
│             │  /\     /\            │             │
│             │ {  `---'  }           │             │
│             │ {  O   O  }           │             │
│             │ ~~>  V  <~~           │             │
│             │  \  \|/  /            │             │
│             │   `-----'____         │             │
│             │   /     \    \_       │             │
│             │  {       }\  )_\_   _ │             │
│             │  |  \_/  |/ /  \_\_/ )│             │
│             │   \__/  /(_/     \__/ │             │
│             │     (__/              │             │
│             └───────────────────────┘             │
│                                                   │
│                     Sniffles                      │
│                                                   │
│                                                   │
│                                                   │
│                                                   │
└───────────────────────────────────────────────────┘

```

## Additional Reading

## Credits

Ascii art: http://www.asciiworld.com/-Cats-.html
