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

> A successful response will look like this:

```json
{
  "data": {
    "userId":100001,
    "userToken":"some-large-entropy-response",
    "vehicleId":1,
    "fullName":"John Doe",
    "removeAds":1
  }
}
```

> A failed response will look like this:

```json
{
  "error": "Invalid Email or Password"
}
```

### Response Body

Variable | Type | Required | Description
-------- | ---- | -------- | -----------
userId | Integer | Yes | A unique identifier for use with Google Analytics
userToken | String | Yes | A very long unique session tokenize
vehicleId | Integer | Yes | The default vehicle for this user
fullName | String | No | The user's full name if they provided it
removeAds | Boolean | Yes | Whether the user has removed ads in the app

> This endpoint performs a login with the username and password

<aside class="notice">
You must replace <code>email</code> and <code>password</code> with the appropriate combo.
</aside>

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
  "data": {
    "userId":100001,
    "userToken":"some-large-entropy-response",
    "vehicleId":1,
    "fullName":"John Doe",
    "removeAds":1
  }
}
```

> A failed response will look like this:

```json
{
  "error": "Invalid Email or Password"
}
```

Variable | Type | Required | Description
-------- | ---- | -------- | -----------
userId | Integer | Yes | A unique identifier for use with Google Analytics
userToken | String | Yes | A very long unique session tokenize
vehicleId | Integer | Yes | The default vehicle for this user
fullName | String | Yes | The user's full name
removeAds | Boolean | Yes | Whether the user has removed ads in the app

## Create User

Creates a new user in the system.

```shell
curl -X POST \
  -H "Accept: application/json" \
  --data $BODY_JSON \
  "https://mpg.3dx2.com/api/userRegister.php"
```

### HTTP Request

`GET https://mpg.3dx2.com/api/userRegister.php`

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
  "data": {
    "userId":100001,
    "userToken":"some-large-entropy-response",
    "vehicleId":1,
    "fullName":"John Doe",
    "removeAds":1
  }
}
```

Variable | Type | Required | Description
-------- | ---- | -------- | -----------
userId | Integer | Yes | A unique identifier for use with Google Analytics
userToken | String | Yes | A very long unique session tokenize
vehicleId | Integer | Yes | The default vehicle for this user
fullName | String | Yes | The user's full name
removeAds | Boolean | Yes | Whether the user has removed ads in the app

> A failed response will return something like this:

```json
{
  "error": "dupe|vehicle|captchaFail|pwmismatch"
}
```

Variable | Type | Required | Description
-------- | ---- | -------- | -----------
error | String | Yes | `Enum` dupe, vehicle, captchaFail, pwmismatch

# Authorization

All API calls after login require the user token provided from the authentication API call to be included in the header, <code>X-Authorization</code>.

```shell
curl "api_endpoint_here" -H "X-Authorization: the-provided-user-token"
```

> Make sure to replace `the-provided-user-token` with your API key.

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
curl -XPUT "https://mpg.3dx2.com/api/fillUps.php"
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
curl -XDELETE "https://mpg.3dx2.com/api/fillUps.php"
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
curl -XGET "https://mpg.3dx2.com/api/fillUps.php" -H "Accept: application/json"
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
curl -XPOST "https://mpg.3dx2.com/api/maintenances.php" -H "Accept: application/json"
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
curl -XPUT "https://mpg.3dx2.com/api/maintenances.php" -H "Accept: application/json"
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
curl -XDELETE "https://mpg.3dx2.com/api/maintenances.php" -H "Accept: application/json"
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

## Get Maintainence for Specifc Vehicle

```shell
curl -XGET "https://mpg.3dx2.com/api/maintenances.php" -H "Accept: application/json"
```

This provides all fill ups for all vehicles in an account.

### HTTP Request

`GET https://mpg.3dx2.com/api/maintenances.php`

### URL Parameters

Parameter | Description
--------- | -----------
userid | The logged in User ID
ascdesc | Enum - `asc` or `desc`
vehicleid | Vehicle ID

### Response Body

> The above command returns JSON structured like this:

```json
[  
   {  
      "service_performed":"New Brakes",
      "mileage":"31492.0",
      "service_date":"2019-06-24",
      "id":4231,
      "cost":"390.23",
      "location_performed":"Dealer",
      "service_date_epoch":1561359600,
      "description":"Brakes"
   }
]
```

Variable | Type | Required | Description
-------- | ---- | -------- | -----------
id | Integer | Yes | The system ID of the service
vehicleId | Integer | No | If provided, without the serviceId it will remove all maintenance for the vehicle.

## Get Maintainence for All Vehicles

```shell
curl "https://mpg.3dx2.com/selectAllServicesAllVehicles.php" -H "Accept: application/json"
```

> The above command returns JSON structured like this:

```json
[  
   {  
      "vehicleid":12883,
      "service_performed":"New Brakes",
      "mileage":"31492.0",
      "service_date":"2019-06-24",
      "id":4231,
      "cost":"390.23",
      "location_performed":"Dealer",
      "service_date_epoch":1561359600,
      "description":"Brakes"
   }
]
```

This provides all fill ups for all vehicles in an account.

### HTTP Request

`GET https://mpg.3dx2.com/selectAllServices.php`

### URL Parameters

Parameter | Description
--------- | -----------
userid | The logged in User ID
ascdesc | Enum - `asc` or `desc`

# Vehicle Management

## Insert/Update Mileage

```shell
curl "https://mpg.3dx2.com/insertUpdateMileage.php" -H "Accept: application/json"
```

> The above command returns JSON structured like this:

```json
{
  "status":true
}
```

Inserts or updates the mileage for a vehicle.

### HTTP Request

`GET https://mpg.3dx2.com/insertUpdateMileage.php`

### URL Parameters

