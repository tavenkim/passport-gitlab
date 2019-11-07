# passport-gitlab

[Passport](https://github.com/jaredhanson/passport) strategy for authenticating
with [Gitlab](http://www.gitlab.com/) using the OAuth 1.0a API.

This module lets you authenticate using Gitlab in your Node.js applications.
By plugging into Passport, Gitlab authentication can be easily and
unobtrusively integrated into any application or framework that supports
[Connect](http://www.senchalabs.org/connect/)-style middleware, including
[Express](http://expressjs.com/).

## Install

    $ npm install passport-gitlab

## Usage

#### Create an Application

Before using `passport-gitlab`, you must first get an Gitlab API key. If you
have not already done so, an API key can be requested at internal.
Your will be issued an API key and secret, which need to be provided to the
strategy.

#### Configure Strategy

The Gitlab authentication strategy authenticates users using an Gitlab
account and OAuth tokens.  The API key secret obtained from Gitlab are
supplied as options when creating the strategy.  The strategy also requires a
`verify` callback, which receives the access token and corresponding secret as
arguments, as well as `profile` which contains the authenticated user's Gitlab
profile.   The `verify` callback must call `cb` providing a user to complete
authentication.

    passport.use(new GitlabStrategy({
        consumerKey: Gitlab_CONSUMER_KEY,
        consumerSecret: Gitlab_CONSUMER_SECRET,
        callbackURL: "http://127.0.0.1:3000/auth/gitlab/callback"
      },
      function(token, tokenSecret, profile, cb) {
        User.findOrCreate({ gitlabId: profile.id }, function (err, user) {
          return cb(err, user);
        });
      }
    ));

#### Authenticate Requests

Use `passport.authenticate()`, specifying the `'gitlab'` strategy, to
authenticate requests.

For example, as route middleware in an [Express](http://expressjs.com/)
application:

    app.get('/auth/gitlab',
      passport.authenticate('gitlab'));
    
    app.get('/auth/gitlab/callback', 
      passport.authenticate('gitlab', { failureRedirect: '/login' }),
      function(req, res) {
        // Successful authentication, redirect home.
        res.redirect('/');
      });

