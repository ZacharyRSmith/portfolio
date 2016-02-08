# Code samples

## Table of Contents

1. [PR refactoring NodeJS backend](#pr-refactoring-nodejs-backend)
1. [PR simplifying AngularJS](#pr-simplifying-angularjs)

## PR refactoring NodeJS backend

> Refactor NodeJS backend to use MVC architecture and simplify code.

One of my proudest refactorings is [this PR](https://github.com/HR10Knights/Sage/pull/96), especially:

Before: See anonymous function beginning at [doozy/routes/index.js line 15](https://github.com/ZacharyRSmith/Sage/commit/029f191beef73b715dc71c5bfc0b556502b24ab9#diff-d5ab1e2c4c009ef73d0dc63b1c182343L15).

```javascript
router.post('/signup', function(req, res, next) {
  var username = req.body.username;
  var password = req.body.password;
  var teamname = req.body.teamname;
  if (username === '' || teamname === '' || password === '') {
    res.status(400).send('Invalid sign up');
  } else {
    Team.findOne({name: teamname}, function(err, team) {
      if (team) {
        User.findOne({username: username}, function(err, user) {
          if (!user) {
            var newUser = new User({
              username: username,
              password: password
            });
            newUser.save(function(err, newUser) {
              if (err) {
                res.send(404, err);
              } else {
                team.users.push(newUser);
                team.save(function(err) {
                  if (err) {
                    res.send(404, err);
                  } else {
                    var token = jwt.encode(newUser, 'secret');
                    res.status(201);
                    res.json({token: token});
                  }
                });
              }
            });
          } else {
            res.status(400).send('Username exists');
          }
        });
      } else {
        team = new Team({
          name: teamname
        });
        team.save(function(err, newTeam) {
          User.findOne({username: username}, function(err, user) {
            if (!user) {
              var newUser = new User({
                username: username,
                password: password
              });
              newUser.save(function(err, newUser) {
                if (err) {
                  res.send(404, err);
                } else {
                  newTeam.users.push(newUser);
                  newTeam.save(function(err) {
                    if (err) {
                      res.send(404, err);
                    } else {
                      var token = jwt.encode(newUser, 'secret');
                      res.status(201);
                      res.json({token: token});
                    }
                  });
                }
              });
            } else {
              res.status(400).send('Username exists');
            }
          });
        });
      }
    });
  } 
});
```

After: See indexController#createUser beginning at [doozy/controllers/indexController.js line 13](https://github.com/ZacharyRSmith/Sage/commit/029f191beef73b715dc71c5bfc0b556502b24ab9#diff-ce50fdc9bc8511d3b625854a16a1bf43R13).

```javascript
  createUser: function(req, res, next) {
    var username = req.body.username.trim();
    var password = req.body.password.trim();
    var teamname = req.body.teamname.trim();
    if (username === '' || teamname === '' || password === '') {
      res.status(400).send('Username, Password, and Teamname must be present');
      return;
    }

    User.findOne({username: username}, function(err, user) {
      if (user) { res.status(400).send('Username exists'); return; }
      var newUser = new User({
        username: username,
        password: password
      });

      newUser.save(function(err, newUser) {
        if (err) { res.status(404).send(err); return; }

        Team.findOne({name: teamname}, function(err, team) {
          if (err) { res.status(404).send(err); return; }
          team = team || new Team({name: teamname});

          team.users.push(newUser);
          team.save(function (err) {
            if (err) { res.status(404).send(err); return; }

            res.status(201);
            sendJWT(newUser, res);
          });
        });
      });
    });
  },
  ```

Relevant tests are in [test/integration/apiauth.js](https://github.com/ZacharyRSmith/Sage/commit/029f191beef73b715dc71c5bfc0b556502b24ab9#diff-7e1a68f2de537b9eb615ac01191e9dfaL1).

While I'm at it, I wrote [this](https://gist.github.com/ZacharyRSmith/343962433a89ef1b454a) to share with my team.  It explains a code pattern I used to help me in the code clean up I linked above.

## PR simplifying AngularJS

> PR that, among other things, abstracts a modal into an AngularJS factory, `MoreInfo`, that is used to extend different AngularJS controllers' scopes.

[This PR](https://github.com/Ryans-to-the-Max/knowhere/pull/180):

[The `MoreInfo` AngularJS factory](https://github.com/Ryans-to-the-Max/knowhere/pull/180/files#diff-9475418f66f2aec7f9941a584e0423eeL382), and [an example](https://github.com/Ryans-to-the-Max/knowhere/pull/180/files#diff-785e2de112ccec3759854133cc0e4225L1) of it being used to extend an AngularJS controller's scope.

