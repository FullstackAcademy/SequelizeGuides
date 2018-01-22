# Getting started

## Installation

Sequelize is available via NPM.

```bash
// Using NPM
$ npm install --save sequelize

# And one of the following:
$ npm install --save pg@6 pg-hstore #pg@7 is currently not supported
$ npm install --save mysql2
$ npm install --save sqlite3
$ npm install --save tedious // MSSQL
```

## Setting up a connection

Sequelize allow you to use a connection uri:

```js
const Sequelize = require('sequelize')
const db = new Sequelize('postgres://localhost:5432/your-db')
```

## Your first model

Models are defined with `sequelize.define('name', {attributes}, {options})`.

```js
const User = sequelize.define('user', {
  firstName: {
    type: Sequelize.STRING
  },
  lastName: {
    type: Sequelize.STRING
  }
});

// force: true will drop the table if it already exists
await User.sync({force: true})
// Table created

await User.create({
  firstName: 'John',
  lastName: 'Hancock'
})


```

You can read more about creating models and available types at [Column Types](/column-types)

## Your first query

```js
const users = await User.findAll();

console.log(users);
```



### Entities vs data

When you console log the returned value from a Sequelize statement just like in the example above, you will see a weird looking object like this:

```js
{
    dataValues:
     { id: 16,
       firstName: 'John',
       lastName: 'Hancock',
       createdAt: 2018-01-22T19:18:39.875Z,
       updatedAt: 2018-01-22T19:18:39.879Z,
       authorId: 6 },
    _changed: {},
    _modelOptions:
     { ... }, // Omitted for brevity
    _options:
     { ... },  // Omitted for brevity
    __eagerlyLoadedAssociations: [],
    isNewRecord: false } 

```

This is not an object containing only your data, this is a full instance of that model: It contains the data you queried plus a bunch of additional information and methods.

Notice that the data is inside a `dataValues` key, but don't worry about this key - Sequelize creates getters and setters automatically for you wich, in plain english, means you can simply type:

```js
  user.firstName // 'John'
```


## Promises

Sequelize uses promises to control async control-flow.

**Note:** Internally, _Sequelize use independent copy of [Bluebird](http://bluebirdjs.com). You can access it using
 `Sequelize.Promise` if you want to set any Bluebird specific options_.


Basically, a promise represents a value which will be present at some point - "I promise you I will give you a result or an error at some point". This means that

```js
// DON'T DO THIS
user = User.findOne()

console.log(user.get('firstName'));
```

_will never work!_ This is because `user` is a promise object, not a data row from the DB. The right way to do it is:

```js
user = await User.findOne()

console.log(user.get('firstName'));
```

**Note:**  this will work but only in the body of an [async](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/async_function) function:

