#cloud-env
[cloud-env](https://github.com/ryanj/cloud-env) provides consistent naming for server configuration strings that are published by various cloud hosting providers.

It works by checking the system environment (`process.env`) for known configuration strings ([OpenShift](http://openshift.com/), [Heroku](http://heroku.com/)), normalizing the results into [a well-defined list](#configuration-stings).

If host-provided configs are not found, local development defaults are returned - allowing you to configure once, and run anywhere.

See the [Configuration Strings](#configuration-strings) list for more information about the settings that this module will automatically resolve.

## Installation
The resulting config object should contain any configuration settings that `cloud-env` was able to detect - including the server `PORT` number and bind `IP` address:

``` js
  //npm install cloud-env
  var config = require('cloud-env')
```

## Listen up

Make sure to pass `config.PORT` and `config.IP` to your server's `listen` function:

```js
app.listen(config.PORT, config.IP, function () {
  console.log("Listening on "+config.IP+", port "+config.PORT)
});
```

## Configuration Strings
Reliable configuration settings for local dev AND for "the cloud":

config.NAME | process.env.SOURCE_VARS | DEFAULT
--------------------|-----------|---------------
IP                  | OPENSHIFT_NODEJS_IP, BIND_IP | 0.0.0.0
PORT                | OPENSHIFT_NODEJS_PORT, PORT | 8080
HOSTNAME            | OPENSHIFT_APP_DNS, HOSTNAME  | localhost
APP_NAME            | OPENSHIFT_APP_NAME, APP_NAME | APP_NAME
MONGODB_DB_URL      | OPENSHIFT_MONGODB_DB_URL, MONGODB_DB_URL | mongodb://127.0.0.1:27017
MONGODB_DB_HOST      | OPENSHIFT_MONGODB_DB_HOST, MONGODB_DB_HOST | 127.0.0.1
MONGODB_DB_PORT      | OPENSHIFT_MONGODB_DB_PORT, MONGODB_DB_PORT | 27017
MONGODB_DB_USERNAME      | OPENSHIFT_MONGODB_DB_USERNAME, MONGODB_DB_USERNAME | undefined
MONGODB_DB_PASSWORD      | OPENSHIFT_MONGODB_DB_PASSWORD, MONGODB_DB_PASSWORD | undefined
POSTGRESQL_DB_URL   | OPENSHIFT_POSTGRESQL_DB_URL, POSTGRESQL_DB_URL | postgresql://127.0.0.1:5432
POSTGRESQL_DB_HOST   | OPENSHIFT_POSTGRESQL_DB_HOST, POSTGRESQL_DB_HOST | 127.0.0.1
POSTGRESQL_DB_PORT   | OPENSHIFT_POSTGRESQL_DB_PORT, POSTGRESQL_DB_PORT | 5432
POSTGRESQL_DB_USERNAME   | OPENSHIFT_POSTGRESQL_DB_USERNAME, POSTGRESQL_DB_USERNAME | undefined
POSTGRESQL_DB_PASSWORD   | OPENSHIFT_POSTGRESQL_DB_PASSWORD, POSTGRESQL_DB_PASSWORD | undefined
MYSQLDB_DB_URL      | OPENSHIFT_MYSQLDB_DB_URL, MYSQLDB_DB_URL | mysql://127.0.0.1:3306
MYSQLDB_DB_HOST      | OPENSHIFT_MYSQLDB_DB_HOST, MYSQLDB_DB_HOST | 127.0.0.1
MYSQLDB_DB_PORT      | OPENSHIFT_MYSQLDB_DB_PORT, MYSQLDB_DB_PORT | 3306
MYSQLDB_DB_USERNAME      | OPENSHIFT_MYSQLDB_DB_USERNAME, MYSQLDB_DB_USERNAME | undefined
MYSQLDB_DB_PASSWORD      | OPENSHIFT_MYSQLDB_DB_PASSWORD, MYSQLDB_DB_PASSWORD | undefined

See [`config-multipaas`](https://github.com/ryanj/config-multipaas/) and the related [`config-chain` API docs](https://github.com/dominictarr/config-chain/#boring-api-docs) for a more advanced configuration solution that incorporates the same set of cloud configuration keys.
