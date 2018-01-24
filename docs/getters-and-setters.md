[Official Docs](http://docs.sequelizejs.com/manual/tutorial/models-definition.html#getters-setters)

Getters and setters are ways of customizing what happens when someone 'gets' or 'sets' a property on an instance. 'Get' and 'set' are referred to as "meta-operations" in JavaScript.

```javascript
const someObj = {foo: 'bar'}
someObj.foo // the 'get' meta-operation
someObj.foo = 'baz' // the 'set' meta-operation
```

Normally, we expect that 'getting' a property will simply return the value at that key in the object, and 'setting' a property will set that property in the object.

'Getters' and 'setters' allow us to *override* that expected behavior.

## Definition

```javascript
const Pug = db.define('pugs', {
  name: {
    type: Sequelize.STRING,
    get () { // this defines the 'getter'
      // 'this' refers to the instance (same as an instance method)
      // in a 'getter', you should not refer to the names of the columns directly
      // as this will recursively call the getter and result in a stack overflow,
      // instead, use the `this.getDataValue` method
      return this.getDataValue('name') + ' the pug'
      // this getter will automatically append ' the pug' to any name
    },
    set (valueToBeSet) { // defines the 'setter'
      // 'this' refers to the instance (same as above)
      // use `this.setDataValue` to actually set the value
      this.setDataValue('name', valueToBeSet.toUpperCase())
      // this setter will automatically set the 'name' property to be uppercased
    }
  }
})
```

## Usage

```javascript
// building or creating an instance will trigger the 'set' operation, causing the name to be capitalized
const createdPug = await Pug.create({name: 'cody'})
  
// when we 'get' createdPug.name, we get the capitalized 'CODY' + ' the pug' from our getter
console.log(createdPug.name) // CODY the pug

// this is the 'set' operation, which will capitalize the name we set
createdPug.name = 'murphy'
console.log(createdPug.name) // MURPHY the pug
  
```
