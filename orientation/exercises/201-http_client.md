# HTTP Client
The `http` module is part of Node core, and it contains all the building blocks for creating a web server with remarkable ease, which we will do in our next exercise. It also exposes a simple shorthand 'convenience' method called [`get`][get]. As you might imagine, it is used for making GET requests and resolving the response with little muss or fuss. `get` takes two arguments: a url and a callback function that itself takes an argument of the respsonse object.

Both the request and the response are streams, allowing you to process the incoming data in chunks. This example is from the Node.js docs. Note that it takes two arguments, a URL and a callback.
```
const { get } = require('http');

get('http://nodejs.org/dist/index.json', (res) => {
  const statusCode = res.statusCode;
  const contentType = res.headers['content-type'];

  let error;
  if (statusCode !== 200) {
    error = new Error(`Request Failed.\n` +
                      `Status Code: ${statusCode}`);
  } else if (!/^application\/json/.test(contentType)) {
    error = new Error(`Invalid content-type.\n` +
                      `Expected application/json but received ${contentType}`);
  }
  if (error) {
    console.log(error.message);
    // consume response data to free up memory, since we won't use the request body. Until the data is consumed, the 'end' event will not fire. Also, until the data is read it will consume memory that can eventually lead to a 'process out of memory' error.
    res.resume();
    return;
  }

  res.setEncoding('utf8');
  let rawData = '';
  res.on('data', (chunk) => rawData += chunk);
  res.on('end', () => {
    try {
      let parsedData = JSON.parse(rawData);
      console.log(parsedData);
    } catch (e) {
      console.log(e.message);
    }
  });
}).on('error', (e) => {
  console.log(`Got error: ${e.message}`);
});
```
Notice how most of the code above is about dealing with errors? Errors sent back from the API server; errors from the request itself (ie the request never makes it to the API server); errors parsing the data. Our schedule does not lend itself to doing a deep dive into error handling in Node.js, but if you have plans to be a ninja rockstar unicorn one day, add this to your list of vital skills.  
[Error handling in Node](https://www.joyent.com/node-js/production/design/errors)

We do eventually get to handling the expected data returned from our `get`, though. It's just a matter of setting up handlers to listen for the chunk(s) of data -- `res.on('data', <callback>)` -- and for when the stream of data has dried up -- `res.on('end', <callback>)`.

## Exercise 
Write a program that performs an HTTP GET request to get the average stock
price. Use the first argument for a ticker symbol. Use the `get` method in the
`http` module with the API provided by
[MarkitOnDemand](http://dev.markitondemand.com/).

It would certainly be easier to test if you can grab the latest stock price, but
because the response is so small, there may not be an opportunity to demonstrate
chunking. On the API docs you will see an example request for data to create a
chart. This will give 365 of daily prices. Use these prices to get an average.

Expected:

```bash
$ ./stocks.js AAPL
$123.45
```

## Bonus

-   Avoid using encoded characters in your url: %22%3A%5B%22c%22%5D%7D%5D%7D
-   Full Destructuring on the API response object and `http` module
-   Abstract a getJSON function. (This is good practice for when we write our own APIs):

```js
const getJSON = (url, cb) => { ... }
getJSON('http://example.com', data => { ... })
```

-   Promisify the getJSON function:

```js
const getJSON = url => { ... }
getJSON('http://example.com').then(data => { ... })
```

[get]: https://nodejs.org/api/http.html#http_http_get_options_callback
