SOCKET
======
App/nima te permite trabajar con sockets donde puedes trabajar con salas de conversaciones. Para ello ten en cuenta que todas las peticiones que hagas tendrán que ir a:


	http://appnima-user.eu01.aws.af.cm/network/{RECURSO}
	
Recuerda que todas las peticiones que hagas a App/nima tienen que ir identificadas con tu `Appnima.key` o bien con el par de datos `client` y `secret`. Ahora veamos los recursos que puedes utilizar, para ello el primer parametro indica el tipo de petición (GET, POST, UPDATE, DELETE …) y el segundo parametro el nombre del recurso.


## GET /rooms
-------------
Un usuario puede obtener el listado de salas de sockets en las que participa, para ello bastaría con llamar al recurso y se obtendría un mensaje `200 Ok` junto con un listado como el siguiente:

```json
    [{
		id:		        "ba54",
		name:   	    "amigos",
		created_at:		"2013-05-23T12:01:02.736Z"
	},
	{
		id:	    	    "asf9a76y2t3ub",
		name:   	    "appnima friends",
		created_at:		"2013-02-23T12:01:02.736Z"
	}
	]
```