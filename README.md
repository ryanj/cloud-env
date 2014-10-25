#cloud-env [![npm version](http://img.shields.io/npm/v/cloud-env.svg)](https://www.npmjs.org/package/cloud-env) [![Dependency Check](http://img.shields.io/david/ryanj/cloud-env.svg)](https://david-dm.org/ryanj/cloud-env) [![monthly downloads](http://img.shields.io/npm/dm/cloud-env.svg)](https://www.npmjs.org/package/cloud-env) [![license](http://img.shields.io/npm/l/cloud-env.svg)](https://www.npmjs.org/package/cloud-env)

[cloud-env](https://github.com/ryanj/cloud-env) provides a vendor-neutral interface for autoconfiguring your server, allowing it to run on a variety of cloud hosting platforms.

It works by checking the system environment (`process.env`) for known configuration strings (published by [OpenShift](http://openshift.com/), [Heroku](http://heroku.com/)), normalizing the results into [a well-defined list](#configuration-stings).

## Installation

The resulting config object contains the configuration settings that `cloud-env` was able to detect - including the server `PORT` number and bind `IP` address:

``` js
  //npm install cloud-env
  var config = require('cloud-env')
```

See the [Configuration Strings](#configuration-strings) list for more information about the settings that this module will automatically resolve.

## Listen up
Make sure to pass `config.PORT` and `config.IP` to your server's `listen` function:

```js
app.listen(config.PORT, config.IP, function () {
  console.log("Listening on "+config.IP+", port "+config.PORT)
});
```

If host-provided configs are not found, local development defaults are returned - allowing you to configure once, and run anywhere.

### Provide your own defaults
Use `.get('KEYNAME')` to fetch keys by name, optionally providing your own default values in the process:

```js
port = config.get('PORT', 8000)
bind_address = config.get('IP','127.0.0.1')
app.listen(port, bind_address, function () {
  console.log("Listening on " + bind_address + ", port " + port)
});
```

The above example will default to port 8000 instead of 8080, and will attempt to bind on '127.0.0.0.1' instead of '0.0.0.0'.

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

### Advanced Configuration

See [`config-multipaas`](https://github.com/ryanj/config-multipaas/) and the related [`config-chain` API docs](https://github.com/dominictarr/config-chain/#boring-api-docs) for a more advanced configuration solution that incorporates the same set of cloud configuration keys.

![MultiPaaS](http://i.imgur.com/fCi6YX6.png)
