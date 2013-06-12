LOCATION
=======
With this module you can work with geolocation. Retrieve information about places around a point or get information about people near someone or someplace. To do so, all request have to go to:

    http://appnima-user.eu01.aws.af.cm/{RESOURCE}

Remember all requests to App/nima should be identified by your `Appnima.key` or key pair `client` and `secret`.


So, the first parameter is the type of request (GET, POST, UPDATE, DELETE …) and the second the name of resource.

## GET /places
--------------
Use this resource to get places around a point. You can determinate the range result with radius. Sends the request with latitude, longitude and optionally the radius (in meters):

```json
    {
		latitude:		"-33.9250334",
		longitude:		"18.423883499999988",
		radio:			"500"
	}
```
Responses are returned with `200 Ok` and the list of places:

```json
    [{
		id:		   		120949303434,
		reference: 		"CqQBlwAAAEXdx350jL2InIRtksTkbZJ-m",
		location:
			0: 	-33.9242692,
			1: 	18.4187029
		place_name:		"Cape Town City Centre",
		vicinity: 		"Cape Town City Centre"
	},
	{
		id:		   		120949303435,
		created_at: 	"2013-05-25T21:08:12.087Z",
		location:
			0: 	-33.9249,
			1: 	18.4241
		place_name:		"Wang Thai - V&A Waterfront",
		vicinity: 		"Queen Victoria Street, Cape Town"
	}
	]
```

## GET /place
--------------
With this resource you can request more details about a particular establishment. Sends the request with the next parameters:

```json
    {
		id:				"120949303434",
		reference:		"CqQBlwAAAEXdx350jL2InIRtksTkbZJ-m"
	}
```


Responses are returned with `200 Ok` and the place detail:

```json
    {
		id:		   		120949303434,
		address: 		"Elorduigoitia Kalea",
		country: 		"ES"
		created_at: 	"2013-06-03T10:02:29.951Z"
		locality: 		"Mungia"
		name: 			"Cruz Roja Española"
		phone: 			"+34 946 74 21 51"
		reference: 		"CqQBlwAAAEXdx350jL2InIRtksTkbZJ-m",
		created_at: 	"2013-05-25T21:08:12.087Z",
		position:
			0: 43.354585
			1: -2.846662
		postal_code: 	"48100"
		reference: 		"CqQBlwAAAEXdx350jL2InIRtksTkbZJ-m",
		website: 		"http://www.cruzroja.es/"
	}
```

## POST /place
--------------

Use this resource to create place data profile. . Sends the request with the next parameters:

```json
    {
		name: "San Mames",
        address: "Felipe Serrate, s/n",
		locality: "Bilbao",
		postal_code: "48013",
		country: "ES",
		latitude: " ﻿43.26",
		longitude: "-2.94"
	}
``` 

Optionally you can add the next information:

```json
    {
		mail: "hello@sanmames.com",
        phone: "944411445",
		website: "www.sanmames.com"
	}
```

Responses are returned with `201 Created`.

## POST /checkin
--------------
Users from your applicaction can register visits into a place. Use this resource with the next parameters: 

```json
    {
		id:				"120949303434",
		reference:		"CqQBlwAAAEXdx350jL2InIRtksTkbZJ-m"
	}
```

Responses are returned with `200 Ok` and the object:

```json
    {
		id:		   		120949303434,
		reference: 		"CqQBlwAAAEXdx350jL2InIRtksTkbZJ-m",
		created_at: 	"2013-05-26T20:48:05.786Z",
		latitude: 		-33.9249,
		longitude: 		18.4241,
		name: 			"Wang Thai - V&A Waterfront",
		phone: 			"+27 21 421 8702",
		reference: 		"CqQBlwAAAEXdx350jL2InIRtksTkbZJ-m",
		vicinity: 		"Queen Victoria Street, Cape Town"
	}
```


## GET /checkin
--------------
Get a list of saved places by your users. Just send the user id: 


```json
    {
		id:				"120949303434"
	}
```

Responses are returned with `200 Ok` and the object:

```json
    {
		id:		   		120949303434,
		reference: 		"CqQBlwAAAEXdx350jL2InIRtksTkbZJ-m",
		created_at: 	"2013-05-26T20:48:05.786Z",
		latitude: 		-33.9249,
		longitude: 		18.4241,
		name: 			"Wang Thai - V&A Waterfront",
		phone: 			"+27 21 421 8702",
		reference: 		"CqQBlwAAAEXdx350jL2InIRtksTkbZJ-m",
		vicinity: 		"Queen Victoria Street, Cape Town"
	}
```


## GET /friends
--------------
Shown to your users information about friends near to a point. You just need send with de request the following parametrs:


```json
    {
		latitude:		"-33.9250334",
		longitude:		"18.423883499999988",
		radio:			"500"
	}
```

If the request was successful App/nima returns `200 Ok` and the user data:

```json
	{
		id: 		120949303431,
		username: 	"haas85",
		name:   	"Iñigo",
		avatar:		"AVATAR_URL"
	}
```

## GET /people
--------------
As **GET /friends** you can provides information about people who are not into users list friends. The request is made with the same parameters:

```json
    {
		latitude:		"-33.9250334",
		longitude:		"18.423883499999988",
		radio:			"500"
	}
```