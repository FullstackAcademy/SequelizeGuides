[Official Docs](http://docs.sequelizejs.com/manual/tutorial/models-definition.html)

To define mappings between a model and a table, use the `define` method.

```js
const Pug = db.define('pugs', {
  name: Sequelize.STRING,
  cute: Sequelize.BOOLEAN,
  age: Sequelize.INTEGER,
  bio: Sequelize.TEXT
})
```

You can also set some options on each column:

```js
const Pug = db.define('pugs', {
  name: {
    type: Sequelize.STRING
    allowNull: false // name MUST have a value
  },
  cute: {
    type: Sequelize.BOOLEAN,
    defaultValue: true // instantiating will automatically set "cute" to true if not set
  },
  bio: Sequelize.TEXT,
  age: {
    type: Sequelize: INTEGER,
    validate: {
      min: 0,
      max: 100
      // note: many validations need to be defined in the "validate" object
      // allowNull is so common that it's the exception
    }
  }
})
```