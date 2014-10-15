#cloud-env

Consistent naming for cloud-provided server configuration strings.

[cloud-env](https://github.com/ryanj/cloud-env) detects common server configuration strings provided by cloud hosting environments ([OpenShift](http://openshift.com/), [Heroku](http://heroku.com/)).

It works by checking the system environment (`process.env`) for common cloud configuration strings, returning sensible defaults when no config can be found.

``` js
  //npm install cloud-env
  var config = require('cloud-env')
```

Your server's `PORT` number and bind `IP` configuration should now be automatically resolved.  If the server-provided config strings are missing, local development defaults are returned instead.

## Listen up

Make sure to pass `config.PORT` and `config.IP` to your app's `listen` function:

```js
app.listen(config.PORT, config.IP, function () {
  console.log("Listening on "+config.IP+", port "+config.PORT)
});
```

That's it! 

## Default Configs

Cloud configuration defaults:

config.get(VARNAME) | READ_FROM | DEFAULT_VALUE
--------------------|-----------|---------------
IP                  | BIND_IP, OPENSHIFT_NODEJS_IP | 0.0.0.0
PORT                | PORT, OPENSHIFT_NODEJS_PORT | 8080
HOSTNAME            | OPENSHIFT_APP_DNS  | localhost
APP_NAME            | OPENSHIFT_APP_NAME, APP_NAME | APP_NAME
MONGODB_DB_URL      | OPENSHIFT_MONGODB_DB_URL, MONGODB_DB_URL | mongodb://127.0.0.1:27017
POSTGRESQL_DB_URL   | OPENSHIFT_POSTGRESQL_DB_URL, POSTGRESQL_DB_URL | postgresql://127.0.0.1:5432
MYSQLDB_DB_URL      | OPENSHIFT_MYSQLDB_DB_URL, MYSQLDB_DB_URL | mysql://127.0.0.1:3306

See [`config-multipaas`](https://github.com/ryanj/config-multipaas/) and the related [`config-chain` API docs](https://github.com/dominictarr/config-chain/#boring-api-docs) for a more advanced configuration solution that can incorporate the same set of default cloud configuration keys.
