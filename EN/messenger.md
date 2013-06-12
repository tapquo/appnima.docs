MESSENGER
=========
This module provide you all resources to send and receive messages like e-mails, SMS and private messaging between users on your application. To do so, all request have to go to:

	http://api.appnima.com/messenger/{RESOURCE}

Remember all requests to App/nima should be identified by your `Appnima.key` or key pair `client` and `secret`.

So, the first parameter is the type of request (GET, POST, UPDATE, DELETE …) and the second the name of resource.

## POST /mail
-------------
With this resource your users can send e-mail. You just need sends the request with the next parameters (subject is optional):

```json
    {
		user:		23094392049024,
		subject:	"Appnima.com",
		message:	"Welcome to appnima.messenger [MAIL]"
	}
```

Responses are returned with `201 Created` and the object:


```json
    {
		message:	'E-mail sent successfully.'
	}
```


## POST /sms
------------
Also, App/nima provides SMS messaging. Your application can send messages to users who have phone number registered into the platform. Sends the request and the following parameters:

```json
    {
		user:		23094392049024,
		message:	"Welcome to appnima.messenger [SMS]"
	}
```

Responses are returned with `201 Created` and the object:


```json
    {
		message:	'SMS sent successfully.'
	}
```


## POST /message
----------------
If you need, App/nima gives private messaging between users on your application. Sends the request with the next parameters:

```json
    {
		user:		23094392049024,
		subject:	"Appnima.com",
		message:	"Welcome to appnima.messenger [MESSAGE]"
	}
```

The field subject is optional and App/nima returns `201 Created` and the object:

```json
    {
		message:	'Message sent successfully.'
	}
```

## GET /message/outbox
---------------
context: outbox

Users of your application can retrieves messages sent. Just use this resource with the next parameter:

```json
    {
		context:	outbox
	}
```

App/nima returns `200 Ok` and a list of messages:

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
As **GET /message/outbox** shown to your users a list received messages. Just change de context:

```json
    {
		context:	inbox
	}
```

And App/nima returns `200 Ok` and a list of messages:

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

Modify the state of a message using this resource. Sends the request and the following parameters:

```json
    {
		message:	23094392049024,
		state:		"READ"
	}
```

Responses are returned with `200 Ok` and the object:

```json
    {
		message:			"Resource READ."
	}
```









