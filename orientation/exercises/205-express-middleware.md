# Express Middleware and Routing

An Express server can be broken down into three basic parts: *the router*, *routes*, and *middleware*.

### Routing
The core of any web server is robust request routing. If you think about the server-client request life cycle, it essentially boils down to: the client requests a resource, the server tries to locate the resource and if it's found, respond in a way that the client is expecting. Without a structured way to handle any route requests, your web server will do very little.

Creating a route looks generally like this: `app.get('/songs/:id', function(res, res, next){...});`.

The HTTP verb, or method, is most often one of these four: GET, POST, PUT, and DELETE.
* A GET request is used to fetch data from a web server. It is the most common type of request.
* A POST request is used to send data to a web server.
* DELETE and PUT requests round out a CRUD application. DELETE is used to delete information from a web server and PUT is generally used to update existing data on a server.
* PATCH can also be helpful when augmenting an existing chunk of data or updating only one property.

The most important difference in HTTP verbs is how data is passed around.
* In a GET request, data is passed either in the URI (route parameters) or as query-string parameters (like `?name="fred"`).
* For POST methods, there is a payload or body attached to the request, such as an object containing the email and username of a newly registered user. This allows POST requests to send much more information in the request compared to GET requests.
* The DELETE method generally lacks a body, similar to GET requests.
* PUT methods have a payload, just like POST.

The second half of an HTTP request is the uniform resource identifier, or URI. Every request a web browser makes is to some URI; www.google.com, for example, is a URI that goes directly to the Google home page resources. As mentioned above, URIs can send parameter data to the web server a couple of ways: query-string parameters and route parameters. For example, an Express path with route parameters could look like this:
`/districts/:regionName/volunteers/:volunteerId` (This should look familiar from Angular routing. Remember using $routeParams?)

( BTW, URL = "Uniform Resource Locator" and URI = "Uniform Resource Identifier". Locators are also identifiers, so every URL is also a URI, but there are URIs which are not URLs. Clear as mud. Moving on. )

