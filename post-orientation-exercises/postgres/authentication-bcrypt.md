# Authentication App with Bcrypt

For this exercise, you will build a simple authentication app. Make sure to have read the previous resource file on [authentication](authentication-101.md). All this app will do is give the user the ability to register with credentials, and then once successfully registered, log in with those same credentials.
Nothing more, nothing less.


## Setup

```
mkdir -p ~/workspace/node/exercises/auth-w-bcrypt && cd $_
npm init --yes
npm install express pug body-parser bcrypt sequelize pg --save
mkdir views public
touch server.js
```

## Instructions

A friend has come and asked you to build an authentication module for their app. All that it needs to do is let users register and login. Users should be asked to register / login with a username and password combination. Once the user successfully logs in, display a welcome message.

Using the npm module `bcrypt`, your friend wants you to hash user passwords and store the username and secure hash in a `PostgreSQL` database. Take a few minutes to read over the [`bcrypt`](https://www.npmjs.com/package/bcrypt) docs on npm.

Be sure to use `body-parser` which gives the `req` object a body property that contains the sent form data (e.g. req.body).


## Requirements

1. Create `login.pug`, `register.pug` and `home.pug` files. The login and register files should each have forms asking for user credentials.

1. The `home.pug` file should at least contain an `h1` tag welcoming the user once they successfully login.

1. **Use the async `bcrypt` methods**

1. When a new user is registering use the `bcrypt` module to hash the password.

1. When registration is successful, store the username and hashed password in a `user` table in the database.

1. When a user is logging in with their credentials, use the `bcrypt` compare method to compare the hashed password used when logging in against the stored hashed value in the database.

1. If the user does not enter both username and password on either the login or the register form, send the user an error message.

1. If the user enters an incorrect username and password combination on either the login or the register form, send the user an error message.

1. When a user successfully logs in, render `home.pug` that greets the user.


# Bonus Features

1. Now instead of using the async `bcrypt` methods, use the promise implementation of `bcrypt`.

1. Once the user logs in, send the user's username to the `home.pug` file.

1. If each `.pug` file is its own HTML file, break the html out into separate templates using the `extends`, `block`, and `includes` keywords.
