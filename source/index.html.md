---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell

toc_footers:
  - <a href='https://github.com/lord/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# Introduction

Welcome to the MPG Tracker API!

# Standard Response Structure

See the example to the right.

> All responses follow the same root JSON structure:

```json
{
  "status": boolean,
  "data": Object, // This is where a success response body will be placed
  "errors": [{
    "code": number,
    "i18n": string,
    "message": string
  }]
}
```

`Root`

Variable | Type | Required | Description
--------- | ---- | -------- | -----------
status | boolean | Yes | If the request succeded
data | Object | No | The data provided by the specific call
errors | Array of `Error` | No | Array of error objects

`Error`

Variable | Type | Required | Description
--------- | ---- | -------- | -----------
code | number | Yes | A code specific to the failure
i18n | string | A key used to look up the response in another language
message | string | Yes | User legible failure message

# Authentication

THere are 3 ways to gain access to an account within MPG Tracker:

1. Provide an email/password combo.
2. Provide the JWT from Google Sign-In.
3. Create a new account.

## User Authentication

Performs login for a user.

```shell
curl -X POST \
  -H "Accept: application/json" \
  --data '{"email":"xyz","password":"xyz"}' \
  "https://mpg.3dx2.com/api/authenticate.php"
```

### HTTP Request

`POST https://mpg.3dx2.com/api/authenticate.php `

### Request body

> An example of the request body

```json
{
  "email": "test@email.com",
  "pssword": "SomePassword"
}
```

Parameter | Type | Required | Description
--------- | ---- | -------- | -----------
email | String | Yes | User's email address
password | String | Yes | User's Password

> This endpoint performs a login with the username and password

<aside class="notice">
You must replace <code>email</code> and <code>password</code> with the appropriate combo.
</aside>

### Response Body

> A successful response will look like this:

```json
{
  "status":true,
  "data": {
    "userId":100001,
    "userToken":"some-large-entropy-response",
    "defaultVehicleId":1,
    "fullName":"John Doe",
    "removeAds":1
  }
}
```

> A failed response will respond with a `401` and look like this:

```json
{
  "status":false,
  "errors": [{
    "message":"Invalid Email or Password",
    "i18n":"authentication.failed",
    "code":401
  }]
}
```

Variable | Type | Required | Description
-------- | ---- | -------- | -----------
userId | Integer | Yes | A unique identifier for use with Google Analytics
userToken | String | Yes | A very long unique session tokenize
defaultVehicleId | Integer | Yes | The default vehicle for this user
fullName | String | No | The user's full name if they provided it
removeAds | Boolean | Yes | Whether the user has removed ads in the app

Failure Codes

Error Code | Internationalized Code | Message
---------- | ---------------------- | -------
101 | authentication.failed | Invalid Email or Password.

## Google Sign-In

```shell
curl -X POST -H "Accept: application/json" \
  --data '{"email":"xyz","name":"John Doe","token":"some-entropy"}'\
  "https://mpg.3dx2.com/api/authenticateGoogle.php"
```

This endpoint retrieves user data.

<aside class="warning">This will create a new user if one does not exist and retieve the existing one if it does.</aside>

### HTTP Request

`GET https://mpg.3dx2.com/api/authenticateGoogle.php`

### Request Body

Parameter | Type | Required | Description
--------- | ---- | -------- | -----------
email | String | Yes | User's email address
name | String | Yes | The full name of the user
token | String | Yes | The Google JWT proving

### Response Body

> A successful response will look like this:

```json
{
  "status": true,
  "data": {
    "userId":100001,
    "userToken":"some-large-entropy-response",
    "defaultVehicleId":1,
    "fullName":"John Doe",
    "removeAds":1
  }
}
```

> A failed response will look like this:

```json
{
  "status": false,
  "errors": [{
    "code": 104,
    "i18n": "accountCreate.captchaFail",
    "message": "Please complete the captcha prior to submitting."
  }]
}
```

Variable | Type | Required | Description
-------- | ---- | -------- | -----------
userId | Integer | Yes | A unique identifier for use with Google Analytics
userToken | String | Yes | A very long unique session tokenize
defaultVehicleId | Integer | Yes | The default vehicle for this user
fullName | String | Yes | The user's full name
removeAds | Boolean | Yes | Whether the user has removed ads in the app

