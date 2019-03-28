# mern-full-stack-mongoose

This is a modified version of the previous MERN starter code sample - https://github.com/IADT-AdvancedJS/mern-full-stack. It has been modified to use [Mongoose](https://mongoosejs.com/) to provide object modeling for MongoDB.

There are three key changes.

## 1. Mongoose installation

Mongoose has been installed using `npm install --save mongoose`. (Note: this will be installed along with all the project dependencies when you run `npm install`).

## 2. `User` model has been added

This defines the properties all Users have in the DB. This is in the `src/server/models/User.js` file.

```javascript
const mongoose = require('mongoose');

const UserSchema = mongoose.Schema({
  name: String,
  picture: String
});

module.exports = mongoose.model('User', UserSchema);
```

## 3. Server code modified

The file `src/server/index.js` has been modified to use Mongoose and the User model to manipulate the database. Key changes are highlighted below. All edits can be viewed in the [version history](https://github.com/IADT-AdvancedJS/mern-full-stack-mongoose/commit/ae50760a69136a8fc20f8e88f195045a732602f8#diff-6e0d62f54b853b53af874f5965d7adf2).

### Connect
```javascript
// import Mongoose and the User model
const mongoose = require('mongoose');
const User = require('./models/User');
```

```javascript
// connect to the DB
mongoose.connect(mongo_uri, { useNewUrlParser: true }, function(err) {
  if (err) {
    throw err;
  } else {
    console.log(`Successfully connected to ${mongo_uri}`);
  }
});
```

### Retrieve
```javascript
// retrieve all Users from the DB
User.find({}, (err, result) => {
  if (err) throw err;

  console.log(result);
  res.send(result);
});
```

```javascript
// retrieve a User by _id
User.findOne({_id: new ObjectID(req.params.id) }, (err, result) => {
  if (err) throw err;

  console.log(result);
  res.send(result);
});
```

### Delete
```javascript
// delete a User by _id
User.deleteOne( {_id: new ObjectID(req.body.id) }, err => {
  if (err) return res.send(err);

  console.log('deleted from database');
  return res.send({ success: true });
});
```

### Create
```javascript
// create a new user object using the Mongoose model and the data sent in the POST
const user = new User(req.body);
// save this object to the DB
user.save((err, result) => {
  if (err) throw err;

  console.log('created in database');
  res.redirect('/');
});
```
### Update
```javascript
// find a user matching this ID and update their details
User.updateOne( {_id: new ObjectID(id) }, {$set: req.body}, (err, result) => {
  if (err) throw err;

  console.log('updated in database');
  return res.send({ success: true });
});
```
