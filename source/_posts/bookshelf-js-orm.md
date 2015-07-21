title: Bookshelf.js Javascript ORM and SQL
id: 951
categories:
  - Software Engineering
  - Javascript
tags:
  - Hack Reactor
  - Web Development
date: 2015-06-19 21:18:08
thumbnail: /bookshelf.png
banner: /bookshelf.png
---

On my current Hack Reactor project, we designed a site that needed a SQL database.

Instead of writing SQL queries, we chose to interface with MySQL through a developer-friendly framework named Bookshelf.js. Bookshelf is an object-relational mapping (ORM) framework that allows you to use object-oriented language to access a relational database.

<!-- more -->

#### Back-End Setup

*   [NodeJS](https://nodejs.org/)
*   [ExpressJS](http://expressjs.com/)
*   [BookshelfJS](http://bookshelfjs.org/)
*   [MySQL](https://www.mysql.com/)

## Building Your Bookshelf

Installing bookshelf is simple, just use [NPM](https://www.npmjs.com/) as a dependency manager!

##### terminal

```shell
npm init
npm install bookshelf --save
npm install knex --save
```

Next, you need to require Bookshelf in your app. We chose to do it in our main server file App.js

##### app.js

```javascript
var express = require('bookshelf');
var app = express();
```

You also need to configure the SQL database table schemas for Bookshelf to interface with. Bookshelf interacts with SQL through KnexJS so see the [Knex documentation](http://knexjs.org/) for more specifics of schema setup.

##### db-config.js

```javascript
var knex = require('knex')({
  client: 'mysql',
  connection: {
    host: '127.0.0.1',
    user: 'root',
    database: 'database'
  }
});

var db = require('bookshelf')(knex);

db.knex.schema.hasTable('users').then(function(exists) {
  if (!exists) {
    db.knex.schema.createTable('users', function(table) {
      table.increments('id').primary();
      table.string('username', 100);
      table.string('password', 100);
      table.string('address', 255);
    }).then(function(table) {
      console.log('created table :', 'users');
    })
  }
});

db.knex.schema.hasTable('events').then(function(exists) {
  if (!exists) {
    db.knex.schema.createTable('events', function(table) {
      table.increments('id').primary();
      table.string('user_id', 100);
      table.string('name', 255);
      table.string('description', 255);
      table.string('venue', 255);
      table.dateTime('date');
      table.string('address', 255);
    }).then(function(table) {
      console.log('created table :', 'events');
    })
  }
});

//this is for the followers many-to-many join table
db.knex.schema.hasTable('followers').then(function(exists) {
  if (!exists) {
    db.knex.schema.createTable('followers', function(table) {
      table.increments('id').primary();
      table.integer('follower_id');
      table.integer('followed_id');
    }).then(function(table) {
      console.log('created table :', 'followers');
    })
  }
});
```

## Arranging Your Bookshelf

We are ready to define our Bookshelf models that we will use to save and retrieve Users and Events.

##### User.js

```javascript
var db = require('../db-config');

var User = db.Model.extend({
  tableName: 'users',
  hasTimestamps: false,

  //define the relationship that each user can have many events
  events: function() {
    //invoking require at runtime so we avoid circular dependency on Event
    var Event = require('./event');
    return this.hasMany(Event);
  },

  //define the relationship that this user follows other users through the followers table
  follows: function() {
    return this.belongsToMany(User,'followers', 'follower_id', 'followed_id');
  },

  //define the relationship that other users follows this user through the followers table
  followers: function() {
    return this.belongsToMany(User,'followers', 'followed_id', 'follower_id');
  },

});

module.exports = User;
```

##### Users.js

```javascript
var db = require('../db-config');
var User = require('../models/user');

var Users = new db.Collection();
Users.model = User;

module.exports = Users;
```

##### Follower.js

```javascript
var db = require('../db-config');

//this is for our join table of users following users
var Follower = db.Model.extend({
  tableName: 'followers',
  hasTimestamps: false
});

module.exports = Follower;
```

##### Event.js

```javascript
var db = require('../db-config');

var Event = db.Model.extend({
  tableName: 'events',
  hasTimestamps: false,

  //define the relationship that each event belongs to one user
  user: function() {
    //invoking require at runtime so we avoid circular dependency on User
    var User = require('./user');
    return this.belongsTo(User, 'user_id');
  }
});

module.exports = Event;
```

##### Events.js

```javascript
var db = require('../db-config');
var Event = require('../models/event');

var Events = new db.Collection();
Events.model = Event;

module.exports = Events;
```

## Using Your Bookshelf

Now it's time to use your bookshelf models to store and retrieve data from your database.

See the code below for examples of storing and retrieving User and Event data.

##### UserUtils.js

```javascript
var db = require('../db-config');
var Users = require('../collections/users');
var User = require('../models/user');
var Follower = require('../models/follower');

module.exports = {

  //get all users from DB
  retrieveAllUsers: function(callback) {
    Users.reset().fetch().then(function(users) {
        callback(null, users.models)
    })
  },

  //get a user from DB by ID
  retrieveUser: function(user_id, callback) {
    user_id = parseInt(user_id);

    new User({
        id: user_id
      }).fetch({
		//here we list the related data we want to include,
		//the strings in the array match the method names in the User model
        withRelated: ['events', 'follows', 'followers'],
        require: true
      }).then(function(found) {
        if (found) {
          var userWithJoins = found.attributes;
		  //add the related data to the oject that will be returned in the callback
          userWithJoins.events = found.relations.events;
          userWithJoins.follows = found.relations.follows;
          userWithJoins.followers = found.relations.followers;

          callback(null, userWithJoins);
        } else {
          console.log('user_id not found:' + user_id);
        }
      })
  },

  //store a new user in DB
  storeUser: function(user, callback) {
    new User(user).fetch().then(function(found) {
        if (found) {
          callback(null, found.attributes);
          console.log('user already found:', username);
        } else {
          var user = new User(user);
          user.save().then(function(newUser) {
              Users.add(newUser);
              callback(null, newUser);
            })
        }
      })
  },

  //add a following relationship to the followers join table
  storeFollowing: function(follower_id, followed_id, callback) {
    follower_id = parseInt(follower_id);
    followed_id = parseInt(followed_id);

    new Follower({
	  follower_id:follower_id,
	  followed_id:followed_id
	})
	.fetch().then(function(found) {
      if (found) {
        callback(null, found.attributes);
        console.log('user_id: ',follower_id, ' is already following user_id: ',followed_id);
      } else {

        new Follower({follower_id:follower_id,followed_id:followed_id})
        .save().then(function(relationship) {
            callback(null, relationship.attributes);
          })
      }
    });
  },

  //remove a following relationship from the followers join table
  removeFollowing: function(follower_id, unFollowing_id, callback) {
    follower_id = parseInt(follower_id);
    followed_id = parseInt(unFollowing_id);

    new Follower({
	  follower_id:follower_id,
	  followed_id:followed_id
	}).fetch().then(function(found){
      if (found) {
        found.destroy();
        callback(null, null);
      }
    })
  },
};
```

##### EventUtils.js

```javascript
var db = require('../db-config');
var Events = require('../collections/events');
var Event = require('../models/event');

module.exports = {

  //get all events from DB
  retrieveAllEvents: function(callback) {
    Events.reset().fetch().then(function(events) {
      callback(null,events);
    })
  },

  //get one events from DB by ID
  retrieveEvent: function(eventID,callback) {
    new Event({
	  id: eventID
	}).fetch({
	//here we list the related data we want to include,
	//the string in the array matches the method names in the User model
	  withRelated:['user'],
	  require: true
	}).then(function(found) {
      if (found) {
        callback(null,found.attributes);
      } else {
        console.log('event not found:' + eventName);
      }
    })
  },

  //set new event to DB
  storeEvent: function(event,callback) {

    new Event({name:event.name}).fetch().then(function(found) {
      if (found) {
        console.log('event already found: ',found.attributes);
        callback(null,found.attributes);
      } else {

        new Event(event).save().then(function(newEvent) {
          console.log('saved event: ',newEvent);
          Events.add(newEvent);
          callback(null,newEvent);
        })
      }
    })
  }
};
```