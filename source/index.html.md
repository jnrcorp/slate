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

# Login

## Website Login

Performs system login for a user.

```shell
curl "https://mpg.3dx2.com/websiteLogin.php?email=$EMAIL&password=$PASSWORD"
```

> The above command returns JSON structured like this:

```json
{  
   "data":{  
      "userId":"1000000001",
      "vehicleId":9326,
      "userName":"Joshua"
   }
}
```

### HTTP Request

`GET https://mpg.3dx2.com/websiteLogin.php`

### Query Parameters

Parameter | Required | Description
--------- | -------- | -----------
email | yes | User's email address
password | yes | User's Password

This endpoint performs a login with the username and password

<aside class="notice">
You must replace <code>email</code> and <code>password</code> with the appropriate combo.
</aside>

## Create User

Creates a new user in the system.

### HTTP Request

`GET https://mpg.3dx2.com/createuser.php`

### Query Parameters

Parameter | Required | Description
--------- | -------- | -----------
email | yes | User's email address
password | yes | User's Password
name | yes | User's Full Name

## Auto Create User

```shell
curl "https://mpg.3dx2.com/autoCreateUser.php?email=$EMAIL&name=$FULL_NAME&password=$PASSWORD"
```

> The above command returns pipe delimited data like this:

```
1000000001|Joshua|mpg|joshua.ross@gmail.com|1
```

This endpoint retrieves user data.

<aside class="warning">This call responds with pipe delimited data, not JSON.</aside>

### HTTP Request

`GET https://mpg.3dx2.com/autoCreateUser.php`

### Query Parameters

Parameter | Required | Description
--------- | -------- | -----------
email | yes | User's email address
password | yes | User's Password
name | yes | User's Full Name

# Fill Ups

## Insert Fill Up

```shell
curl "https://mpg.3dx2.com/insertFillUp.php"
```

> The above command returns JSON structured like this:

```json
{
  "status": true
}
```

This inserts a new fill up.

### HTTP Request

`GET https://mpg.3dx2.com/insertFillUp.php`

### URL Parameters

Parameter | Description
--------- | -----------
userid | The logged in User ID
vehicleId | The ID of the vehicle being filled up
milesDriven | Miles driven since last fill up
gasUsed | Amount of gas used to fill tank
dateFilled | Date of the fill up (e.g. `2019-06-23 14:08:00 -0400`)
pricePerGal | Price per gallon of gas to the 10 of a cent
odometer | Odometer reading at fill up
note | A custom user note

## Update Fill Up

```shell
curl "https://mpg.3dx2.com/updateFillUp.php"
```

> The above command returns JSON structured like this:

```json
{
  "status": true
}
```

This updates an existing fill up.

### HTTP Request

`GET https://mpg.3dx2.com/updateFillUp.php`

### URL Parameters

Parameter | Description
--------- | -----------
fillupid | The ID of the fill up being updated
userid | The logged in User ID
vehicleId | The ID of the vehicle being filled up
miles | Miles driven since last fill up
gallons | Amount of gas used to fill tank
date | Date of the fill up (e.g. `2019-06-23 14:08:00 -0400`)
price | Price per gallon of gas to the 10 of a cent
odometer | Odometer reading at fill up
note | A custom user note

## Delete a Fill Up

```shell
curl "https://mpg.3dx2.com/removeFillUp.php"
```

> The above command returns JSON structured like this:

```json
{
  "status": true
}
```

This removes an existing fill up.

### HTTP Request

`GET https://mpg.3dx2.com/removeFillUp.php`

### URL Parameters

Parameter | Description
--------- | -----------
fillupid | The ID of the fill up being updated
userid | The logged in User ID

## Get Fillups

```shell
curl "https://mpg.3dx2.com/selectAllFillUps.php" -H "Accept: application/json"
```

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

This provides all fill ups.

### HTTP Request

`GET https://mpg.3dx2.com/selectAllFillUps.php`

### URL Parameters

Parameter | Description
--------- | -----------
userid | The logged in User ID
ascdesc | Enum - `asc` or `desc`
vehicleid | The system ID for the vehicle

## Get Fillups for All Vehicles

```shell
curl "https://mpg.3dx2.com/selectAllFillUpsAllVehicles.php" -H "Accept: application/json"
```

> The above command returns JSON structured like this:

```json
[
  {
    "vehicleid": 1,
    "miles": "232.0",
    "gallons": "11.000",
    "fill_up_date": "2011-04-16 00:00:00",
    "id": 244,
    "price_per_gal": null,
    "odometer": null,
    "note": null,
    "fill_up_date_epoch": 1302937200
  }
]
```

