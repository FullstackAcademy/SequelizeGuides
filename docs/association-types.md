[Official Docs](http://docs.sequelizejs.com/manual/tutorial/associations.html)

Associations in Sequelize establish three things:

1. For one-one or one-many associations, it establishes a foreign key relationship between two tables (though a table could be related to itself). For many-many associations, it establishes a join-table that contains pairs of foreign keys.
2. It creates several special instance methods (like "getAssociation", "setAssociation", and potentially others depending on the type of association) that an instance can use to search for the instances that they are related to
3. It creates the ability to use "include" in queries to include data about related rows (known as "eager loading")

## Types of Associations

It is possible to specify the following associations in Sequelize:

* `belongsTo`
* `hasOne`
* `hasMany`
* `belongsToMany`

These relations can be combined to establish *one-one*, *one-many* and *many-many* relationships, like so:

* `one-one`
  * `Collar.belongsTo(Pug)`
  * `Pug.hasOne(Collar)`

* `one-many`
  * `Pug.belongsTo(Human)`
  * `Human.hasMany(Pug)`

* `many-many`
  * `Pug.belongsToMany(Treat, {through: 'PugTreats'})`
  * `Treat.belongsToMany(Pug, {through: 'PugTreats'})`

## Methods Gained from Associations

By defining the associations between models we gain a series of methods for each model.

### hasOne():

When we set `Human.hasOne(Pug)`, we gain these methods on each instance of the human:

```
human.getPug()
human.setPug()
human.addPug()
human.createPug()
human.removePug()
human.hasPug()
```

### hasMany():

When we set `Human.hasMany(Pug)`, we gain these methods on each instance of the human:
```
human.getPugs()
human.setPugs()
human.addPug()
human.createPug()
human.removePug()
human.hasPug()
human.hasPugs()
```

### belongsTo() & belongsToMany:

When we set `Pug.belongsTo(Human)`, we gain these methods on each instance of the pug:
```
// belongsTo()
pug.getHuman()
pug.setHuman()
pug.createHuman()

// belongsToMany
pug.getHumans()
pug.setHumans()
pug.createHumans()
```