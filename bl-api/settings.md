[\< bl-api](./summary.md)

# Settings
Bl-api has a load of settings to make the application work.

- [Environment variables](#environment-variables)
- [Application settings](#application-settings)

## Environment variables
The environment variables are either saved in your bash-shell or (preffered) stored on a file called `.env`. This is a file
that is not shared with github and should only be stored locally on your
computer.

_On Heroku the environment variables are stored in their settings page of the application._

[Read more about environment variables](https://en.wikipedia.org/wiki/Environment_variable)

### List of environment variables

* PORT
  * the port where the server should run
* SERVER_PATH
  * if the server should have a path after domain, default `/`
* BLAPI_HOST
  * where the server is hosted, for example `api.boklisten.no` or `localhost`
* BLAPI_PORT
  * the port where the server should run
* BL_API_URI
  * the whole uri of where the application is running
* MONGODB_URI
  * location of mongodb, ex: `mongodb://localhost:27017/test_db`
* GOOGLE_CLIENT_ID
  * client id from google (for google login)
* GOOGLE_SECRET
  * client secret from google (for google login)
* FACEBOOK_CLIENT
  * client id from facebook (for facebook login)
* FACEBOOK_SECRET
  * client secret from facebook (for faceboook login)
* FEIDE_CLIENT_ID
  * client id from feide (for feide login)
* FEIDE_SECRET
  * client secret from feide (for feide login)
* FEIDE_AUTHORIZATION_URL
  * authorization endpoint for feide
* FEIDE_TOKEN_URL
  * token endpoint for feide
* FEIDE_USER_INFO_URL
  * user info endpoint for feide
* DIBS_SECRET_KEY
  * client secret from dibs (for dibs payments)
* DIBS_CHECKOUT_KEY
  * client checkout key from dibs (for dibs payments)
* DIBS_URI
  * the api location for dibs
  * test: `test.api.dibspayment.eu/v1/`
  * prod: `api.dibspayment.eu/v1/`
* SENDGRID_API_KEY
  * the access token from sendgrid (for sending email)
* CLIENT_URI
  * the default client uri for bl-web `http://localhost:4200/`
* URI_WHITELIST
  * all domains that are allowed to access bl-api.
  * can be as many as you want, must be seperated by spaces
  * ex: `http://localhost:4200 http://localhost:8080 http://bladmin.test.boklisten.no`
* ACCESS_TOKEN_SECRET
  * the secret that the access token is signed with
* REFRESH_TOKEN_SECRET
  * the secret that refresh token is signed with
* NPM_TOKEN

# Application settings
The application also has a file called `src/application-config.ts`. This file
has settings that are not regarded as `sensitive`. It is here you set how long
a access token lasts and default delivery days. You can read more about the
settings in this file by visiting the file in the `bl-api` repo.





