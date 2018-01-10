# Config. with files

There is a lot of things you can configure in Otoroshi. By default, Otoroshi provides a configuration that should be enough for testing. But you should take a look here when you will be ready for production.

## Common configuration

| name | type | default value  | description |
| ---- |:----:| -------------- | ----- |
| `app.domain` | string | "foo.bar" | the domain on which Otoroshi UI/API will be exposes|
| `app.rootScheme` | string | "http" | the scheme on which Otoroshi will be exposed, either "http" or "https" |
| `app.snowflake.seed` | number | 0 | this number will be used to generate unique ids across the cluster. Each Otorshi instance must have a unique seed. |
| `app.events.maxSize` | number | 1000 | max number of analytic and allert events stored localy |
| `app.backoffice.subdomain` | string | "otoroshi" | the subdomain on wich Otoroshi backoffice will be served |
| `app.backoffice.session.exp` | number | 86400000 | the number of seconds before the Otoroshi backoffice session expires |
| `app.privateapps.subdomain` | string | "privateapps" | the subdomain on wich private apps UI will be served |
| `app.privateapps.session.exp` | number | 86400000 | the number of seconds before the private apps session expires |
| `app.claim.sharedKey` | string | "secret" | the shared secret used for signing the JWT token passed between Otoroshi and backend services |
| `app.webhooks.size` | number | 100 | number of events sent at most when calling one of the analytics webhooks |

## Admin API configuration

when Otoroshi starts for the first time, the DB is empty. As Otoroshi uses Otoroshi to expose its admin REST API, you have to provide the details for the admin API exposition. **This part is super important** because if you go to production with the default values, your Otoroshi server won't be secured anymore. 

@@@ warning
YOU HAVE TO CUSTOMIZE THE FOLLOWING VALUES BEFORE GOING TO PRODUCTION !!
@@@

Here, some terms in the description will be osbcure to you, but you will learm their meaning in the following chapters :)

| name | type | default value  | description |
| ---- |:----:| -------------- | ----- |
| `app.adminapi.targetSubdomain` | string | "otoroshi-admin-internal-api" | the subdomain on wich admin API call will be redirected from `app.adminapi.exposedDubdomain` |
| `app.adminapi.exposedDubdomain` | string | "otoroshi-api" | the subdomain on wich the Otoroshi admin API will be exposed |
| `app.adminapi.defaultValues.backOfficeGroupId` | string | "admin-api-group" | the name of the service groups that will contains the service descriptors for the Otoroshi admin API |
| `app.adminapi.defaultValues.backOfficeApiKeyClientId` | string | "admin-api-apikey-id" | the client id of the Otoroshi admin API apikey |
| `app.adminapi.defaultValues.backOfficeApiKeyClientSecret` | string | "admin-api-apikey-secret" | the client secret of the Otoroshi admin API apikey  |
| `app.adminapi.defaultValues.backOfficeServiceId` | string | "admin-api-service" | the id of the service descriptors for the Otoroshi admin API |
| `app.adminapi.proxy.https` | boolean | false | whether or not the current Otoroshi instance serves its content over https. This setting is useful for the backoffice UI to access Otoroshi admin API |
| `app.adminapi.proxy.local` | boolean | true | whether or not the admin API is accessible on `127.0.0.1`. This setting is useful for the backoffice UI to access Otoroshi admin API |

## DB configuration

as Otoroshi supports multiple DB, you have to provide some details about how to connect/configure it.

| name | type | default value  | description |
| ---- |:----:| -------------- | ----- |
| `app.storage` | string | "inmemory" | what kind of storage engine you want to use. Possible values are `inmemory`, `leveldb`, `redis`, `cassandra` |
| `app.importFrom` | string | | a file path or a URL to an Otoroshi export file. If the DB is empty at startup, this file will be used to import data to the empty DB |
| `app.importFromHeaders` | array | [] | a list of `:` separated header to use if the `app.importFrom` setting is a URL |
| `app.redis.host` | string | "localhost" | the host of the redis server |
| `app.redis.port` | number | 6379 | the port of the redis server |
| `app.redis.slaves` | array | [] | the redis slaves lists |
| `app.leveldb.path` | string | "./leveldb" | the path where levelDB files will be written |
| `app.cassandra.hosts` | string | "127.0.0.1" | the host of the cassandra server |
| `app.cassandra.host` | string | "127.0.0.1" | the list of cassandra hosts |
| `app.cassandra.port` | number | 9042 | the port of the cassandra servers |

## Headers configuration

Otoroshi uses a fair amount of http headers in order to work properly. The name of those headers are customizable to fit your needs.

