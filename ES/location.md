LOCATION
=======

Este módulo te permite obtener toda la funcionalidad respecto a la geolocalización de usuarios y lugares. Mediante latitud y longitud obtén los sitios en un radio determinado así como los detalles de un lugar concreto. Si tu aplicación lo requiere, también puedes registrar un sitio mediante checkins y además ofrecer a tus usuarios información sobre amigos cercanos.

Para ello ten en cuenta que todas las peticiones que hagas tendrán que ir a:

	http://appnima-user.eu01.aws.af.cm/location/{RECURSO}

Recuerda que todas las peticiones que hagas a App/nima tienen que ir identificadas con tu `Appnima.key` o bien con el par de datos `client` y `secret`. Ahora veamos los recursos que puedes utilizar: el primer parámetro indica el tipo de petición (GET, POST, UPDATE, DELETE …) y el segundo el nombre del recurso.


## GET /places
--------------
Con este recurso puedes obtienes una lista de lugares alrededor de un punto en un radio determinado. Envía como parámetros la latitud, longitud y opcionalmente el radio (en metros):

```json
    {
		latitude:		"-33.9250334",
		longitude:		"18.423883499999988",
		radio:			"500"
	}
```
En el caso de que haya respuesta, se devuelve un `200 Ok` junto con una lista de lugares e información relacionada:

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
Utiliza este recurso para obtener información detallada de un sitio en concreto. Envía junto con la petición los parámetros id y reference:

```json
    {
		id:				"120949303434",
		reference:		"CqQBlwAAAEXdx350jL2InIRtksTkbZJ-m"
	}
```

Si la consulta ha obtenido respuesta se devuelve un `200 Ok` junto con el objeto:


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
Utiliza este recurso para dar de alta un sitio. Envía junto con la petición los siguientes parámetros:

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
Opcionalmente se pueden añadir los siguientes parámetros:

```json
    {
		mail: "hello@sanmames.com",
        phone: "944411445",
		website: "www.sanmames.com"
	}
```

Si la consulta ha obtenido respuesta se devuelve un `201 Created`.



## POST /checkin
--------------
Los usuarios de tu aplicación pueden registrar visitas a sitios concretos. Para ello utiliza este recurso pasando como parámetros:

```json
    {
		id:				"120949303434",
		reference:		"CqQBlwAAAEXdx350jL2InIRtksTkbZJ-m"
	}
```

Si todo ha salido bien, se devuelve un `200 Ok` junto con el objeto:

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
Obtén la lista de sitios guardados por tus usuarios con este recurso. Para ello únicamente tienes que enviar el id del usuario en la petición:

```json
    {
		id:				"120949303434"
	}
```

Si ha salido todo bien, obtienes un `200 Ok` junto con el objeto:

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
Proporciona a tus usuarios información sobre amigos cercanos a un punto determinado. Para ello utiliza este recurso pasando los siguientes parámetros:

```json
    {
		latitude:		"-33.9250334",
		longitude:		"18.423883499999988",
		radio:			"500"
	}
```

Si todo ha salido bien, se recibe un `200 Ok` junto con el objeto:

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
De la misma forma que puedes mostrar a un usuarios qué amigos están a su alrededor, puedes por ejemplo proponer gente fuera de sus listas de amigos. La petición se realiza con los mismos parámetros:


```json
    {
		latitude:		"-33.9250334",
		longitude:		"18.423883499999988",
		radio:			"500"
	}
```