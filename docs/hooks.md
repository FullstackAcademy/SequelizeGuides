[Official Docs](http://docs.sequelizejs.com/manual/tutorial/hooks.html)

When we perform various operations in sequelize (like updating, creating, or destroying an instance), that's not _all_ that happens. There are various stages that an instance goes through as it's being updated/created/destroyed. These are called *lifecycle events*. Hooks give us the ability to "hook" into these lifecycle events and execute arbitrary functions related to that instance.

This can be useful in several ways. For example:

* Before creating an instance of a User, we could hash that user's plaintext password, so that what gets saved is the hashed version
* Before destroying an instance of a TodoList, we could also destroy all Todos that are associated with that TodoList.

We define hooks on the model, but they are executed against instances when those instances pass through those lifecycle events.

All hooks are defined using a function that takes two arguments. The first argument is the instance passing through the lifecycle hook. The second argument is an options object (rarely used - you can often ignore it or exclude it).

Here's what the above two examples might look like. Note that there are several different ways that hooks can be defined (but they are mostly equivalent).

```javascript
// given the following User model:
const User = db.define('users', {
  name: Sequelize.STRING,
  password: Sequelize.STRING
})
// we want to hook into the "beforeCreate" lifecycle event
// this lifecycle event happens before an instance is created and saved to the database,
// so we can use this to change something about the instance before it gets saved.

User.beforeCreate((userInstance, optionsObject) => {
  userInstance.password = hash(userInstance.password)
})

// This lifecycle hook would get called after calling something like:
// User.create({name: 'Cody', password: '123'})
// and it would run before the new user is saved in the database.
// If we were to inspect the newly created user, we would see that
// the user.password wouldn't be '123', but it would be another string
// representing the hashed '123'
```

```javascript
// given the following TodoList and Todo models:
const TodoList = db.define('todolists', {
  name: Sequelize.STRING
})

const Todo = db.define('todo', {
  name: Sequelize.STRING,
  completedStatus: Sequelize.BOOLEAN
})

Todo.belongsTo(TodoList, {as: 'list'})

// we want to hook into the "beforeDestroy" lifecycle event
// this lifecycle event happens before an instance is removed from the database,
// so we can use this to "clean up" other rows that are also no longer needed
TodoList.beforeDestroy((todoListInstance) => {
  // make sure to return any promises inside hooks! This way Sequelize will be sure to
  // wait for the promise to resolve before advancing to the next lifecycle stage!
    return Todo.destroy({
      where: {
        listId: todoListInstance.id
      }
    })
})
```


## Available Hooks (in Order of Operation)

```
(1)
  beforeBulkCreate(instances, options)
  beforeBulkDestroy(options)
  beforeBulkUpdate(options)
(2)
  beforeValidate(instance, options)
(-)
  validate
(3)
  afterValidate(instance, options)
  - or -
  validationFailed(instance, options, error)
(4)
  beforeCreate(instance, options)
  beforeDestroy(instance, options)
  beforeUpdate(instance, options)
  beforeSave(instance, options)
  beforeUpsert(values, options)
(-)
  create
  destroy
  update
(5)
  afterCreate(instance, options)
  afterDestroy(instance, options)
  afterUpdate(instance, options)
  afterSave(instance, options)
  afterUpsert(created, options)
(6)
  afterBulkCreate(instances, options)
  afterBulkDestroy(options)
  afterBulkUpdate(options)
```