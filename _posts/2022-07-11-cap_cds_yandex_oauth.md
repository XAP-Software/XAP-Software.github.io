---
layout: post
title: How to create custom Yandex OAuth2 authorization in SAP CAP CDS Application?
categories: [CAP CDS, OAuth2]
author_github: negativename
author: Vladislav Kobenko
---

If you want to create custom `Yandex OAuth2` authorization in `SAP CAP CDS Application`, this article will help you a lot.

## Creating an app

First of all you need to create an app on this page: `https://oauth.yandex.ru/client/new`. After that you need to get from there next parameters:

1. Client ID
2. Client Secret Key
3. Callback URL

## Configurating

You need to install two modules before we start creating custom authentication. 

```bash
    npm install passport cookie-session
```

Also you need to install passport-strategy package. In this article we used `Yandex OAuth2`, that's why you need to run next command:

```bash
    npm install passport-yandex
```

And in the end you need to modify `package.json` to let the `cds` module to know, that you are using custom authentication:

```json
    "cds": {
        "requires": {
            "auth": {
                "impl": "./auth/handler.js"
            }
        }
    }
```

## Passport strategy

Create a directory `auth` and put there two files: `passport.js` and `handler.js`.

In the `passport.js` file you need to paste following code:

```js
const passport = require('passport');
const YandexStrategy = require('passport-yandex').Strategy;

const { YANDEX_CALLBACK_URL, YANDEX_CLIENT_ID, YANDEX_CLIENT_SECRET } = process.env;

passport.serializeUser(function (user, done) {
  done(null, user);
});

passport.deserializeUser(function (user, done) {
  done(null, user);
});

passport.use(new YandexStrategy({
    clientID: YANDEX_CLIENT_ID,
    clientSecret: YANDEX_CLIENT_SECRET,
    callbackURL: YANDEX_CALLBACK_URL
  },
  function(accessToken, refreshToken, profile, done) {
    process.nextTick(function () {
        // here you can add an validation for existing of user in your database or something else.
        return done(null, profile);
    });
  }
));
```

Here we created serializer and deserializer methods for user object and added passport strategy. 

In `handler.js` file we are watching if user exists in cookie-session and creating `cds.User` for him. If logging in failed we return error `Unauthorized` with status code `401`.

```js
const cds = require("@sap/cds");

module.exports = (req, res, next) => {
  if (req?.user) {
    req.user = new cds.User(req.user);
    next();
  } else {
    res.status(401).send();
  }
} 
```

### Creating authorization logic

Finally you need to create `server.js` file in `srv/` directory and add the following code there:

```js
const cds = require("@sap/cds");
const implementation = require('./serverImplementation');

cds.on("bootstrap", async (app) => await implementation(app));

module.exports = cds.server;
```

Also you should create `serverImplementation.js` file in the same directory. In this file we will add logic for authentication process.

```js
const passport = require('passport');
const cookieSession = require('cookie-session');
require('../auth/passport');

module.exports = async (app) => {
  const { YANDEX_CLIENT_ID, YANDEX_CLIENT_SECRET } = process.env;

  app.use(cookieSession({
    name: 'yandex-auth-session',
    keys: ['key1', 'key2']
  }));

  app.use(passport.initialize());
  app.use(passport.session());

  app.get('/error', function(req, res){
    // you can implement error here
    res.send('Current user does not exist'); 
  });

  app.get('/auth/my-user', (req, res) => {
    // showing user details in json format
    res.json(req?.user?._json)
  })

  app.get('/auth/yandex',
    passport.authenticate('yandex'),
    function(req, res){
      // The request will be redirected to Yandex for authentication, so this
      // function will not be called.
  });

  app.get('/auth/yandex/redirect', 
  passport.authenticate('yandex', { failureRedirect: '/error' }),
  function(req, res) {
    res.redirect('/');
  });

  app.get('/logout', function(req, res){
    req.session = null;
    req.logout();
    res.redirect('/');
  });
}
```

First of all you need to create and initialize `cookie-session` for keeping data of authenticated user. Now let's see the routes in the code above:

1. `/auth/yandex` route is using for redirect user to the yandex authorization page.
2. `/auth/yandex/redirect` - this route should be the same as `Callback URL` in your yandex application. It uses for sending the code, which user will get after successful authorization on yandex page, and getting the access and refresh tokens. After that this method send access token to the `Yandex API` and getting back user's data and redirect user to the root page `/`.
3. `/error` this route is using for catching the error.
4. `/logout` this route is using for clearing the session and logout the user.
5. `/auth/my-user` this route shows user information in json format.

That would be nice if you could make your code a little bit more expressive.
