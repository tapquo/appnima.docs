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

## Rooms
--------
To do real time communication, appnima connects to the rooms that have been created using socket.io. Now you will see the methods that are exposed to interact with them. Although we recommend using our library `Appnima.js` for this purpose.


## open
-------
To create a room we will call to the `open` method. This one will receive the following parameters:

* User's token
* Context (it will be used to identify the room for future connections)
* Room name
* Room Type
* If we want that the room persist in time
* Allowed users' id array

If everythng is right the room will be created and the author will be automatically connected to it.

## join
-------
A user can be connected to a room if it is created and he has permission to connect to it. To do this `join` method must be called with the following parameters:

* User's token (for anonymous session not required)
* Context (identification of the room to connect)
* Application id (in case of anonymous session it is mandatory)

## leave
--------
To disconnect from a room just call the method `leave`.

## sendmessage
--------------
To send a message to a room just call the method `sendMessage` with an object as parameter (a message can be any type of data). This message will be received by all the room's users, also the sender, through a listener called `onMessage` and it will have the following format:

```json
    {
		user:		    {"user that sends the message"},
		message:   	    {"message sent"},
		created_at:		"2013-05-23T12:01:02.736Z"
	}
```

## sendbroadcast
----------------
This methods works as `sendMessage`, the unique difference is that the sender won't receive the message.

## allowusers
-------------
Users can be added to the allowed user list using `allowUsers` method, to do this just call this method and use users' id array as parameter.

## disallowusers
-------------
Users can be removed from the allowed user list using `disallowUsers` method, to do this just call this method and use users' id array as parameter.

## Room types and permissions
-----------------------------
There are several room types, each one has different permissions depending the user.

* Private: Only users in the allowed list can connect, send and receive messages in this room.
* Public: All the people can connect to this room, but only the owner can post.
* Inbox: Only the owner can read messages, but any user can send messages.
* Application: This room exists in all the applications so no one can create it, everyone can connect, receive or send messages in it.