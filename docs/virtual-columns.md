"Virtual" columns are columns that *do not* get saved in your database - they are calculated on the fly based on the values of other columns. They are helpful for saving space if there are values we want to use on our instances that can be easily calculated.

Virtual columns always have the data type of Sequelize.VIRTUAL.

Virtual columns must have *at least* one custom 'getter' or 'setter' to be useful. This does not mean that getters and setters can _only_ be used with virtual columns though (see above).

Virtual columns are similar to instance methods. The difference is you access virtual columns the same way you access a regular property (via the 'get' and 'set' meta-operation), whereas instance methods are functions that you must invoke.

##### Definition

```javascript
const Pug = db.define('pugs', {
  firstName: {
    type: Sequelize.STRING,
  },
  lastName: {
    type: Sequelize.STRING
  },
  fullName: {
    type: Sequelize.VIRTUAL,
    get () {
      return this.getDataValue('firstName') + ' ' + this.getDataValue('lastName')
    }
  }
})
```

##### Usage

```javascript
const pug = await Pug.create({firstName: 'Cody', lastName: 'McPug'})
  
console.log(pug.fullName) // "Cody McPug"
// however, if you look inside your database, there won't be a "fullName" column!
```
