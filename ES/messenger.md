MESSENGER
=========

Este módulo recoge toda la funcionalidad de mensajería: enviar e-mail, SMS y mensajes privados entre usuarios de tu misma aplicación.

Para ello ten en cuenta que todas las peticiones que hagas tendrán que ir a:

	http://api.appnima.com/messenger/{RECURSO}

Recuerda que todas las peticiones que hagas a App/nima tienen que ir identificadas con tu `Appnima.key` o bien con el par de datos `client` y `secret`. Ahora veamos los recursos que puedes utilizar, para ello el primer parámetro indica el tipo de petición (GET, POST, UPDATE, DELETE …) y el segundo parámetro el nombre del recurso.


## POST /mail
-------------
Con este recurso los usuarios de tu aplicación podrán enviar e-mails. Para ello debes pasar los siguientes parámentros (el campo subject es opcional):


```json
    {
		user:		23094392049024,
		subject:	"Appnima.com",
		message:	"Welcome to appnima.messenger [MAIL]"
	}
```

Si todo ha salido bien, devolverá un `201 Created` junto con el objeto:

```json
    {
		message:	'E-mail sent successfully.'
	}
```

## POST /sms
------------
Con este recurso tu aplicación podrá enviar mensajes de texto a los dispositivos móviles registrados de tus usuarios. Tan solo debes enviar junto con la petición los parámetros:

```json
    {
		user:		23094392049024,
		message:	"Welcome to appnima.messenger [SMS]"
	}
```

En el caso de que la respuesta haya sido satisfactoria se devolverá un `201 Created` junto con el objeto:

```json
    {
		message:	'SMS sent successfully.'
	}
```

## POST /message
----------------
Si lo necesitas, Appnima te provee de un sistema de mensajería interno entre los usuarios de tu aplicación. Los parámetros que necesita la petición son:

```json
    {
		user:		23094392049024,
		subject:	"Appnima.com",
		message:	"Welcome to appnima.messenger [MESSAGE]"
	}
```

El campo subject es opcional y si la petición ha salido bien se devolverá un `201 Created` junto con el objeto:

```json
    {
		message:	'Message sent successfully.'
	}
```


## GET /message/outbox
---------------
context: outbox

Los usuarios de tu aplicación pueden recuperar los mensajes enviados. Para ello basta con llamar al recurso pasando como parámetro outbox.

```json
    {
		context:	outbox
	}
```

Si todo ha salido bien, devolverá un `200 Ok` junto con lista de mensajes de la bandeja indicada:

```json
    {
		_id:			120949303434,
		from:	 		120949303434,
		to:				120949303433,
		application		220949303432
		subject			"Appnima.com",
		body			"Welcome to appnima.messenger [MESSAGE]",
		state			SENT
	}
```

## GET /message/inbox
---------------
context: inbox

Los usuarios de tu aplicación pueden recuperar los mensajes recibidos. Para ello basta con llamar al recurso pasando como parámetro inbox.

```json
    {
		context:	inbox
	}
```

Si todo ha salido bien, devolverá un `200 Ok` junto con lista de mensajes de la bandeja indicada:

```json
    {
		_id:			120949303434,
		from:	 		120949303434,
		to:				120949303433,
		application		220949303432
		subject			"Appnima.com",
		body			"Welcome to appnima.messenger [MESSAGE]",
		state			READ
	}
```

## PUT /message
---------------
state: READ || DELETED

Para que se refleje que el usuario ha leído un mensaje o que desea eliminarlo de su sistema, basta con enviar en la petición los siguientes parámetros:

```json
    {
		message:	23094392049024,
		state:		"READ"
	}
```

Si todo ha salido bien, devolverá un `200 Ok`junto con el mensaje de confirmación:

```json
    {
		message:			"Resource READ."
	}
```