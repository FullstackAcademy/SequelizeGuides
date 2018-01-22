A one-one association is established by pairing a `belongsTo` and a `hasOne` association (though the `hasOne` is often omitted).

Say we have two model tables, `Pug` and an `Owner`. We might associate them like so:

```javascript
Pug.belongsTo(Owner)
Owner.hasOne(Pug)
```

This means that a pug belongs to an owner, and an owner has one (and only one) pug.

By doing this, the following three things will happen/be available to use:

1. The Pug table will have a foreign key column, "ownerId", corresponding to a primary key in the Owner table.

*Pugs* - includes an ownerId!
```
id | name | createdAt | updatedAt | ownerId
```

*Owner* - no changes!
```
id | name | createdAt | updatedAt
```

2. Sequelize automatically creates two instance methods for pugs, "getOwner" and "setOwner". This is because we defined `Pug.belongsTo(Owner)`. Likewise, owners get two instance methods, "getPug" and "setPug" (because we defined `Owner.hasOne(Pug)`).

```javascript
pug.getOwner() // returns a promise for the pug's owner

pug.setOwner(ownerInstanceOrID) // updates the pug's ownerId to be the id of the passed-in owner, and returns a promise for the updated pug

owner.getPug() // returns a promise for the owner's pug

owner.setPug(pugInstanceOrID) // updates the passed-in pug's ownerId to be the id of the owner, and returns a promise for the updated pug
```

3. Sequelize will allow us to "include" the pug's owner, or the owner's pug in queries.

```javascript
const pugsWithTheirOwners = await Pug.findAll({
  include: [{model: Owner}]
})

console.log(pugsWithTheirOwners[0])
// looks like this:
// {
//   id: 1,
//   name: 'Cody',
//   ownerId: 1,
//   owner: {
//     id: 1,
//     name: 'Tom'
//   }
// }
```

Likewise (but note that if we do omit the `Owner.hasOne(Pug)`, this is not possible):

```javascript
const ownersWithTheirPug = await Owner.findAll({
  include: [{model: 'Pug'}]
})

console.log(ownersWithTheirPug[0])
// looks like this:
// {
//   id: 1,
//   name: 'Tom',
//   pug: {
//     id: 1,
//     name: 'Cody',
//     ownerId: 1
//   }
// }

```