Failure Codes

Error Code | Internationalized Code | Message
---------- | ---------------------- | -------
101 | accountCreate.captchaFail | Please complete the captcha prior to submitting.
102 | accountCreate.pwmismatch | Both passwords must match.
103 | accountCreate.dupe | The email address provided already has an account, please try resetting the password.
104 | accountCreate.vehicle | Your account was created, but we were unable to create a default vehicle. Please do so manually.
105 | accountCreate.invalidGoogleToken | Invalid Google Token.

## Create User

Creates a new user in the system.

```shell
curl -X POST \
  -H "Accept: application/json" \
  --data $BODY_JSON \
  "https://mpg.3dx2.com/api/userRegister.php"
```

### HTTP Request

`POST https://mpg.3dx2.com/api/userRegister.php`

### Request Body

```json
{
  "email":"test@email.com",
  "password0":"password",
  "password1":"password",
  "name":"John Doe",
  "captcha":"<some-token",
  "source":"android"
}
```

Parameter | Type | Required | Description
--------- | ---- | -------- | -----------
email | String | Yes | User's email address
password0 | String | Yes | User's Password
password1 | String | Yes | User's Password Confirmed
name | String | Yes | User's Full Name
captcha | String | Yes | The token provided by the captcha response
source | Enum | Yes | ENUM: `website` or `android` (to indcate which private captcha key should be used)

### Response Body

> A successful response will look like this:

```json
{
  "status": true,
  "data": {
    "userId":100001,
    "userToken":"some-large-entropy-response",
    "defaultVehicleId":1,
    "fullName":"John Doe",
    "removeAds":1
  }
}
```

Variable | Type | Required | Description
-------- | ---- | -------- | -----------
userId | Integer | Yes | A unique identifier for use with Google Analytics
userToken | String | Yes | A very long unique session tokenize
defaultVehicleId | Integer | Yes | The default vehicle for this user
fullName | String | Yes | The user's full name
removeAds | Boolean | Yes | Whether the user has removed ads in the app

> A failed response will return something like this:

```json
{
  "status": false,
  "errors": [{
    "code": 104,
    "i18n": "accountCreate.captchaFail",
    "message": "Please complete the captcha prior to submitting."
  }]
}
```

Failure Codes

Error Code | Internationalized Code | Message
---------- | ---------------------- | -------
101 | accountCreate.captchaFail | Please complete the captcha prior to submitting.
102 | accountCreate.pwmismatch | Both passwords must match.
103 | accountCreate.dupe | The email address provided already has an account, please try resetting the password.
104 | accountCreate.vehicle | Your account was created, but we were unable to create a default vehicle. Please do so manually.

## Forgot Password

Sends a password reset email.

```shell
curl -X POST \
  -H "Accept: application/json" \
  --data $BODY_JSON \
  "https://mpg.3dx2.com/api/forgotPassword.php"
```

### HTTP Request

`POST https://mpg.3dx2.com/api/forgotPassword.php`

### Request Body

```json
{
  "email":"test@email.com",
  "captcha":"some-token",
  "source":"android"
}
```

Parameter | Type | Required | Description
--------- | ---- | -------- | -----------
email | String | Yes | User's email address
captcha | String | Yes | The token provided by the captcha response
source | Enum | Yes | ENUM: `website` or `android` (to indcate which private captcha key should be used)

### Response Body

> A successful response will look like this:

```json
{
  "status": true,
  "data": {
    "message": "Email sent.  The link for password reset will only be valid for 5 minutes."
  }
}
```

> A failed response will return something like this:

```json
{
  "status": false,
  "errors": [{
    "code": 102,
    "i18n": "resetPassword.emailFailed",
    "message": "Unable to send password reset email."
  }]
}
```

Variable | Type | Required | Description
-------- | ---- | -------- | -----------
data | String | Yes | A unique identifier for use with Google Analytics
message | String | No | The succuess message to display
error | String | No | The failure message to display

Failure Codes

Error Code | Internationalized Code | Message
---------- | ---------------------- | -------
101 | resetPassword.captchaFail | Please complete the captcha prior to submitting.
102 | resetPassowrd.emailFailed | Unable to send password reset email.

