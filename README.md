passport-google-id-token
========================

Google ID token authentication strategy for [Passport](http://passportjs.org/) and [Node.js](http://nodejs.org/).

This module lets you authenticate using Google ID tokens in your Node.js applications.
This is useful for scenarios where we don't want to perform API calls to Google on behalf of the client, but we only want to authenticate it to our server. In short, we only validate the identity of the user by token verification, so there is no server-side OAuth operation.

Official Google documentation:

- [Authenticate with a backend server](https://developers.google.com/identity/sign-in/android/backend-auth)

## Install

    $ npm install git+https://github.com/riyaz-ali/passport-google-id-token.git

## Usage

#### Configure Strategy

The strategy requires a `verify` callback which accepts the `idToken` coming from the user to be authenticated, and then calls the `done` callback supplying a `parsedToken` (with all its information in visible form) and the `googleId`.

```js
var GoogleTokenStrategy = require('passport-google-id-token');

passport.use(new GoogleTokenStrategy({
    clientID: GOOGLE_CLIENT_ID
  },
  function(rawToken, refreshToken /* is null always */, parsedToken, done) {
    User.findOrCreate({ googleId: parsedToken.email }, function (err, user) {
      return done(err, user);
    });
  }
));
```

#### Authenticate Requests

Use `passport.authenticate()`, specifying the `'google-id-token'` strategy, to authenticate requests.

```js
app.post('/auth/google',
  passport.authenticate('google-id-token'),
  function (req, res) {
    // do something with req.user
    res.send(req.user? 200 : 401);
  }
);
```

The post request to this route should include a JSON object with the key `id_token` set to the one the client received from Google (e.g. after successful Google+ sign-in).

## Credits

  - [Juanma Reyes](http://github.com/jmreyes)
  - [Mike Nicholson](http://github.com/themikenicholson)
  - [Marco Sanson](http://github.com/marcosanson)
  - [Michal Kubenka](https://github.com/mkubenka)
  - [Tom Hoag](https://github.com/tomhoag)
  - [Bence Ferdinandy](https://github.com/priestoferis)

## License

(The MIT License)

Copyright (c) 2014-2017 Juan Manuel Reyes

Permission is hereby granted, free of charge, to any person obtaining a copy of
this software and associated documentation files (the "Software"), to deal in
the Software without restriction, including without limitation the rights to
use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of
the Software, and to permit persons to whom the Software is furnished to do so,
subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS
FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR
COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER
IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN
CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
