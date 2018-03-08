# Model.findOrCreate
[Official Docs](http://docs.sequelizejs.com/manual/tutorial/models-usage.html#-findorcreate-search-for-a-specific-element-or-create-it-if-not-available)

Finds an instance that matches the specified query. If no such instance exists, it will create one. This method is a little funny - it returns a promise for an array! The first element of the array is the instance. The second element is a boolean (true or false), which will be true if the instance was newly created, and false if it wasn't (that is, an existing match was found).

In the example below, assume we do not have any existing row in our pugs table with the name 'Cody'.

```javascript

const arr = await Pug.findOrCreate({where: {name: 'Cody'}})  
const instance = arr[0] // the first element is the instance
const wasCreated = arr[1] // the second element tells us if the instance was newly created
console.log(instance) // {id: 1, name: 'Cody', etc...}
console.log(wasCreated) // true


// now if we findOrCreate a second time...

const arr2 = Pug.findOrCreate({where: {name: 'Cody'}}) 
const instance = arr2[0]
const wasCreated = arr2[1]
console.log(instance) // {id: 1, name: 'Cody', etc...}
console.log(wasCreated) // false -> the query found an existing pug that matched the query
```

It's often preferably to handle with a promise for an array using destructuring assignment:

```javascript
const [instance, wasCreated] = await Pug.findOrCreate({where: {name: 'Cody'}})
```

For other examples of this pattern: http://es6-features.org/#ParameterContextMatching

# new Model()

Creates a temporary instance. Returns the instance. This instance is NOT yet saved to the database!

```javascript
const cody = new Pug({name: 'Cody'})
console.log(cody) // we can start using cody right away...but cody is NOT in our db yet
```

**Note:** This operation is synchronous - you should not use the `await` keyword.


## Model.build

Same as above (with alternative syntax) - creates a temporary, unsaved instance.

```javascript
const cody = Pug.build({name: 'Cody'})
console.log(cody) // we can start using cody right away...but cody is NOT in our db yet
```

**Note:** This operation is synchronous - you should not use the `await` keyword.


# Model.create

Creates and saves a new row to the database. Returns a promise for the created instance.

```javascript
const cody = await Pug.create({name: 'Cody'})

// now, cody is saved in our database
console.log(cody)
```

# Model.update

Updates all instances that match a query.
Takes two parameters: the first parameter contains the info you want to update. The second parameter contains the query for which instances to update.

Like `findOrCreate`, it returns a promise for an array. The first element of the array is the number of rows that were affected. The second element of the array is the affected rows themselves.

```javascript
// update all pugs whose age is 7 to have an adoptedStatus of true
// because we return a promise for an array, destructuring is recommended
const [numberOfAffectedRows, affectedRows] = await Pug.update({ 
  adoptedStatus: true
}, {
  where: {age: 7},
  returning: true, // needed for affectedRows to be populated
  plain: true // makes sure that the returned instances are just plain objects
})

console.log(numberOfAffectedRows) // say we had 3 pugs with the age of 7. This will then be 3
console.log(affectedRows) // this will be an array of the three affected pugs

```

# Model.destroy

Destroys all instances that match a query.
It returns a promise for the number of rows that were deleted.

```javascript
const numAffectedRows = await Pug.destroy({
  where: {
    age: 7 // deletes all pugs whose age is 7
  }
})

console.log(numAffectedRows) // if we had 3 pugs with the age of 7, this will be 3

```

---

# Using Instances

## instance.save and instance.update
[Official Docs](http://docs.sequelizejs.com/manual/tutorial/instances.html#updating-saving-persisting-an-instance)

If we already have an instance, we can save changes with either instance.save or instance.update

Both returns promises for the saved/updated instance

Here's an example using save:
```javascript
// we already have a pug instance, which we've put in a variable called cody
console.log(cody.age) // 7
cody.age = 8 // we can change the age to 8 (but it isn't saved in the database yet)
cody = await cody.save() // we can use .save to actually save to the database

console.log(cody.age) // 8
```

Here's another example using update:
```javascript
console.log(cody.age) // 7
cody = await cody.update({age: 8})
  
console.log(cody.age) // 8  
```

## instance.destroy
[Official Docs](http://docs.sequelizejs.com/manual/tutorial/instances.html#destroying-deleting-persistent-instances)

If we want to remove an individual instance from the database, we can use instance.destroy.
It returns a promise that will be resolved when the row is removed from the database.
(The promise itself does not resolve to anything in particular though - it's always just an empty array)

```javascript
// no need to expect the promise to resolve to any useful value
await cody.destroy() // no! bye Cody!
  
// now the cody row is no longer in the database  
```

