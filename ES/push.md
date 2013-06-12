PUSH
====

Con este módulo puedes enviar notificaciones a los terminales registrados de los usuarios de tu aplicación. 

	http://appnima-push.eu01.aws.af.cm/{RECURSO}
	
Recuerda que todas las peticiones que hagas a App/nima tienen que ir identificadas con tu `Appnima.key` o bien con el par de datos `client` y `secret`. Ahora veamos los recursos que puedes utilizar, para ello el primer parametro indica el tipo de petición (GET, POST, UPDATE, DELETE …) y el segundo parametro el nombre del recurso.


## POST /push
-------------
Envía notificaciones push mediante este recurso. Junto con la petición, envía los siguientes parámetros:

```json
    {
		user:		23094392049024,
		alert:      ""Texto a mostrar en la notificación",
		content":   {"title": "JSON con los campos necesarios", "text": "Hola App/nima!"}
	}
```
En caso de éxito se devolverá el código `200 Ok`.