When we create routes in an Express app, we want to be able to match up an incoming request URI with a route definition in order to load the correct template (when building an application server), or return the correct data to the user (if we're building an API). Regardless, it's all about sending back the proper response to the request.

For example, say we had an Express server running a pet adoption site and our browser made a request using the path `/dogs/breeds/beagle/05?spayed=true`. Browsers default to sending GET requests, so the browser would make a GET with that path.

In our Express routes we have a definition that would look like `GET /:petType/breeds/:breedName/:petId`.
This route would match our request that we sent from the browser. When the request comes into the Express router, the route parameters `:petType` and `:petId` will be parsed from the incoming URI and made available via `req.params.petType` and `req.params.petId`. Because we included a query string (`?spayed=true`), the updated path parameter would become `/:petType/breeds/:breedName/:petId?`. The query-string part of the URI is not used in routing decisions, but the `spayed` parameter would be available as a property on `req.query`. So, in our example, checking for `req.query.spayed` in our callback function would give us `true`.

### Middleware
Remember from the earlier http exercises you learned that Node.js http requests are readable streams, and node http responses are writable streams. When a request comes into the server, it travels through a pipeline. At each stage of the pipeline, a _middleware_ function can modify the stream, pass it on to the next function, or not pass it on. The most common middleware functionality needed are error managing, database interaction, getting info from static files or other resources.

Note that every middleware function takes three arguments: `(req, res, next)`. Those are the request object, the response object (look back at exercises 01 and 02 to jog your memory about request and response), and a function to run when the middleware is finished and ready to pass control to the next middleware in the stack. With those three it can do the following:

+ Make changes to the request and the response objects (like adding addional properties or formatting data)
+ End the request-response cycle (by sending a response back to the client or just calling 'res.end()')
+ Call the next middleware in the stack ( with `next()`)

A single Express route can have as many middleware functions associated with it as needed. Think of middleware as the workers on an assembly line, adding and changing things, dare we say _transforming_ your data before sending it on to the next middleware function or finally back to the client. A middleware function could be used to
--set a cookie or header
--check a user'<s></s> login status
--compress JavaScript
--thousands of other purposes

Middleware are always invoked in the order they are added. If the current middleware function does not end the request-response cycle, it must call `next()` to pass control to the next middleware function. Otherwise, the request will be left hanging (Yeah, you'll make that mistake a few times. Trust us). This is useful for middleware functions that have asynchronous activity. Just call `next()` when the code is complete to alert Express that this particular middleware function is done. (If the middleware is responsible for sending a response instead of passing the baton to another middleware, there’s no need to call next)

A request is considered complete when a middleware function sends a response to the client. A middleware function that completes a request is sometimes referred to as a handler.

After that long data dump, let's start small, shall we?

So you have a simple express server set up from the previous exercises. You are receiving the user's request and sending your response. <strong>You have a stream.</strong> A pretty boring stream, but a stream nonetheless. What if we wanted to DO something with our request before deciding what to send as a response?

You can tell your Express app to use a middleware that handles the request stream before sending back a response.

### app.use()
Remember, in the previous exercise you learned that `use()` tells Express to load whatever middleware you pass to it. Whether that middleware actually _executes_ depends on how you set it up. For example, the following will run on every request:
```
var app = express();

app.use(function(req, res, next) {
  console.log(req.method, req.url);
  next();
});
```
But this example passes in a route as the first argument, and therefore the code block will execute only on requests that match `/user/<some user id>`:
```
app.use('/user/:id', function (req, res, next) {
  console.log('Request Type:', req.method)
  next()
});
```

### Common Middlewares
- [app.use(express.static('public'))](https://expressjs.com/en/starter/static-files.html)
- [app.use(errorHandler)](https://expressjs.com/en/guide/error-handling.html)


## Exercise: In-App Easter Egg
1. Create a simple express app that includes the following routes:
  - /home
  - /see-our-chickens
  - /see-our-eggs

2. Create a directory in your project for your simple html pages. Each route should have its own webpage.

3. Use a middleware to let your app know which static (cough, hint) html files it should use.

4. Create your own middleware that will place an Easter Egg in your app (see below for specs).

5. Create a middleware that 'catches' the end of the stream if the requested route doesn't match your three defined routes and sends an error back to the browser with `res.send()`.

### Output
If the user goes to any of your routes, they should see the corresponding html page. If they go to a url that contains the word 'egg', the terminal console should display the following:

```
You found the Easter Egg at Mon Sep 12 2016 15:36:57 GMT-0500 (CDT)

        ,ggadddd8888888bbbbaaa,_
     ,ad888,      `Y88,      `Y888baa,
   ,dP"  "Y8b,      `"Y8b,      `"Y8888ba,
  ,88      "Y88b,      `"Y8ba,       `"Y88Ya,
 ,P88b,      `"Y88b,       `"Y8ba,_       ""8a,
,P'"Y88b,        "Y88b,        `"Y8ba,_      `Ya,
8'    "Y88b,        ""Y8ba,         ""Y8ba,_   `8,
8b       "Y88b,         ""Y8ba,_         ""Y88baaY
88b,        "Y88ba,         ""Y88ba,_         `""P
8Y88ba,        ""Y88ba,_         ""Y88ba,,_    ,P'
`b,"Y88ba,         ""Y88baa,_         """Y888bd"
 `b, `"Y88ba,_         ""Y888baa,_         ,8"
  `8,   `""Y88ba,_         `"""Y8888baaaaaP"
   `Ya,     `""Y888ba,_           `"d88P"
     `"Yb,,_     `""Y888baa,__,,adP""'
         `"""YYYY8888888PPPP"""'
```
Experiment with where in your file you place the middelware functions. Does it make a difference if one comes before the other? What if they run before or after you tell Express where to find your static files?

## Additional Reading

-   [app.use()](http://expressjs.com/en/api.html#app.use)
-   [Express Middleware](https://expressjs.com/en/resources/middleware.html)
-   [Body Parser](https://expressjs.com/en/resources/middleware/body-parser.html)
-   [Write Your Own Middleware](https://expressjs.com/en/guide/writing-middleware.html)
-   [What's Inside Req and Res?](http://www.murvinlai.com/req-and-res-in-nodejs.html)

[exercise 5]: https://github.com/nashville-software-school/node-milestones/blob/master/02-db-driven-applications/exercises/04-express-static.md