# Authorization

All API calls after login require the user token provided from the authentication API call to be included in the header, <code>X-Authorization</code>.

```shell
curl "api_endpoint_here" -H "X-Authorization: $TOKEN"
```

> Make sure to set `$TOKEN` with your API key.

<aside class="notice">
You must replace <code>the-provided-user-token</code> with your personal API key.
</aside>

# Fill Ups

## Insert Fill Up

<aside class="notice">Requires Authorization</aside>

```shell
curl -XPOST "https://mpg.3dx2.com/api/fillUps.php"
```

This inserts a new fill up.

### HTTP Request

`POST https://mpg.3dx2.com/api/fillUps.php`

### Request Body

```json
{  
  "vehicleId":"13201",
  "distanceDriven":"100.0",
  "fuelUsed":"10.000",
  "dateFilled":"2019-07-08 21:31:00 -0400",
  "pricePer":"1.249",
  "odometer":"32",
  "note":"Welcome"
}
```

Parameter | Type | Required | Description
--------- | ---- | -------- | -----------
vehicleId | Integer | Yes | The ID of the vehicle being filled up
milesDriven | Double | Yes | Miles driven since last fill up
gasUsed | Double | Yes | Amount of gas used to fill tank
dateFilled | String | Yes | Date of the fill up (e.g. `2019-06-23 14:08:00 -0400`)
pricePerGal | Double | No | Price per gallon of gas to the 10 of a cent
odometer | Double | No | Odometer reading at fill up
note | String | No | A custom user note

### Response Body

> The above command returns JSON structured like this:

```json
{
  "status": true
}
```


## Update Fill Up

<aside class="notice">Requires Authorization</aside>

```shell
curl -XPUT "https://mpg.3dx2.com/api/fillUps.php" -H "X-Authorization: $TOKEN"
```

This updates an existing fill up.

### HTTP Request

`PUT https://mpg.3dx2.com/api/fillUps.php`

### Request Body

```json
{  
  "fillUpId":63117,
  "vehicleId":"13201",
  "distanceDriven":"100.0",
  "fuelUsed":"10.000",
  "dateFilled":"2019-07-09 00:31:00 -0400",
  "pricePer":"1.249",
  "odometer":"32.0",
  "note":"Welcome"
}
```

Parameter | Type | Required | Description
--------- | ---- | -------- | -----------
fillUpId | Integer | Yes | The ID of the fill up being updated
vehicleId | Integer | Yes | The ID of the vehicle being filled up
miles | Dobule | Yes | Miles driven since last fill up
gallons | Double | Yes | Amount of gas used to fill tank
date | String | Yes | Date of the fill up (e.g. `2019-06-23 14:08:00 -0400`)
price | Double | No | Price per gallon of gas to the 10 of a cent
odometer | Double | No | Odometer reading at fill up
note | String | No | A custom user note

### Response Body

> The above command returns JSON structured like this:

```json
{
  "status": true
}
```

## Delete a Fill Up

<aside class="notice">Requires Authorization</aside>

```shell
curl -XDELETE "https://mpg.3dx2.com/api/fillUps.php" -H "X-Authorization: $TOKEN"
```

This removes an existing fill up.

### Request Body

`DELETE https://mpg.3dx2.com/removeFillUp.php`

```json
{  
 "fillUpId":63021
}
```

Parameter | Type | Required | Description
--------- | ---- | -------- | -----------
fillUpId | Integer | Yes | The ID of the fill up being updated

### Response Body

> The above command returns JSON structured like this:

```json
{
  "status": true
}
```

## Get Fillups

<aside class="notice">Requires Authorization</aside>

```shell
curl -XGET "https://mpg.3dx2.com/api/fillUps.php" -H "Accept: application/json" -H "X-Authorization: $TOKEN"
```

This provides all fill ups.

### HTTP Request

`GET https://mpg.3dx2.com/api/fillUps.php`

### URL Parameters

Parameter | Type | Reqired | Description
--------- | ---- | ------- | -----------
ascdesc | Enum | Yes | Enum - `asc` or `desc`
vehicleId | Integer | No | If provided, returns all fill ups for that vehicle
fillUpId | Integer | No | If provided, returns only that fill up
mostRecent | Boolean | No | If provided, returns only the most recent fill up

