[Official Docs](http://docs.sequelizejs.com/manual/tutorial/querying.html#operators)

We often want to specify comparisons like "greater than", "less than" in our `where` clauses.

In sequelize, we need to use special properties called "operators" to do this. Here are some of the most common. (Note: there are of course many more operators - there's no need to memorize all of them, but be sure to read through them so that you have an idea of what you can do!)

*Note*: Up until very recently, we used regular object properties to refer to operators. In upcoming versions of Sequelize, these will be replaced by Symbols that must be obtained from Sequelize, like so:

```javascript
// Sequelize stores these operators on the `Sequelize.Op` module:
const Op = Sequelize.Op

Pug.findAll({
  where: {
    age: {
      [Op.lte]: 7 // square brackets are needed for property names that aren't plain strings
    }
  }
})
```

The examples below demonstrate using operators as regular object properties, with the Symbol equivalent in an adjacent comment.

Here is a list of some of the more commonly used operators, and their usage:

* $gt: Greater than // soon to be replaced by [Op.gt]
* $gte: Greater than or equal // soon to be replaced by [Op.gte]
* $lt: Less than // soon to be replaced by [Op.lt]
* $lte: Less than or equal // soon to be replaced by [Op.lte]
* $ne: Not equal // soon to be replaced by [Op.ne]
* $eq: Equal // soon to be replaced by [Op.eq]
* $or: Use or logic for multiple properties // soon to be replaced by [Op.or]


```javascript
// gt, gte, lt, lte

// SELECT * FROM pugs WHERE age <= 7
Pug.findAll({
  where: {
    age: {
      $lte: 7 // soon to be replaced by [Op.lte]
    }
  }
})
```

```javascript
// $ne

// SELECT * FROM pugs WHERE age != 7
Pug.findAll({
  where: {
    age: {
      $ne: 7 // soon to be replaced by [Op.ne]
    }
  }
})
```

```javascript
// $or

// SELECT * FROM pugs WHERE age = 7 OR age = 6
Pug.findAll({
  where: {
    age: {
      $or: [ // soon to be replaced by [Op.or]
        {$eq: 7}, // soon to be replaced by [Op.eq]
        {$eq: 6} // soon to be replaced by [Op.eq]
      ]
    }
  }
})
```
