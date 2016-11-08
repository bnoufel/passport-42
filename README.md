# passport-42

[![Build](https://img.shields.io/travis/pandark/passport-42.svg)](https://travis-ci.org/pandark/passport-42)
[![Coverage](https://img.shields.io/coveralls/pandark/passport-42.svg)](https://coveralls.io/r/pandark/passport-42)
[![Quality](https://img.shields.io/codeclimate/github/pandark/passport-42.svg?label=quality)](https://codeclimate.com/github/pandark/passport-42)
[![Dependencies](https://img.shields.io/david/pandark/passport-42.svg)](https://david-dm.org/pandark/passport-42)

[Passport](http://passportjs.org/) strategy for authenticating with
[42](https://api.intra.42.fr/apidoc) using the OAuth 2.0 API.

This module lets you authenticate using 42 in your Node.js applications.
By plugging into Passport, 42 authentication can be easily and unobtrusively
integrated into any application or framework that supports
[Connect](http://www.senchalabs.org/connect/)-style middleware, including
[Express](http://expressjs.com/).

## Install

    $ npm install passport-42

## Usage

#### Create an Application

Before using `passport-42`, you must register an application with
42.  If you have not already done so, a new application can be created at
[42 Applications](https://profile.intra.42.fr/oauth/applications).  Your
application will be issued an app UID and app SECRET, which need to be provided
to the strategy.  You will also need to configure a redirect URI which matches
the route in your application.

#### Configure Strategy

The 42 authentication strategy authenticates users using a 42 account and OAuth
2.0 tokens.  The app UID and SECRET obtained when creating an application are
supplied as options when creating the strategy.  The strategy also requires a
`verify` callback, which receives the access token and optional refresh token,
as well as `profile` which contains the authenticated user's 42 profile.  The
`verify` callback must call `cb` providing a user to complete authentication.

```js
passport.use(new FortyTwoStrategy({
    clientID: FORTYTWO_APP_ID,
    clientSecret: FORTYTWO_APP_SECRET,
    callbackURL: "http://localhost:3000/auth/42/callback"
  },
  function(accessToken, refreshToken, profile, cb) {
    User.findOrCreate({ fortytwoId: profile.id }, function (err, user) {
      return cb(err, user);
    });
  }
));
```

#### Authenticate Requests

Use `passport.authenticate()`, specifying the `'42'` strategy, to
authenticate requests.

For example, as route middleware in an [Express](http://expressjs.com/)
application:

```js
app.get('/auth/42',
  passport.authenticate('42'));

app.get('/auth/42/callback',
  passport.authenticate('42', { failureRedirect: '/login' }),
  function(req, res) {
    // Successful authentication, redirect home.
    res.redirect('/');
  });
```

## Examples

Developers using the popular [Express](http://expressjs.com/) web framework can
refer to an [example](https://github.com/pandark/passport-42-example)
as a starting point for their own web applications.

## FAQ

##### How do I obtain a user profile with specific fields?

The 42 profile contains a lot of information about a user.  By default,
not all the fields in a profile are returned.  The fields need by an application
can be indicated by setting the `profileFields` option.

```js
new FortyTwoStrategy({
  clientID: FORTYTWO_APP_ID,
  clientSecret: FORTYTWO_APP_SECRET,
  callbackURL: "http://localhost:3000/auth/42/callback",
  profileFields: ['id', 'username', 'displayname', 'email', 'url', 'phone', 'image_url']
}), ...)
```

Refer to the [User](https://api.intra.42.fr/apidoc/2.0/users.html)
section of the 42 API Reference for the complete set of available fields.

## Contributing

#### Tests

The test suite is located in the `test/` directory.  All new features are
expected to have corresponding test cases.  Ensure that the complete test suite
passes by executing:

```bash
$ make test
```

#### Coverage

The test suite covers 100% of the code base.  All new feature development is
expected to maintain that level.  Coverage reports can be viewed by executing:

```bash
$ make test-cov
$ make view-cov
```

## License

[The MIT License](http://opensource.org/licenses/MIT)

Copyright (c) 2016 Adrien "Pandark" Pachkoff
<[https://lifeleaks.com/](https://lifeleaks.com/)>