### Response Body

> The above command returns JSON structured like this:

```json
[
  {
    "miles": "308.0",
    "gallons": "10.826",
    "fill_up_date": "2019-06-15 21:52:36",
    "id": 62615,
    "price_per_gal": "2.679",
    "odometer": "14028.0",
    "note": null,
    "fill_up_date_epoch": 1560660756
  },
  {
    "miles": "280.0",
    "gallons": "10.874",
    "fill_up_date": "2019-06-03 18:16:56",
    "id": 62338,
    "price_per_gal": "2.759",
    "odometer": "13720.0",
    "note": null,
    "fill_up_date_epoch": 1559611016
  }
]
```

# Maintainence

## Insert Automobile Maintenance Event

<aside class="notice">Requires Authorization</aside>

```shell
curl -XPOST "https://mpg.3dx2.com/api/maintenances.php" -H "Accept: application/json" -H "X-Authorization: $TOKEN"
```

This provides all fill ups for all vehicles in an account.

### Request Body

`POST https://mpg.3dx2.com/api/maintenances.php`

```json
{  
  "vehicleId":"13201",
  "servicePerformed":"Oil Change",
  "description":"Replaced the oil",
  "locationPerformed":"Dealer",
  "odometerReading":"12321.0",
  "date":"2019-07-08 21:53:00",
  "cost":"123.14"
}
```

Parameter | Type | Required | Description
--------- | ---- | -------- | -----------
vehicleId | Integer | Yes | The system ID for the vehicle
servicePerformed | String | Yes | The type of maintainence completed - `user provided`
description | String | No | `user provided`
locationPerformed | String | No | `user provided`
cost | Double | Yes | The price of the service
odometerReading | Integer | Yes | odometer mileage at service date
date | String | Yes | Date in YYYY-MM-dd HH:mm:ss format (2019-06-24 21:23:00)

### Response Body

> The above command returns JSON structured like this:

```json
{
  "status":true
}
```

## Update Automobile Maintainence Event

```shell
curl -XPUT "https://mpg.3dx2.com/api/maintenances.php" -H "Accept: application/json" -H "X-Authorization: $TOKEN"
```

This provides all fill ups for all vehicles in an account.

### Request Body

`PUT https://mpg.3dx2.com/api/maintenances.php`

```json
{  
  "serviceId":4244,
  "vehicleId":"13201",
  "servicePerformed":"Oil Change",
  "description":"Replaced the oil",
  "locationPerformed":"Dealer",
  "odometerReading":"12321.0",
  "date":"2019-07-08 03:00:00",
  "cost":"123.14"
}
```

Parameter | Type | Required | Description
--------- | ---- | -------- | -----------
serviceId | Integer | Yes | The system ID of the service
vehicleId | Inteer | Yes | The system ID for the vehicle
servicePerformed | String | Yes | The type of maintainence completed - `user provided`
description | String | Yes | `user provided`
locationPerformed | String | No | `user provided`
cost | Double | Yes | 123.21
odometerReading | Double | Yes | odometer mileage at service date
date | String | Yes | Date in YYYY-MM-dd HH:mm:ss format (2019-06-24 21:23:00)

### Response Body

> The above command returns JSON structured like this:

```json
{
  "status":true
}
```

## Delete Automobile Maintainence Event

```shell
curl -XDELETE "https://mpg.3dx2.com/api/maintenances.php" -H "Accept: application/json" -H "X-Authorization: $TOKEN"
```

This provides all fill ups for all vehicles in an account.

### Request Body

`DELETE https://mpg.3dx2.com/api/maintenances.php`

```json
{  
  "serviceId":4244
}
```

Variable | Type | Required | Description
-------- | ---- | -------- | -----------
serviceId | Integer | Yes | The system ID of the service
vehicleId | Integer | No | If provided, without the serviceId it will remove all maintenance for the vehicle.

### Response Body

> The above command returns JSON structured like this:

```json
{
  "status":true
}
```

## Get Maintainence Data

