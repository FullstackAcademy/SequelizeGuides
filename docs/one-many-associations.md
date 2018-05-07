A one-many association is established by pairing a `belongsTo` and a `hasMany` relation (though like `hasOne`, the `belongsTo` is sometimes omitted).

Given our Pug and Owner, we might allow an owner to have multiple pugs like so:

```javascript
Pug.belongsTo(Owner)
Owner.hasMany(Pug)
```

This means that a pug belongs to an owner, and an owner can have many pugs (also known as a [grumble](https://www.google.com/search?q=grumble+of+pugs&ie=utf-8&oe=utf-8&client=firefox-b-1-ab)).

By doing this, the following three things will happen/be available to use:

1. The Pug table will have a foreign key column, "ownerId", corresponding to a primary key in the Owner table. *Note: this is the same as a one-one association*.

*Pugs* - includes an ownerId!
```
id | name | createdAt | updatedAt | ownerId
```

*Owner* - no changes!
```
id | name | createdAt | updatedAt
```

2. Sequelize automatically creates three instance methods for pugs, "getOwner", "setOwner", and "createOwner". This is because we defined `Pug.belongsTo(Owner)`. Likewise, owners get a bunch of new instance methods, "getPugs", "setPugs", "createPug", "addPug", "addPugs", "removePug", "removePugs", "hasPug", "hasPugs", and "countPugs" (because we defined `Owner.hasMany(Pug)`). *Note: the difference here from one-one is that the owner's methods are now pluralized, and return promises for arrays of pugs instead of just a single pug!*

```javascript
pug.getOwner() // returns a promise for the pug's owner

pug.setOwner(owner) // updates the pug's ownerId to be the id of the passed-in owner, and returns a promise for the updated pug

owner.getPugs() // returns a promise for an array of all of the owner's pugs (that is, all pugs with ownerId equal to the owner's id)

owner.setPugs(arrayOfPugs)
// updates each pug in the passed in array to have an ownerId equal to the owner's id.
// returns a promise for the owner (NOT the pugs)
```

3. Sequelize will allow us to "include" the pug's owner, or the owner's pugs in queries.

```javascript
// this is the same as one-one
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

Likewise (but note that if we do omit the `Owner.hasMany(Pug)`, this is not possible):

```javascript
// the difference is that instead of a "pug" field, the owner has a "pugs" field, which is an array of all that owner's pugs
const ownersWithTheirPug = await Owner.findAll({
  include: [{model: 'Pug'}]
})

console.log(ownersWithTheirPug[0])
// looks like this:
// {
//   id: 1,
//   name: 'Tom',
//   pugs: [{
//     id: 1,
//     name: 'Cody',
//     ownerId: 1
//   }]
// }
```
