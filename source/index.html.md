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
