# line-oauth

## Overview

This package provides a Meteor wrapper for the LINE API methods, including the Login and the Get User Information APIs. Both APIs use OAuth login methodology.

## Login

This feature provides integration with the OAuth LINE Login method. Amongst its main dependencies, you can find ```accounts```, ```service-configuration``` and ```oauth``` Meteor packages.

### Getting Started

Before we can use the login feature, first we must configure the service with our keys, by adding them to this snippet inside ```accounts.js``` file.

```
ServiceConfiguration.configurations.update(
  { "service": "line" },
  {
    $set: {
      "clientId": <your_client_id>,
      "secret": <your_client_secret>,
    }
  },
  { upsert: true }
);
```
#### Important

Also, take into account that the default redirect uri of this package is ```<YOUR_DOMAIN>/_oauth/line``` so don't forget to add that url to the allowed Callback Urls inside Line channel config.

### Usage

The next step is to integrate the ```loginWithLine``` feature inside the LINE button (Don't forget to follow [these guidelines](https://developers.line.me/web-api/setting-up-login-button) to implement the LINE Login Button).

```
Meteor.loginWithLine(function (err, res) {
  console.log('login callback', err, res);
  if (err !== undefined) {
    console.log('sucess ' + res);
  } else {
    console.log('login failed ' + err);
  }
});
```

### Get User Information API

This package also provides some methods to get the LINE User profile information without logging that user into your app. 
To do so, we provide the ```getLoginUrl``` method, which will receive the ```redirectUri``` and the ```state``` as params, and it will return the loginUrl.

The last and most important method that this package provides to get the user information, is "getLineUserData", which will receive the authentication code as a param, and will return an object with the necessary data to append to the user in the users collection.
It's very important to take into account that this method should be called from the client through a Meteor.call, in order to avoid CORS issues, since the data is returned through REST APIs.