Parameter | Example | Description
--------- | ------- | -----------
userid | 1000000001 | The logged in User ID
defaultFillUpMode | tripometer | Enum `tripometer` / `odometer`
mode | mpg | Enum `mpg` / `kpg` / `kpl`
nextOilChange | 3000.0 | Mileage of next oil change
odometer | 1150.0 | Odometer reading at last fill up
oilChangeDuration | 3000 | The # of miles between oil changes
vehicleId | 12883 | The id of the vehicle

## Insert Vehicle

```shell
curl "https://mpg.3dx2.com/insertVehicle.php" -H "Accept: application/json"
```

> The above command returns JSON structured like this:

```json
{  
   "status":"true",
   "vehicleId":"13186"
}
```

Inserts or updates the mileage for a vehicle.

### HTTP Request

`GET https://mpg.3dx2.com/insertVehicle.php`

### URL Parameters

Parameter | Example | Description
--------- | ------- | -----------
userid | 1000000001 | The logged in User ID
name | Test card 2 | User chosen name for the vehicle
make | Honda | The vehicle manufacturer
model | CR-V | The model of the vehicle

## Update Vehicle

```shell
curl "https://mpg.3dx2.com/updateVehicle.php" -H "Accept: application/json"
```

> The above command returns JSON structured like this:

```json
{  
   "status":"true"
}
```

Inserts or updates the mileage for a vehicle.

### HTTP Request

`GET https://mpg.3dx2.com/updateVehicle.php`

### URL Parameters

Parameter | Example | Description
--------- | ------- | -----------
userid | 1000000001 | The logged in User ID
vehicleid | 101 | System ID for vehicle
name | Test card 2 | User chosen name for the vehicle
make | Honda | The vehicle manufacturer
model | CR-V | The model of the vehicle

## Delete Vehicle

```shell
curl "https://mpg.3dx2.com/deleteVehicle.php" -H "Accept: application/json"
```

> The above command returns JSON structured like this:

```json
{  
   "status":"true"
}
```

Inserts or updates the mileage for a vehicle.

### HTTP Request

`GET https://mpg.3dx2.com/deleteVehicle.php`

### URL Parameters

Parameter | Example | Description
--------- | ------- | -----------
userid | 1000000001 | The logged in User ID
vehicleid | 101 | System ID for vehicle
deleted | 1 | 1 for deleted, 0 for not deleted

## Get Vehicle

```shell
curl "https://mpg.3dx2.com/getVehicle.php?vehicleid=12883&userid=1000000001" -H "Accept: application/json"
```

> The above command returns JSON structured like this:

```json
 {  
   "id":12883,
   "name":"Test Car",
   "make":"",
   "model":"",
   "odometer":"1150.0",
   "next_oil_change":"3000.0",
   "oil_change_duration":3000,
   "display_units":"mpg",
   "default_fill_up_mode":"tripometer",
   "default_vehicle":0
}
```

Gets vehicle for a user.

### HTTP Request

`GET https://mpg.3dx2.com/getVehicle.php`

### URL Parameters

Parameter | Example | Description
--------- | ------- | -----------
userid | 1000000001 | The logged in User ID

## Get Vehicles

```shell
curl "https://mpg.3dx2.com/getVehicles.php" -H "Accept: application/json"
```

> The above command returns JSON structured like this:

```json
[
   {
      "id":12883,
      "name":"Test Car",
      "make":"",
      "model":"",
      "odometer":"1150.0",
      "next_oil_change":"3000.0",
      "oil_change_duration":3000,
      "display_units":"mpg",
      "default_fill_up_mode":"tripometer",
      "default_vehicle":0
   },
   {
      "id":13187,
      "name":"Test CArd 2",
      "make":"asdv",
      "model":"asd",
      "odometer":"32.0",
      "next_oil_change":"2000.0",
      "oil_change_duration":1233,
      "display_units":"mpg",
      "default_fill_up_mode":"tripometer",
      "default_vehicle":0
   }
]
```

Gets all vehicles for a user.

### HTTP Request

`GET https://mpg.3dx2.com/getVehicles.php`

### URL Parameters

Parameter | Example | Description
--------- | ------- | -----------
userid | 1000000001 | The logged in User ID

## Get Default Vehicle

```shell
curl "https://mpg.3dx2.com/getDefaultVehicle.php?userid=1000000001" -H "Accept: application/json"
```

> The above command returns plain text:

```text
12883
```

Gets default vehicle for a user.

### HTTP Request

`GET https://mpg.3dx2.com/getDefaultVehicle.php`

### URL Parameters

Parameter | Example | Description
--------- | ------- | -----------
userid | 1000000001 | The logged in User ID

## Set Default Vehicle

```shell
curl "https://mpg.3dx2.com/setDefaultVehicle.php?userid=1000000001&vehicleid=9326" -H "Accept: application/json"
```

> The above command returns JSON:

```json
 {
   "status":true
 }
```

Sets default vehicle for a user.

### HTTP Request

`GET https://mpg.3dx2.com/setDefaultVehicle.php`

### URL Parameters

Parameter | Example | Description
--------- | ------- | -----------
userid | 1000000001 | The logged in User ID
vehicleid | 101 | System ID for vehicle

# Android Management

## Remove Ads

```shell
curl "https://mpg.3dx2.com/removeAds.php?userid=1000000001&removeAds=true" -H "Accept: application/json"
```

> The above command returns JSON structured like this:

```json
{
  "status":true
}
```

Tracks when a user removes ads.

### HTTP Request

`GET https://mpg.3dx2.com/removeAds.php`

### URL Parameters

Parameter | Description
--------- | -----------
userid | The logged in User ID
removeAds | Boolean

## Insert Error

```shell
curl "https://mpg.3dx2.com/insertError.php" -H "Accept: application/json"
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