This provides all fill ups for all vehicles in an account.

### HTTP Request

`GET https://mpg.3dx2.com/selectAllFillUpsAllVehicles.php`

### URL Parameters

Parameter | Description
--------- | -----------
userid | The logged in User ID
ascdesc | Enum - `asc` or `desc`

## Get Most Recent Fillup

```shell
curl "https://mpg.3dx2.com/selectMostRecentFillUp.php?userid=1000000001" -H "Accept: application/json"
```

> The above command returns JSON structured like this:

```json
{  
   "miles":"315.0",
   "gallons":"10.098",
   "fill_up_date":"2019-06-23 20:29:59",
   "id":62784,
   "price_per_gal":"2.639",
   "odometer":"69393.0",
   "note":null,
   "fill_up_date_epoch":1561346999
}
```

This provides the most recent fill up for a vehicle. If the vehicle ID is not provided, it'll load the default vehicle ID.

### HTTP Request

`GET https://mpg.3dx2.com/selectMostRecentFillUp.php`

### URL Parameters

Parameter | Required | Description
--------- | -------- | -----------
userid | Yes | The logged in User ID
vehicleid | No | The ID of the vehicle. If not provided, pulls for default vehicle

# Maintainence

## Insert Automobile Maintainence Event

```shell
curl "https://mpg.3dx2.com/insertService.php" -H "Accept: application/json"
```

> The above command returns JSON structured like this:

```json
{
  "status":true
}
```

This provides all fill ups for all vehicles in an account.

### HTTP Request

`GET https://mpg.3dx2.com/insertService.php`

### URL Parameters

Parameter | Example | Description
--------- | ------- | -----------
userid | 101 | The logged in User ID
vehicleid | 101 | The system ID for the vehicle
servicePerformed | Oil Change | The type of maintainence completed - `user provided`
description | Replaced the oil | `user provided`
locationPerformed | Midas | `user provided`
cost | Price of the | 123.21
odometerReading | 31492 | odometer mileage at service date
date | 2019-06-24 21:23:00 | Date in YYYY-MM-dd HH:mm:ss format

## Update Automobile Maintainence Event

```shell
curl "https://mpg.3dx2.com/updateService.php" -H "Accept: application/json"
```

> The above command returns JSON structured like this:

```json
{
  "status":true
}
```

This provides all fill ups for all vehicles in an account.

### HTTP Request

`GET https://mpg.3dx2.com/updateService.php`

### URL Parameters

Parameter | Example | Description
--------- | ------- | -----------
userid | 101 | The logged in User ID
vehicleid | 101 | The system ID for the vehicle
serviceId | 101 | The system ID of the service
servicePerformed | Oil Change | The type of maintainence completed - `user provided`
description | Replaced the oil | `user provided`
locationPerformed | Midas | `user provided`
cost | Price of the | 123.21
odometerReading | 31492 | odometer mileage at service date
date | 2019-06-24 21:23:00 | Date in YYYY-MM-dd HH:mm:ss format

## Delete Automobile Maintainence Event

```shell
curl "https://mpg.3dx2.com/deleteService.php" -H "Accept: application/json"
```

> The above command returns JSON structured like this:

```json
{
  "status":true
}
```

This provides all fill ups for all vehicles in an account.

### HTTP Request

`GET https://mpg.3dx2.com/deleteService.php`

### URL Parameters

Parameter | Example | Description
--------- | ------- | -----------
userid | 101 | The logged in User ID
serviceId | 101 | The system ID of the service

## Delete All Automobile Maintainence Event

```shell
curl "https://mpg.3dx2.com/deleteServices.php" -H "Accept: application/json"
```

> The above command returns JSON structured like this:

```json
{
  "status":true
}
```

This provides all fill ups for all vehicles in an account.

### HTTP Request

`GET https://mpg.3dx2.com/deleteServices.php`

### URL Parameters

Parameter | Example | Description
--------- | ------- | -----------
userid | 101 | The logged in User ID
vehicleid | 101 | The system ID for the vehicle

## Get Maintainence for Specifc Vehicle

```shell
curl "https://mpg.3dx2.com/selectAllServices.php" -H "Accept: application/json"
```

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

This provides all fill ups for all vehicles in an account.

### HTTP Request

`GET https://mpg.3dx2.com/selectAllServices.php`

### URL Parameters

Parameter | Description
--------- | -----------
userid | The logged in User ID
ascdesc | Enum - `asc` or `desc`
vehicleid | Vehicle ID

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
