Instance and class methods are common ways to add functionality to your Sequelize.js models.

# Instance Methods

The code example below demonstrates an instance method.
Instance methods are methods that are available on *instances* of the model.
We often write these to get information or do something related *to that instance*.

## Definition:
```javascript
const Pug = db.define('pugs', {/* etc*/})

// instance methods are defined on the model's .prototype
Pug.prototype.celebrateBirthday = function () {
  // 'this' in an instance method refers to the instance itself
  const birthday = new Date(this.birthday)
  const today = new Date()
  if (birthday.getMonth() === today.getMonth() && today.getDate() === birthday.getDate()) {
    console.log('Happy birthday!')
  }
}
```

## Usage:

```js
const createdPug = await Pug.create({name: 'Cody'}) // let's say `birthday` defaults to today
// the instance method is invoked *on the instance*
createdPug.celebrateBirthday() // Happy birthday!
```


### Class Methods

The code example below demonstrates a class method.
Class methods are methods that are available on the *model itself* (aka the _class_).
We often write these to get instances, or do something to more than one instance

##### Definition

```javascript
const Pug = db.define('pugs', {/* etc*/})

// class methods are defined right on the model
Pug.findPuppies = function () {
  // 'this' refers directly back to the model (the capital "P" Pug)
  return this.findAll({ // could also be Pug.findAll
    where: {
      age: {$lte: 1} // find all pugs where age is less than or equal to 1
    }
  })
}
```

##### Usage
```javascript
const foundPuppies = await Pug.findPuppies()
console.log('Here are the pups: ', foundPuppies)
```
