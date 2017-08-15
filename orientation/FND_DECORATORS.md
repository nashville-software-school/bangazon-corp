# Python Method Decorators

If you believe you had a good comprehension of how function were first order objects in JavaScript, then Python decorators should be fairly easy to grasp. This is because a Python decorator is simply a function that accepts a function as an argument, and then returns another function.

## How it looks in JavaScript

Here's the mechanism in JavaScript. A function reference is passed to another, decorating function. The decorating function returns a new function that, itself, invokes the original function and decorates its return value.

```js
// Returns a simple string
function message () {
  return 'Nashville Software School';
}

/*
  Decorator function that will invoke any original function,
  and wrap an h1 tag around its return value
*/
function heading (messageFunction) {
  return function final_function () {
    return `<h1>${messageFunction()}</h1>`;
  }
}


final_function_reference = heading(message);
decorated_value = final_function_reference(); // "<h1>Nashville Software School</h1>"
```
