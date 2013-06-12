NETWORK
=======

Este modulo recoge toda la funcionalidad para crear una red social dentro de tu aplicación; buscar usuarios, seguirlos (o no seguirlos, tu decides), listas de seguidores... Para ello ten en cuenta que todas las peticiones que hagas tendrán que ir a:

	http://appnima-user.eu01.aws.af.cm/network/{RECURSO}
	
Recuerda que todas las peticiones que hagas a App/nima tienen que ir identificadas con tu `Appnima.key` o bien con el par de datos `client` y `secret`. Ahora veamos los recursos que puedes utilizar, para ello el primer parámetro indica el tipo de petición (GET, POST, UPDATE, DELETE …) y el segundo parámetro el nombre del recurso.


## GET /search
--------------
Si necesitas buscar usuarios dentro de tu aplicación debes utilizar este recurso el cual únicamente recibe un único parámetro y con el cual App/nima hará todo el trabajo difícil buscando los mejores resultados:

```json
    {
		query:		"javi@tapquo.com"
	}
```

En el caso de que la respuesta haya sido satisfactoria se devolverá un `200 Ok` junto con una lista de usuarios coincidentes a esa búsqueda:

```json
    [{
		id:		    120949303434,
		username: 	"soyjavi",
		name:   	"Javi",
		avatar:		"AVATAR_URL"
	},
	{
		id:	    	120949303433,
		username: 	"cataflu",
		name:   	"Catalina",
		avatar:		"AVATAR_URL"
	},
	{
		id: 		120949303431,
		username: 	"haas85",
		name:   	"Iñigo",
		avatar:		"AVATAR_URL"
	}
	]
```


## POST /follow
---------------
Si necesitas seguir a un usuario utiliza este recurso junto con el parámetro: 

```json
    {
		user:		23094392049024
	}
```

Devolverá un `200 Ok` junto con el objeto:

```json
    {
		status:		'ok'
	}
```


## POST /unfollow
-----------------
Al igual que el recurso **POST /follow** funciona de igual manera y únicamente tendrás que enviar el parámetro:

```json
    {
		user:		23094392049024
	}
```

Devolverá un `200 Ok` junto con el objeto:

```json
    {
		status:		'ok'
	}
```


## GET /following
-----------------
Funciona de igual manera que **POST /follow**  y únicamente tendrás que enviar como parámetro el usuario del que quieres conocer la lista de usuarios que esta siguiendo:

```json
    {
		user:		23094392049024
	}
```

Devolverá un `200 Ok` junto con lista de usuarios que sigue el usuario indicado:

```json
    [{
		id:		    120949303434,
		username: 	"soyjavi",
		name:   	"Javi",
		avatar:		"AVATAR_URL"
	},
	{
		id:	    	120949303433,
		username: 	"cataflu",
		name:   	"Catalina",
		avatar:		"AVATAR_URL"
	},
	{
		id: 		120949303431,
		username: 	"haas85",
		name:   	"Iñigo",
		avatar:		"AVATAR_URL"
	}
	]
```


## GET /followers
-----------------
Funciona de igual manera que **GET /following**  y esta vez tendrás que enviar como parámetro el usuario del que quieres conocer la lista de usuarios que le están siguiendo:

```json
    {
		user:		23094392049024
	}
```

Devolverá un `200 Ok` junto con lista de usuarios que siguen al usuario indicado:


```json
    [{
		id:		    120949303434,
		username: 	"soyjavi",
		name:   	"Javi",
		avatar:		"AVATAR_URL"
	},
	{
		id:	    	120949303433,
		username: 	"cataflu",
		name:   	"Catalina",
		avatar:		"AVATAR_URL"
	},
	{
		id: 		120949303431,
		username: 	"haas85",
		name:   	"Iñigo",
		avatar:		"AVATAR_URL"
	}
	]
```


## GET /stats
-------------
Si quieres tener una visión general de un determinado usuario dentro de la red social de tu aplicación utiliza este recurso, que al igual que en los recursos anteriores solo necesita del id del usuario como parámetro:

```json
    {
		user:		23094392049024
	}
```

Devolverá un `200 Ok` junto con los totales de *followers* y *followings* que tiene el usuario indicado:

```json
    {
		following:	123,
		followers: 	343
	}
```


## GET /check
-------------
Este recurso sirve para saber la relación que tienes con un determinado usuario (tal vez te interese seguirlo o no) para ello tenemos que enviar el id del usuario a consultar de la siguiente manera:

```json
    {
		user:		23094392049024
	}
```

En el caso de que todo haya ido correctamente devolverá un `200 Ok` junto con el objeto que representa la relación que tiene el usuario consultado con el usuario de tu aplicación (TOKEN):
 
```json
    {
		following:	true,
		follower: 	false
	}
```