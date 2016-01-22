# halfpenny // COINS API Javascript client

What is a [halfpenny](https://en.wikipedia.org/wiki/Halfpenny_(British_pre-decimal_coin)), anyway?  Beyond an old-timey coin, halfpenny is the official javascript API client for the COINS platform.

## alpha warning
halfpenny is still in active development!  it is not ready for public usage

## peer dependencies

### request
In order to keep the use of this client as flexible as possible, the client
needs to be initialized with a function that can be used to make XHR requests.
The client was designed to work with the [request](https://www.npmjs.com/package/request)
package, but it can be adapted to work with others (like browser-request, or xhr).

## configuration

### basic configuration:
```
const request = require('request');
const Promise = require('bluebird');
const apiClientOptions = {
    requestFn: Promise.promisify(request),
    baseUrl: 'http://localhost:3000'
};j
const client = require('../sdk/index.js')(apiClientOptions);
```
The above configuration parameters are *required*.
**Note that the 'requestFn' must be promisified**

### advanced configuration

This client can use multiple request engines to make requests to the API. This
allows the COINS team to use the client for its integration tests as well as
for its browser application. Most of the configuration is centered around mapping
the parameters that the client will feed to the request engine. If you are using
*request* as your request engine, then there is no configuration needed. See
`nodeapi/test/utils/init-api-client` for an example of how to configure the
client to use *hapi server.injectThen*.

## usage

Once you have a configured client, you can use the following methods to interact
with the COINS API:

### *{Promise}* = .auth.login(username, password)

Sends POST request to /auth/keys, and stores resulting credentials.

### *{Promise}* = .auth.logout(username, password)

Sends DELETE request to /auth/keys, and removes credentials.

### *{Promise}* = .getAuthCredentials()

Get the credentials currently stored.

### *{Promise}* = .setAuthCredentials(val)

Set the credentials stored: will overwrite if already set.

### *{Promise}* = .makeRequest(options, sign)

Takes a set of request options formatted for the *request* library, and
modifies the options according to the requestObjectMap before signing the
request (*if `sign !== false`*) and sending it.

# examples

See `nodeapi/test/integration/keys.js`