| name | type | default value  | description |
| ---- |:----:| -------------- | ----- |
| `otoroshi.headers.trace.label` | string | "Otoroshi-Viz-From-Label" | header to pass request tracing informations |
| `otoroshi.headers.trace.from` | string | "Otoroshi-Viz-From" | header to pass request tracing informations (ip address) |
| `otoroshi.headers.trace.parent` | string | "Otoroshi-Parent-Request" | header to pass request tracing informations (parent request id) |
| `otoroshi.headers.request.adminprofile` | string | "Otoroshi-Admin-Profile" | header to pass admin name when the admin API is called from the Otoroshi backoffice |
| `otoroshi.headers.request.clientid` | string | "Otoroshi-Client-Id" | header to pass apikey client id |
| `otoroshi.headers.request.clientsecret` | string | "Otoroshi-Client-Secret" | header to pass apikey client secret |
| `otoroshi.headers.request.id` | string | "Otoroshi-Request-Id" | header containing the id of the current request |
| `otoroshi.headers.response.proxyhost` | string | "Otoroshi-Proxied-Host" | header containing the proxied host |
| `otoroshi.headers.response.error` | string | "Otoroshi-Error" | header containing whether or not the request generated an error |
| `otoroshi.headers.response.errormsg` | string | "Otoroshi-Error-Msg" | header containing error message if some |
| `otoroshi.headers.response.proxylatency` | string | "Otoroshi-Proxy-Latency" | header containing the current latency induced by Otoroshi |
| `otoroshi.headers.response.upstreamlatency` | string | "Otoroshi-Upstream-Latency" | header containing the current latency from Otoroshi to service backend |
| `otoroshi.headers.response.dailyquota` | string | "Otoroshi-Daily-Calls-Remaining" | header containing the number of remaining daily call (apikey) |
| `otoroshi.headers.response.monthlyquota` | string | "Otoroshi-Monthly-Calls-Remaining" | header containing the number of remaining monthly call (apikey) |
| `otoroshi.headers.comm.state` | string | "Otoroshi-State" | header containing a random value for secured mode |
| `otoroshi.headers.comm.stateresp` | string | "Otoroshi-State-Resp" | header containing a random value for secured mode |
| `otoroshi.headers.comm.claim` | string | "Otoroshi-Claim" | header containing a JWT token for secured mode |
| `otoroshi.headers.healthcheck.test` | string | "Otoroshi-Health-Check-Logic-Test" | header containing a logic test for healthcheck |
| `otoroshi.headers.healthcheck.testresult` | string | "Otoroshi-Health-Check-Logic-Test-Result" |  header containing the result of a logic test for healthcheck |
| `otoroshi.headers.jwt.issuer` | string | "Otoroshi" | the name of the issuer for the JWT token |
| `otoroshi.headers.canary.tracker` | string | "Otoroshi-Canary-Id" | header containing the ID of the canary session if enabled |

## Play specific configuration

As Otoroshi is a [Play app](https://www.playframework.com/), you should take a look at [Play configuration documentation](https://www.playframework.com/documentation/2.5.x/Configuration) to tune its internal configuration

| name | type | default value  | description |
| ---- |:----:| -------------- | ----- |
| `http.port` | number | 8080 | the http port used by Otoroshi. You can use 'disabled' as value if you don't want to use http |
| `https.port` | number | disabled | the https port used by Otoroshi. You can use 'disabled' as value if you don't want to use https |
| `play.crypto.secret` | string | "secret" | the secret used to sign Otoroshi session cookie |
| `play.http.session.secure` | boolean | false | whether or not the Otoroshi backoffice session will be served over https only |
| `play.http.session.httpOnly` | boolean | true | whether or not the Otoroshi backoffice session will be accessible from Javascript |
| `play.http.session.maxAge` | number | 259200000 | the number of seconds before Otoroshi backoffice session expired |
| `play.http.session.domain` | string | ".foo.bar" | the domain on which the Otoroshi backoffice session is authorized |
| `play.http.session.cookieName` |  string | "otoroshi-session" | the name of the Otoroshi backoffice session |
| `play.ws.play.ws.useragent` | string | "Otoroshi" | the user agent sent by Otoroshi if not present on the original http request |
| `play.server.https.keyStore.path` | string | | the path to the keystore containing the private key and certificate, if not provided generates a keystore for you |
| `play.server.https.keyStore.type` | string | | the key store type, defaults to JKS | 
| `play.server.https.keyStore.password` | string | | the password, defaults to a blank password | 
| `play.server.https.keyStore.algorithm` | string | | the key store algorithm, defaults to the platforms default algorithm |

## Example of configuration file

```conf
include "base.conf"

http.port = 8080

app {
  storage = "leveldb"
  importFrom = "./my-state.json"
  env = "prod"
  domain = "foo.bar"
  rootScheme = "http"
  snowflake {
    seed = 0
  }
  events {
    maxSize = 1000
  }
  backoffice {
    subdomain = "otoroshi"
    session {
      exp = 86400000
    }
  }
  privateapps {
    subdomain = "privateapps"
    session {
      exp = 86400000
    }
  }
  adminapi {
    targetSubdomain = "otoroshi-admin-internal-api"
    exposedDubdomain = "otoroshi-api"
    defaultValues {
      backOfficeGroupId = "admin-api-group"
      backOfficeApiKeyClientId = "admin-api-apikey-id"
      backOfficeApiKeyClientSecret = "admin-api-apikey-secret"
      backOfficeServiceId = "admin-api-service"
    }
  }
  claim {
    sharedKey = "mysecret"
  }
  leveldb {
    path = "./leveldb"
  }
}

play.http {
  session {
    secure = false
    httpOnly = true
    maxAge = 2592000000
    domain = ".foo.bar"
    cookieName = "oto-sess"
  }
}
```
