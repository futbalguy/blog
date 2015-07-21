title: Storing Images In Firebase
id: 1120
categories:
  - Software Engineering
  - Javascript
tags:
  - Hack Reactor
  - Web Development
date: 2015-07-02 18:15:31
thumbnail: /firebase.jpg
banner: /firebase.jpg
---

I spent the last day at Hack Reactor adding an image capture feature to a website. For storing images, we decided between storing on our own server, Amazon AWS, and Firebase.

We picked Firebase to offload storage and data transfer work from our own server. We also just wanted to check out Firebase after hearing so many good things about its ease of use!

Getting started with Firebase is very fast and straight-forward! Go to [Firebase](https://www.firebase.com/) and add a new account. They will provide you with a database for storing your information.

<!-- more -->

#### Our Stack

*   [AngularJS](https://angularjs.org/)
*   [Firebase](https://www.firebase.com/)

### Firebase Basics

All Firebase database data is stored as JSON objects. When you add data to the JSON tree, it becomes a key in the existing JSON structure.

However, **Firebase databases have no native support for arrays**. If we try to store an array, it really gets stored as an "object" with integers as the key names.

You access the database with a URL and command it to do what you need. For example, if you want to save images from event #3, you could use the following URL path:
_https://yourAppName.firebaseio.com/events/3/images_

### Interacting With Firebase

There are several options for interacting with Firebase:

1.  Firebase website
2.  RESTful API
3.  Firebase and AngularFire modules for Angular

We chose Option 3 - Firebase and AngularFire modules - because it is the most Javascript-friendly.

**PRO TIP:** Image files for Firebase need to be in a string format (and must be under 10mb)! You'll want to get the base64-encoded contents so you can store it as a string. The general approach is to convert the file locally (using FileReader).

Now let's see how it works in practice...

### Javascript and Firebase

First step: install bower dependency manager and the bower components for Firebase and AngularFire

<div class="code">

#### terminal

[code]
bower init
bower install firebase --save
bower install angularfire --save

[/code]
</div>

Now instantiate a Firebase object with the URL path for your app and inject the firebase module into your app.

<div class="code">

#### app.js

[js]
var fb = new Firebase(&quot;https://yourAppName.firebaseio.com/&quot;);

angular.module('app', ['firebase'])

//...other code here...//
[/js]
</div>

Next, inject Firebase into the Angular module where your controller is located. The **$firebaseArray** variable below comes from the AngularFire module.

<div class="code">

#### controller.js

[js]
angular.module('app.controllers', ['firebase'])

.controller('AppCtrl', function($scope,$firebaseArray) {

	$scope.uploadImage = function(imageData, incidentID) {
    var FR = new FileReader();

      FR.onload = function(e) {
        var imageString = e.target.result;

		//create URL that refers to a specific event and add images as an array-like object to hold images
        var eventReference = fb.child(&quot;events/&quot; + incidentID);
        var syncArray = $firebaseArray(eventReference.child(&quot;images&quot;));

        $scope.user = Auth.getUser();
        var username = $scope.user.username || '';

        var submitDate = new Date().toISOString().slice(0, 10);

        syncArray.$add({
            imageString: imageString,
            username: username,
            submitDate: submitDate
		})
          .then(function() {
            console.log(&quot;Image has been uploaded&quot;);
          });
      };
      FR.readAsDataURL(imageData);
    });

[/js]
</div>