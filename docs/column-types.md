[Official Docs](http://docs.sequelizejs.com/manual/tutorial/models-definition.html#data-types)

The example below demonstrates some common data types:

```javascript
const Pug = db.define('pugs', {
  name: {
    type: Sequelize.STRING // for shorter strings (< 256 chars)
  },
  bio: {
    type: Sequelize.TEXT // for longer strings
  },
  age: {
    type: Sequelize.INTEGER
  },
  birthday: {
    type: Sequelize.DATE
  },
  colors: {
    type: Sequelize.ENUM('black', 'fawn')
  },
  toys: {
    type: Sequelize.ARRAY(Sequelize.TEXT) // an array of text strings (Postgres only)
  },
  adoptedStatus: {
    type: Sequelize.BOOLEAN
  }
})
```
