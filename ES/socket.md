SOCKET
======
App/nima te permite trabajar con sockets donde puedes tener con salas de conversaciones. Para ello ten en cuenta que todas las peticiones que hagas tendrán que ir a:


	http://socket.appnima.com/{RECURSO}
	
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

## Salas
--------
Para la comunicación en tiempo real, appnima utiliza socket.io para conectarse a las diferentes salas creadas. A continuación se exponen los siguientes métodos de interacción con ellas. Aunque recomendamos el uso de nuestra librería `Appnima.js` para esta labor.

## open
-------
Para crear una sala se llamará al método `open`. Esté recibirá los siguientes parametros:

* Token del usuario
* Contexto (que se usará para identificar la sala para futuras conexiones)
* Nombre de sala
* Tipo de sala
* Si queremos que la información de la sala sea persistente
* Array de los ids de los usuarios permitidos

Si todo está correcto la sala se creará y automáticamente el autor estará conectado a ella.

## join
-------
Un usuario puede conectarse a una sala siempre y cuando ya esté creada y tenga permiso de conexión a la misma. Para ello se llamará al método `join` y se le pasarán los siguientes parámetros:

* Token del usuario (obligatorio a no ser que sea sesión anónima)
* Contexto (identificador de la sala a la que se quiere conectar)
* Id de aplicación (en caso de sesión anónima se requiere de este campo)

## leave
--------
En caso de querer desconectarse de una sala se llamará al método `leave`.

## sendmessage
--------------
Para enviar un mensaje a una sala basta con llamar al método `sendMessage` y pasarle como parámetro un objeto (el mensaje puede ser cualquier tipo de dato). Este mensaje llegará a todos los usuarios conectados a la sala, emisor incluido, se recibirá a través del listener `onMessage` y tendrá el siguiente formato:

```json
    {
		user:		    {"usuario que envía el mensaje"},
		message:   	    {"mensaje enviado"},
		created_at:		"2013-05-23T12:01:02.736Z"
	}
```

## sendbroadcast
----------------
Este método funciona igual que el método `sendMessage` con la única diferencia de que el emisor no recibirá el mensaje enviado.

## allowusers
-------------
Se pueden añadir usuarios a la lista de admitidos a través del método `allowUsers`, para ello bastará con llamar a este método y pasarle un array con los ids de los usuarios a admitir.

## disallowusers
-------------
Se pueden eliminar usuarios de la lista de admitidos a través del método `disallowUsers`, para ello bastará con llamar a este método y pasarle un array con los ids de los usuarios a eliminar de la lista de admitidos.

## Tipos de salas y permisos
----------------------------
Existen distintos tipos de salas, cada una tiene unos permisos distintos dependiendo del usuario.

* Privadas: Solo se pueden conectar, leer y escribir usuarios que se encuentren en la lista de admitidos.
* Públicas: Todo el mundo se puede conectar y leer, pero solo el dueño puede publicar en ella.
* Inbox: Solo el dueño puede leer lo que se escribe, pero cualquier usuario puede ecribir.
* Aplicación: Esta sala existe en todas las aplicaciones, por lo que nadie tiene que crearla, todos pueden conectarse, leer y escribir en ella.