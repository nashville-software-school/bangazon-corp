# Queries with ORMS

Once you have your models set up, writing queries in bookshelf is simpler than writing queries in raw SQL.

Check out the following examples.

```
//SQL
SELECT * FROM monsters

//Bookshelf (after a model has been created)
Monster.forge().fetchAll()

//Bookshelf (after a collection has been created)
Monsters.forge().fetch()

```

Helpful tip: If you pass `debug: true` as one of the options in your initialize settings, you can see all of the query calls being made. You can do this in the `development` section of your knexfile:
```
development: {
  client: 'pg',
  debug: true,
  connection: 'postgres://localhost/foo',
  migrations: {
    directory: __dirname + '/db/migrations'
  }
}
```
Then, on each request you will get an output in your terminal that looks something like this:  
```
{ method: 'select',
  options: {},
  timeout: false,
  cancelOnTimeout: false,
  bindings: [],
  __knexQueryUid: '0aa7bb57-0182-46e9-9275-97c0120d8c85',
  sql: 'select * from "shows"' }
```
