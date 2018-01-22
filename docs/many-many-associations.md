One-One and One-Many associations are very similar. Many-Many associations are different! Instead of placing a foreign key in one table, they create a brand new "join" table where each row contains a foreign key for each entity in the relationship.

For our example, let's introduce a new table, Friend:

```js
const Friend = db.define('friends', {
  name: Sequelize.STRING
})
```

A "friend" is a (human) person that is friends with pugs (but not necessarily a pug Owner themselves)! Any given "friend" may be a friend of many pugs. Likewise, any given pug will have many human friends! This is the nature of a many-to-many relationship.

Here is how we define one:

```js
Friend.belongsToMany(Pug, {through: 'friendship'})
Pug.belongsToMany(Friend, {through: 'friendship'})
```

The "through" parameter defines the name of the join table that gets created.

By establishing the relation above, the following *four* things happen:

1. A brand new table called `"friendship"` is created.

*friendship*
```
createdAt | updatedAt | pugId | pugFriendId
```

No changes occur to the `pug` or `friend` tables!

2. A new Sequelize model becomes automatically generated for the table:

```javascript
const Friendship = db.model('friendship')
```

You can use this model the same way you use any other Sequelize model (you can query it, create instances, etc).

3. Sequelize automatically creates several instance methods for pugs and for friends, the same ones as created by `hasMany` above:

```
pug.getFriends() // returns a promise for the array of friends for that pug
pug.addFriend(friend) // creates a new row in the friendship table for the pug and the friend, returns a promise for the friendship (NOT the pug OR the friend - the "friendship")
pug.addFriends(friendsArray) // creates a new row in the friendship table for each friend, returns a promise for the friendship
pug.removeFriend(friend) // removes the row from the friendship table for that pug-friend, returns a promise for the number of affected rows (as if you'd want to destroy any friendships...right?)
pug.removeFriends(friendsArray) // removes the rows from the friendship table for those pug-friend pairs, returns a promise for the number affected rows

// analogous to above ^
friend.getPugs()
friend.addPug(pug)
friend.addPugs(pugsArray)
friend.setPugs(pugsArray)
friend.removePug(pug)
friend.removePugs(pugsArray)
```

4. Allows us to include a pug's friends, or a friend's pugs

```javascript
const pugsWithFriends = await Pug.findAll({
  include: [{model: Friend}]
})

console.log(pugsWithFriends[0])
// looks like this:
// {
//  id: 1,
//  name: 'Cody',
//  friends: [
//    {
//      id: 1,
//      name: 'CodyFan <3'
//    },
//    ....any many more!
//  ]
// }

```

The inverse also applies if we `Friend.findAll({include: [{model: Pug}]})`
