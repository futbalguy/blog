title: ExpressJS Session Tutorial
id: 917
categories:
  - Software Engineering
  - Javascript
tags:
  - Hack Reactor
  - Web Development
date: 2015-06-09 17:13:32
thumbnail: /express.png
banner: /express.png
---

On my latest Hack Reactor sprint, I had to design a server that could authenticate a user and identify them later through cookies. The following is a tutorial on how to use ExpressJS and sessions to save user state.

<!-- more -->

#### Back-End Setup

*   [NodeJS](#)
*   [ExpressJS web framework](#)
*   Express - Middleware

### Creating the Express App

Creating an Express app is simple and quick! Simply require Express and our middleware (after installing through NPM).

#### terminal

[code]
npm init
npm install express --save
npm install cookie-parser --save
npm install express-session --save

[/code]

#### app.js

[js]

var express = require('express');
var cookieParser = require('cookie-parser');
var session = require('express-session');

var app = express();

[/js]

### Express Sessions

When a client connects to the server, the Express-Session middleware automatically adds a session object to the request. You can add any informaton you want to remember to the session object and save it on the server-side. A cookie with an identifier token is saved on the client side.

Here we apply the middleware to the app:

#### app.js

[js]

//cookie parser is required for sessions to appear
app.use(cookieParser());

//session instantiation has some required arguments,
//the Session documentation is pretty clear
app.use(session({
  secret: 'boo',
  resave: false,
  saveUninitialized: false
}));

[/js]

When the client connects in the future, the server can restore the session that corrsponds to the cookie identifier.

To check authentication and enforce access restrictions, we created a custom middleware function that determines if a users cookie identifier matches a stored session on the server.

(In this example, we are using the session memory store included with Express-Session to save and load prior sessions. This memory store is deleted everytime the server stops so you will need to save to a more stable storage (i.e. database) for production code.)

#### app.js

[js]

//check for session.user, if not found then redirect user to login
function restrict(req, res, next) {
  if (req.session.user) {
    next();
  } else {
    req.session.error = 'Access denied!';
    res.redirect('/login');
  }
}

[/js]

### Putting It All Together

Here is the final code for our tutorial server. The server will remember a user who previously logged in and save them the hastle of logging in again!

#### app.js

[js]

var express = require('express');
var cookieParser = require('cookie-parser');
var session = require('express-session');

var app = express();

//cookie parser is required for sessions to appear
app.use(cookieParser());

//session instantiation has some required arguments,
//the Session documentation is pretty clear
app.use(session({
  secret: 'boo',
  resave: false,
  saveUninitialized: false
}));

function restrict(req, res, next) {
  //allow users with an active session as well as login page access
  if (req.session.user || req.url === '/login') {
    next();
  } else {
    req.session.error = 'Access denied!';
    res.redirect('/login');
  }
}

app.use(restrict);

app.post('/login', function(req, res) {
	if (/*login is valid*/) {
		req.session.user = {};
		req.session.save();
		res.redirect('/');
    } else {
		res.send(/*loginPageData*/)
	}
});

app.get('/login', function(req, res) {
 	res.send(/*loginPageData*/)
});

//client will only be routed to paths below if they were authenticated
app.get('/', function(req, res) {
 	res.send('You are a real user if you can see this!'')
});

console.log('Listening on 8080');
app.listen(808);

[/js]