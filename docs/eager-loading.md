# Joins/Includes (aka "Eager Loading")
[Official Docs](http://docs.sequelizejs.com/manual/tutorial/models-usage.html#eager-loading)

If we have two tables that are associated with each other, we often want to join that data together. In raw SQL queries, our favorite tool for this is an INNER JOIN. We can do something similar in Sequelize - it just goes by the slightly different name of "eager loading". Don't get hung up on the terminology - when you see "eager loading", think "join two tables".

If two tables have an association, we can "include" information from the associated table like so:

```js
const Pug = db.define('pugs', {name: Sequelize.STRING})
const Owner = db.define('owners', {name: Sequelize.STRING})

Pug.belongsTo(Owner)

const getPugs = async () => {
  const pugs = await Pug.findAll({ // we want to find all the pugs, and include their owners
    include: [{model: Owner}]
  })

  console.log(pugs)
  // [{name: 'Cody', ownerId: 1, owner: {name: 'Tom'}}, ...etc]
}
```

*Important!* A Model can only eagerly load an association if the association is defined *on* that model.

This means that for the above example, if we attempt to do the following:

```js
const Pug = db.define('pugs', {name: Sequelize.STRING})
const Owner = db.define('owners', {name: Sequelize.STRING})

// the relation only exists on Pug
Pug.belongsTo(Owner)

Owner.findAll({include: [{model: Pug}]}) // this will error!
```

...it will error! For us to include Pug when we query Owner, we must also establish the 'hasOne' or 'hasMany' association between Owners and Pugs.

Example with `hasOne`:

```javascript
const Pug = db.define('pugs', {name: Sequelize.STRING})
const Owner = db.define('owners', {name: Sequelize.STRING})

Pug.belongsTo(Owner)
Owner.hasOne(Pug) // 1-1 association

const getPugs = async () => {
  const owners = await Owner.findAll({include: [{model: Pug}]})
  console.log(owners) // [{name: 'Tom', pug: {name: 'Cody', ownerId: 1}}]
}
```

Example with `hasMany`:

```javascript
const Pug = db.define('pugs', {name: Sequelize.STRING})
const Owner = db.define('owners', {name: Sequelize.STRING})

Pug.belongsTo(Owner)
Owner.hasMany(Pug) // 1-Many Relationship

const getPugs = async () => {
  const owners = await Owner.findAll({include: [{model: Pug}]})
  console.log(owners) // [{name: 'Tom', pugs: [{name: 'Cody', ownerId: 1}]}]
}
```

Note that the difference between the two examples above is that in the `hasOne` case, the resultant owners have a "pug" field with the name of their (single) pug. In the `hasMany` case, the resultant owners have a "pugs" (plural!) field with an _array_ of their (possibly many) pugs!

This same rule applies to many-to-many associations!
