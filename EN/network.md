NETWORK
=======
Use this module to create a social network into your application: find friends, follow people, unfollow, get list of friends… To do so, all request have to go to:

	http://api.appnima.com/network/{RESOURCE}

Remember all requests to App/nima should be identified by your `Appnima.key` or key pair `client` and `secret`.

So, the first parameter is the type of request (GET, POST, UPDATE, DELETE …) and the second the name of resource.

## GET /search
--------------
Search people into App/nima using this resource. You just need one parameter:

```json
    {
		query:		"javi@tapquo.com"
	}
```

Responses are returned with `200 Ok` and a list of users that match the search: 


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
To follow a user you can use this resource with the ID user:


```json
    {
		user:		23094392049024
	}
```

Responses are returned with `200 Ok` and the object:

```json
    {
		status:		'ok'
	}
```

## POST /unfollow
-----------------
As **POST /follow** to unfollow a person you just need send with the request the ID user:


```json
    {
		user:		23094392049024
	}
```

Responses are returned with `200 Ok` and the object:

```json
    {
		status:		'ok'
	}
```

## GET /following
-----------------
Retrieves the list of followings of a user. Sends the request and ID user:

```json
    {
		user:		23094392049024
	}
```

App/nima returns `200 Ok` and the list:

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
	}
	]
```

## GET /followers
-----------------
As **GET /following** you can retrieve a list of followers of a user.


Retrieves the list of followings of a user. Sends the request and ID user:

```json
    {
		user:		23094392049024
	}
```

App/nima returns `200 Ok` and the list:

```json
    [{
		id:		    120949303434,
		username: 	"soyjavi",
		name:   	"Javi",
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
Get an overview about users network. Use this resource and ID user:

```json
    {
		user:		23094392049024
	}
```

App/nima returns `200 Ok` and user total *followers* and *followings*:

```json
    {
		following:	123,
		followers: 	343
	}
```


## GET /check
-------------
If you need to know about relationship with another user, use this resource and his ID user:

```json
    {
		user:		23094392049024
	}
```

If the query was successful App/nima returns `200 Ok` and the object which describes the relationship between both users:

```json
    {
		following:	true,
		follower: 	false
	}
```

