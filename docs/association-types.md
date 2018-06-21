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
  * `A.belongsTo(B)`
  * `B.hasOne(A)`

* `one-many`
  * `A.belongsTo(B)`
  * `B.hasMany(A)`

* `many-many`
  * `A.belongsToMany(B, {through: 'AB'})`
  * `B.belongsToMany(A, {through: 'AB'})`

## Methods Gained from Associations

By defining the associations between models we gain a series of methods for each model.

### hasOne():

When we set `Parent.hasOne(Child)`, we gain these methods on each instance of the parent:

```
parent.getChild()
parent.setChild()
parent.addChild()
parent.createChild()
parent.removeChild()
parent.hasChild()
```

### hasMany():

When we set `Parent.hasMany(Child)`, we gain these methods on each instance of the parent:
```
parent.getChildren()
parent.setChildren()
parent.addChild()
parent.createChild()
parent.removeChild()
parent.hasChild()
parent.hasChildren()
```

### belongsTo() & belongsToMany:

When we set `Child.belongsTo(Parent)`, we gain these methods on each instance of the child:
```
// belongsTo()
child.getParent()
child.setParent()
child.createParent()

// belongsToMany
child.getParents()
child.setParents()
child.createParents()
```