```shell
curl -XGET "https://mpg.3dx2.com/api/maintenances.php" -H "Accept: application/json" -H"X-Authorization: $TOKEN"
```

This provides all fill ups for all vehicles in an account.

### HTTP Request

`GET https://mpg.3dx2.com/api/maintenances.php`

### URL Parameters

Parameter | Type | Required | Description
--------- | ---- | -------- | -----------
ascdesc | Enum | No | `asc` or `desc`
vehicleId | Integer | No | If provided, limits result set to 1 vehicle
serviceId | Integer | No | If provided, only that service returned

### Response Body

> The above command returns JSON structured like this:

```json
[  
  {  
    "service_performed":"Oil Change",
    "mileage":"12321.0",
    "service_date":"2019-07-08",
    "id":4245,
    "cost":"123.14",
    "location_performed":"Dealer",
    "service_date_epoch":1562569200,
    "description":"Replaced the oil",
    "vehicleid":13201
  }
]
```

Variable | Type | Description
-------- | ---- |  -----------
id | Integer | Yes | The system ID of the service
vehicleid | Integer | No | If provided, without the serviceId it will remove all maintenance for the vehicle.
service_performed | String | work that was done on the vehicle
mileage | Double | The odometer at the time the work was done
service_date | String | Date of the service in format: YYYY-MM-dd
cost | Double | The price
location_performed | String | Where the work was done
service_date_epoch | Integer | Seconds after epoch for processing
description| String | Any additional information the user provides

# Vehicle Management

## Create Vehicle

```shell
curl -XPOST "https://mpg.3dx2.com/api/vehicles.php" -H "Accept: application/json" -H "X-Authorization: $TOKEN"
```

Inserts a new vehicle.

### HTTP Request

`POST https://mpg.3dx2.com/api/vehicles.php`

```json
{
  "name":"Test Car 3",
  "make":"Honda",
  "model":"CR-V",
  "odometer":"32",
  "nextOilChange":"2000",
  "oilChangeDuration":3000,
  "displayUnits":"mpg",
  "defaultFillUpMode":"odometer"
}
```

Variable | Type | Required | Description
--------- | ---- | -------- | -----------
name | String | Yes | User chosen name for the vehicle
make | String | Yes | The vehicle manufacturer
model | String | Yes | The model of the vehicle
odometer | Double | Yes | Odometer reading at last fill up
nextOilChange | Double | Yes | Mileage of next oil change
oilChangeDuration | Double | Yes | The # of miles between oil changes
displayUnits | Enum | Yes | Enum `mpg` / `kpg` / `kpl`
defaultFillUpMode | Enum | Yes | Enum `tripometer` / `odometer`

### Response Body

> The above command returns JSON structured like this:

```json
{  
  "status":"true",
  "vehicleId":"13239"
}
```

## Update Vehicle

```shell
curl -XPUT "https://mpg.3dx2.com/api/vehicles.php" -H "Accept: application/json" -H "X-Authorization: $TOKEN"
```

Updates a vehicle's settings.

### HTTP Request

`PUT https://mpg.3dx2.com/api/vehicles.php`

```json
{  
  "vehicleId":13239,
  "name":"Test Car 3",
  "make":"Honda",
  "model":"CR-V",
  "odometer":"32.0",
  "nextOilChange":"2000.0",
  "oilChangeDuration":3000,
  "displayUnits":"mpg",
  "defaultFillUpMode":"odometer"
}
```

Variable | Type | Required | Description
--------- | ---- | -------- | -----------
vehicleId | Integer | Yes | ID to update
name | String | Yes | User chosen name for the vehicle
make | String | Yes | The vehicle manufacturer
model | String | Yes | The model of the vehicle
odometer | Double | Yes | Odometer reading at last fill up
nextOilChange | Double | Yes | Mileage of next oil change
oilChangeDuration | Double | Yes | The # of miles between oil changes
displayUnits | Enum | Yes | Enum `mpg` / `kpg` / `kpl`
defaultFillUpMode | Enum | Yes | Enum `tripometer` / `odometer`

### Response Body

> The above command returns JSON structured like this:

```json
{  
  "status":"true"
}
```

## Delete Vehicle

