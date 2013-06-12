PUSH
====
Use this module to send push notifications to users devices. To do so, all request have to go to:

    http://appnima-user.eu01.aws.af.cm/{RESOURCE}

Remember all requests to App/nima should be identified by your `Appnima.key` or key pair `client` and `secret`.

So, the first parameter is the type of request (GET, POST, UPDATE, DELETE …) and the second the name of resource.


## POST /push
-------------
This resource allows you to send push notifications to the user device. Sends the request and the following parameters:

```json
    {
		user:		23094392049024,
		alert:      ""Texto a mostrar en la notificación",
		content":   {"title": "JSON con los campos necesarios", "text": "Hola App/nima!"}
	}
```


If the notification was successful App/nima returns `200 Ok`.