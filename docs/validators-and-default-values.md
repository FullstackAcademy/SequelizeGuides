[Official Docs](http://docs.sequelizejs.com/manual/tutorial/models-definition.html#validations)

The example below demontrates usage for important validations (allowNull, min/max) and also demonstrates how to set a default value (defaultValue)

```javascript
const Pug = db.define('pugs', {
  name: {
    type: Sequelize.STRING
    allowNull: false // name MUST have a value
  },
  bio: {
    name: Sequelize.TEXT
  },
  age: {
    type: Sequelize.INTEGER,
    validate: {
      min: 0,
      max: 100
      // note: many validations need to be defined in the "validate" object
      // allowNull is so common that it's the exception
    }
  },
  birthday: {
    type: Sequelize.DATE,
    defaultValue: Date.now()
    // if no birthday is specified when we create the row, it defaults to right now!
  },
  colors: {
    type: Sequelize.ENUM('black', 'fawn')
  },
  toys: {
    type: Sequelize.ARRAY(Sequelize.TEXT)
  },
  adoptedStatus: {
    type: Sequelize.BOOLEAN
  }
})
```
