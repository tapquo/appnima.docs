SOCKET
======
App/nima provides sockets environments to get chat rooms. To do so, all request have to go to:

    http://appnima-user.eu01.aws.af.cm/{RESOURCE}

Remember all requests to App/nima should be identified by your `Appnima.key` or key pair `client` and `secret`.

So, the first parameter is the type of request (GET, POST, UPDATE, DELETE …) and the second the name of resource.

## GET /rooms
-------------
Get a list of rooms where a user is participant. You just need call the resource and if the query was successful App/nima returns `200 Ok` and the list like this:

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