```shell
curl -XDELETE "https://mpg.3dx2.com/api/vehicles.php" -H "Accept: application/json" -H "X-Authorization: $TOKEN"
```

Deletes a vehicle.

### Request Body

`DELETE https://mpg.3dx2.com/api/vehicles.php`

```json
{  
  "vehicleId":13239,
  "deleted":1
}
```

Variable | Type | Required | Description
--------- | ---- | -------- | -----------
vehicleId | Integer | Yes | System ID for vehicle
deleted | Integer | Yes | 1 for deleted, 0 for not deleted

### Response Body

> The above command returns JSON structured like this:

```json
{  
   "status":"true"
}
```

## Get Vehicles

```shell
curl "https://mpg.3dx2.com/api/vehicles.php?vehicleid=12883&userid=1000000001" -H "Accept: application/json" -H "X-Authorization: $TOKEN"
```

Gets vehicle for a user.

### HTTP Request

`GET https://mpg.3dx2.com/api/vehicles.php`

### URL Parameters

Variable | Type | Required | Description
-------- | ---- | -------- | -----------
vehicleId | Integer | No | If provided, responds with only this one requested

### Response Body

> The above command returns JSON structured like this:

```json
[
  {  
    "id":13201,
    "name":"Test CArd 2",
    "make":"asdv",
    "model":"asd",
    "odometer":"45.0",
    "next_oil_change":"3423.0",
    "oil_change_duration":3242,
    "display_units":"kpg",
    "default_fill_up_mode":"tripometer",
    "default_vehicle":0
  }
]
```

## Get Default Vehicle

```shell
curl -XGET "https://mpg.3dx2.com/api/defaultVehicle.php" -H "Accept: application/json" -H "X-Authorization: $TOKEN"
```

Gets default vehicle for a user.

### HTTP Request

`GET https://mpg.3dx2.com/api/defaultVehicle.php`

### Response Body

> The above command returns JSON:

```json
{
  "id":9326,
  "name":"Gertrude",
  "make":"Subaru",
  "model":"Impreza",
  "odometer":"69723.0",
  "next_oil_change":"70140.0",
  "oil_change_duration":6000,
  "display_units":"mpg",
  "default_fill_up_mode":"odometer",
  "default_vehicle":1
}
```

## Set Default Vehicle

```shell
curl -XPUT "https://mpg.3dx2.com/api/defaultVehicle.php" -H "Accept: application/json" -H "X-Authorization: $TOKEN" -d '{ "vehicleId": 9326 }'
```

Sets default vehicle for a user.

### HTTP Request

`PUT https://mpg.3dx2.com/api/defaultVehicle.php`

### Request Body

Variable | Type | Required | Description
-------- | ---- | -------- | -----------
vehicleId | Integer | Yes | The ID to be set as the default vehicle.

### Response Body

> The above command returns JSON:

```json
 {
  "id":9326,
  "name":"Gertrude",
  "make":"Subaru",
  "model":"Impreza",
  "odometer":"69723.0",
  "next_oil_change":"70140.0",
  "oil_change_duration":6000,
  "display_units":"mpg",
  "default_fill_up_mode":"odometer",
  "default_vehicle":1
}
```

# Android Management

## Remove Ads

```shell
curl -XPOST "https://mpg.3dx2.com/api/removeAds.php" -H "Accept: application/json" -H "X-Authorization: $TOKEN" -d '{ "removeAds": true }'
```

Tracks when a user toggles ad removal.

### HTTP Request

`PUT https://mpg.3dx2.com/api/removeAds.php`

### Request Body

Variable | Type | Required | Description
-------- | ---- | -------- | -----------
removeAds | Boolean | Yes | Whether or not ads are removed

### Response Body

> The above command returns JSON structured like this:

```json
{
  "status":true
}
```

## Insert Error

```shell
curl "https://mpg.3dx2.com/insertError.php" -H "Accept: application/json" -H "X-Authorization: $TOKEN"
```

> The above command returns JSON structured like this:

```json
{
  "status":true
}
```

Tracks when a user removes ads.

### HTTP Request

`GET https://mpg.3dx2.com/insertError.php`

### URL Parameters

Parameter | Description
--------- | -----------
errorFrom | The place the error occurred
errorDetails | The stacktrace details
