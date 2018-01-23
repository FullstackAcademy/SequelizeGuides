# Querying Using Models

## Model.findOne
[Official Docs](http://docs.sequelizejs.com/manual/tutorial/models-usage.html#-find-search-for-one-specific-element-in-the-database)

Finds a single instance that matches the search criteria (even if there are more than one that match the search criteria - it will return the first it finds)

```javascript
const foundPug = await Pug.findOne({
  where: {name: 'Cody'}
})

console.log(foundPug)

```

## Model.findById
[Official Docs](http://docs.sequelizejs.com/manual/tutorial/models-usage.html#-find-search-for-one-specific-element-in-the-database)

Finds the instance with the specified id.

```javascript
pugWithIdOne = await Pug.findById(1)

console.log(pugWithIdOne)
```
## Model.findAll
[Official Docs](http://docs.sequelizejs.com/manual/tutorial/models-usage.html#-findall-search-for-multiple-elements-in-the-database)

Finds all instances that match the search criteria. If no criteria are given, it returns all the instances in the table.

```javascript
const allPugs = await Pug.findAll() // will find ALL the pugs!
  
console.log(allPugs)  
```

### "Select" Clauses ("attributes")
[Official Docs](http://docs.sequelizejs.com/manual/tutorial/querying.html#attributes)

You can select specific columns to be included in the returned instance by specifying an "attributes" array.

```javascript
const allPugs = await Pug.findAll({
  attributes: ['id', 'name', 'age'] // like saying: SELECT id, name, age from pugs;
})
  
console.log(allPugs) // [{id: 1, name: 'Cody', age: 7}, {id: 2, name: "Murphy", age: 4}]
// note that all the pugs only have key-value pairs for id, name and age included

```

### "Where" Clauses
[Official Docs](http://docs.sequelizejs.com/manual/tutorial/querying.html#where)

You can narrow down the search in a `findAll` by specifying a `where` clause

```javascript
const sevenYearOldPugs = await Pug.findAll({
  where: { // like saying: SELECT * from pugs WHERE age = 7;
    age: 7,
  }
})

console.log(sevenYearOldPugs)
```

Specifying multiple options uses "AND" logic

```javascript
const sevenYearOldBlackPugs = await Pug.findAll({
  where: { // like saying: SELECT * from pugs WHERE age = 7 AND color = 'black';
    age: 7,
    color: 'black'
  }
})

console.log(sevenYearOldBlackPugs)

```

**Note**: To specify comparisons like "greater than" and "less than", Check out [Search Operators](/search-operators).