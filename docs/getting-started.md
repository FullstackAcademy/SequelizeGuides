# Installation

Sequelize is available via NPM.

```bash
$ npm install sequelize

# And one of the following:
$ npm install pg@6 pg-hstore #pg@7 is currently not supported
$ npm install mysql2
$ npm install sqlite3
```

# Setting up a connection

Sequelize allow you to use a connection uri:

```js
const Sequelize = require('sequelize')
const db = new Sequelize('postgres://localhost:5432/your-db')
```

# Your first model

Models are defined with the `.define()` method: `db.define('name', {attributes}, {options})`.

```js
const User = db.define('user', {
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

# Your first query

```js
const users = await User.findAll();

console.log(users);
```



### Instances vs data

When you console log the returned value from a Sequelize statement (just like in the example above), what you see is a weird looking object like this:

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

This is not a simple object containing only data, this is a full instance of the given model: It contains the data you queried plus a bunch of additional information and methods.

Notice that the actual data is available inside `dataValues`, but don't worry about this key - Sequelize creates getters and setters automatically for you - In plain english this means that you can simply type the desired field name directly:

```js
  user.firstName // 'John'
```


## Promises

Sequelize uses promises to control async control-flow.

**Note:** _Internally, Sequelize uses an independent copy of [Bluebird](http://bluebirdjs.com). You can access it using
 `Sequelize.Promise` if you want to set any Bluebird-specific options_.


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

