App/nima
========
Find out how to add soul to your apps with first LaaS in the world.

*Current version: [1.0.0]()*

Introduction
------------
Just for reading this document you are telling that you are a developer who wants to improve his skills continously and wants to create more efficient projects. App/nima is the first platform which offers logic services for any type of project, no matter you want to create an application or a site, App/nima will help you in both situations.

A bit of history, 4 years ago in [**Tapquo**](http://tapquo.com) we found a common problem in the development area, it was sthat each new product we create we had to repeat every time the same basic functionalities. App/nima borns from the need of being more efficients and the desire to develop only the business features of our new product and no more the horizontal l

+ [OAuth 2 services for authentication](#oauth2)
+ [User management](#user)
+ [User social network](#network)
+ [Messaging service to send emails, SMS, calls and private messages](#message)
+ [Find/geolocate places](#places)
+ [Find/geolocate other users](#places)
+ [Real-time communications via sockets](#sockets)
+ [Push notifications for ioS, Android and Blackberry](#push)
+ Get our users' behaviour

If sometime you have had to develop some of this functionalities in any project, you are welcome to our platform because App/nima is thougth to help you as a developer. We want to give you a platform made to ease your life, we love software as much as you and we seek being better. Use App/nima.


### Arquitecture
Probably now you have doubts of how App/nima can help you in being more efficient, but calm down we are going to explain you how it works. App/nima is completely based in the authentication protocol [**Oauth 2**](http://oauth.net/2/) and in the serialization via REST-style with JSON objects. If you have never used this paradigm don't worry, we are explaining step by step how to connect to the platform and will offer you explicit tools such as [**AppnimaJS**](http://) that will ease you the connection process.

All the backend is deployed around the world using platforms such as Amazon or Google Cloud Engine, the ones which give us the ability to give allways the best response quality for our users. Don't worry, you won't have to be a *Jedy SysAdmin* because you will only have to communicate with our REST. The scalability is our duty, you will only have to worry about creating the best project and us in providing every day the best platform.

In summary, App/nima is a platform in a REST API way that provide you with the logical services of your projects, and depending the nature of your project maybe you won't need your own backend. Each logical service is hosted in different servers to obtain the biggest independent scalability, along the documentation we will watch the routes of each service.

### [OAuth 2](id:oauth2)
All App/nima services use the authentication protocol [**OAuth 2**](http://oauth.net/2/) looking for the largest compatibility with third party tools. If you are not an expert in Oauth 2 we recommend you to study it, because it is beeing the authentication protocol by default and companies like Google, Facebook or Twitter use it to connect with their APIs. Read the [documentation](http://) to start right now working with App/nima.

### No XML, just JSON
We only allow JSON type data serialization. Our format is not to have any root element and use *snake_case* to describe atributes' keys. This means that each petition to App/nimamust be send the sufix:

    Content-Type: application / json; charset = utf-8

Yo will receive a response 415 *Unsupported Media Type* if you try to use another URL sufix.


### Use HTTP cache
You have to use the *freshness* HTTP headers to reduce our servers' load (and speed up your application!). Most of the petitions we return will include an *ETag* or a header *Last-Modified*. The first time you request a resource, store this value and send it to us in the next petitions as *If-None-Match* and *If-Modified-Since*. If the resource hasn't change you will obtain the code 304 *Not Modified* as response, saving time and broadband (because you already have the resource)


### Handling errors
If App/nima has any problem, it is possible that you get an 5xx error. 500 means that the application is totally down, but you can also get a 502 *Bad Gateway*, 503 *Service Unavailable*, or 504 *Gateway Timeout*. In all these situations it is your responsibility to try again later.



Clients
-------


Help us
-------
Please, don't have any doubt in contacting us if you think you can do a better API. If you think that we have to support a new functionalit or if you have found a bug, use GitHub issues. Make a fork of this documentation and send us your *pulls* with the improvements.

To talk with us or with other developers about the API, suscribe to our [**mailing list**](https://groups.google.com/forum/#!forum/appnima).




API REST
========
Authentication
--------------
With this resource App/nima get user token through the OAuth system called 2-step. The resource path is:

    http://appnima.com/{RESOURCE}

The following explains how to get token using this method.


#### Step 1: GET /oauth/authorize
User will be redirected to another URL. This resource is called with the following parameters:
``` json
	"response_type" : "type of request. Should be code.",
	"client_id"     : "your public ID application.",
	"scope"         : "token permissions",
	"redirect_uri"  : "URL to redirect. It will be the same as you provide when you registered your application.",
	"state"         : "state parameter with response to identify the operation."
```

The URL will be like this:

    http://api.appnima.com/oauth2/authorize?scope=profile,push&response_type=code&client_id=519b84f0c1881dc1b3000002&redirect_uri=http://myapp.com&state=user48

The user will see a site where will be shown data application and permissions required. If rejected or something it is wrong with query, user will be redirect to site specifying the error.

If the request was successful, user will be redirect to a site with a field `code` that allows to follow the process.


#### Step 2: POST /oauth2/token
After get `code`, token will be request. To do so, this resource will be called with "http basic authorization"  header with your appnima key and the parameters:
```header
    {
        Authorization: "basic APPNIMA-KEY"
    }
```


```json
    {
        "grant_type" : "code",
        "code"       : "NUMBER_CODE",
        "client_id"  : "CLIENT_ID"
    }
```

If the query was successful returns `201 Created` and the object:
```json
    {
        "token_type"    : "bearer",
        "refresh_token" : "REFRESH_TOKEN",
        "access_token"  : "ACCESS_TOKEN"
    }
```

#### Refreshing Token: POST /oauth2/token
When you try to access to a server's resource but it returns an error code `480` indicates that the token is expired, to refresh the `access_token` and `refresh_token` values use this resource using the "http authorization basic" header with your appnima key and the parameters:
```header
    {
        Authorization: "basic APPNIMA-KEY"
    }
```

```json
    {
        "grant_type"    : "refresh_token",
        "refresh_token" : "REFRESH_TOKEN"
    }
```

Response returns the following object:

```json
    {
        "access_token"  : "ACCESS_TOKEN",
        "expires_in"    : "2013-08-06T06:58:37.298Z",
        "refresh_token" : "REFRESH_TOKEN",
        "scope"         : [ "profile" ]
    }
```

[User](id:user)
----
Use this resource to add users of your project within App/nima platform. To do so, all request have to go to:

    http://api.appnima.com/user/{{resource}}

Remember all requests to App/nima should be identified by your `Appnima.key` or key pair `client` and `secret`. So, the first parameter is the type of request (GET, POST, UPDATE, DELETE …) and the second the name of resource.

*Example*

    [GET] http://api.appnima.com/user/info


### Security
#### POST /signup
All users of your application must be App/nima users. So the first step is register the user to get his token. Sends the request with the next parameters:
```json
    {
        "mail"     : "USER_MAIL",
        "password" : "USER_PASSWORD"
    }
```

You can send additional fields like:
```json
    {
        ...
        "username" : "USER_NICKNAME",
        "name"     : "Javi Jimenez",
        "avatar"   : "http://USER_AVATAR_URL"
    }
```
Responses are returned with `201 Created` and the object:
```json
    {
        "id"       : "USER_ID",
        "mail"     : "USER_MAIL",
        "username" : "USER_NICKNAME",
        "name"     : "Javi Jimenez",
        "avatar"   : "http://USER_AVATAR_URL"
    }
```


#### POST /oauth2/token
The second step is get the token Oauth2 authentication. It will be the key for all requests into App/nima. So, send the following parameters within the header "http basic authorization"(client_id:client_secret Base64 encoded):
```json
	"Authorization" : "basic",
	"client_id"     : "CLIENT_ID"
```

```json
    {
        "grant_type" : "password",
        "mail"       : "USER_MAIL",
        "password"   : "USER_PASSWORD"
    }
```

Responses are returned with `201 Created` and the object:
```json
    {
        "token_type"    : "bearer",
        "refresh_token" : "REFRESH_TOKEN",
        "access_token"  : "ACCESS_TOKEN"
    }
```


#### POST /login
Use this resource to find out user permissions into your application. Sends the request with the next parameters:
```json
    {
        "mail"     : "USER_MAIL",
        "password" : "USER_PASSWORD"
    }
```

If the validation was successful App/nima returns `200 Ok` and the user data.


### Info
#### GET /info
If you need to get data from the user, you should use this resource and how you're using the OAuth authentication protocol 2 is not required to send any der parameter unless you want to get the information of any user of App/nima.

In that case you just have to send the id of the user with the method call.
Get user data with this resource. Then, just wait the response `200 Ok` and APP/NIMA will returns:
```json
    {
        "id"       : "USER_ID"
        "mail"     : "USER_MAIL",
        "username" : "USER_NICKNAME",
        "name"     : "Javi Jimenez",
        "avatar"   : "http://USER_AVATAR_URL",
        "language" : "spanish",
        "country"  : "ES",
        "bio"      : "Founder & CTO at @tapquo",
        "phone"    : "USER_PHONE",
        "site"     : "http://USER_URL"
    }
```

#### PUT /update

This resource is used to modify the personal data of a user within your application, as in the resource **GET/user/info** is not necessary to identify the user as parameter. You can send all the parameters below (but are not required to send them all):
```json
    {
        "mail"     : "USER_MAIL",
        "username" : "USER_NICKNAME",
        "name"     : "Javi Jimenez",
        "avatar"   : "http://USER_AVATAR_URL",
        "picture"  : "http://USER_PICTURE_URL",
        "bio"      : "Founder & CTO at @tapquo",
        "phone"    : "USER_PHONE"
    }
```
If the request was successful App/nima returns `200 Ok` and the same object **GET /user**. If the user has not permission to modify his data App/nima returns `403 Forbidden`.


#### POST /password:
Change the user password sending the old one and the new one:
```json
    {
        "old_password" : "OLD_PASSWORD",
        "new_password" : "NEW_PASSWORD"
    }
```

#### POST /avatar
Upload user avatar with this resource. Sends the request and the following parameters:
```json
    {
        "avatar": "USER_AVATAR"
    }
```

**avatar**: Avatar data coded in base64.

Responses are returned with `201 RESOURCE CREATED`.

### Likes
#### POST /like/post
This resource is used to do favorite particular post or to remove a favorite already done. To do this, you just have to send the *id* of that post.

```json
    {
        "post": "POST_ID"
    }
```
If this is the first time you mark as favorite, APP NIMA return the `200 OK` response. But if you had liked before, delete that favorite and APP/NIMA return a message: *unliked*.

#### GET /like/post
This resource, if all goes well, return the list of the *posts* of the user has logged liked. To do this, you do not send anything.
Here there is also the possibility of using *pagination* to list post as explained in the resource **TIMELINE** in ```MESSENGER```scope.

#### GET /post/likers
This resource used to get all users who have liked an specific *post*. To do this, you only need to send the *id* of that post.

```json
    {
        "post": "POST_ID"    }
```

### Comment
#### POST/ comment
This resource is used to create comments on a `post`. The idea of ​​this resourse is that you can create discussions on the post. To make a comment you must send the following parameters:

```json
{
	"text"    : "Lorem ipsum",
	"post"    : "POST_ID",
	"message" : "MESSAGE_ID"
}
```

#### GET /post/comment
This resource is used to get all the comments from a post. You just have to send the id of the *post* so that it will return the list of comments.

### Terminal
#### POST /terminal
This resource allows you register the user device. Sends the request and the following parameters:
```json
{
	"type"    : "phone",
	"os"      : "ios",
	"version" : "6.0"
}
```

**type**: computer, tablet, phone, tv
**os**: windows, macos, linux, ios, android, blackberry, firefoxos, windowsphone, other

Responses are returned with `201 RESOURCE CREATED`.


#### GET /terminal
Retrieves information about what devices accessed to your application. Like **GET /user/info** you do not need any parameter. App/nima returns `200 Ok` and the following object:
```json
[
	{
    	"_id"     : "USER_ID",
       	"type"    : "phone",
       	"os"      : "ios",
       	"token"   : "USER_TOKEN",
       	"version" : "6.0"
    },
    {
        "_id"     : "USER_ID",
        "type"    : "desktop",
        "os"      : "macos",
        "token"   : "USER_TOKEN",
        "version" : "10.8"
    }
]
```

#### PUT /terminal
Updadtes the device information with this resource. Just send:
```json
{
	"terminal" : "TERMINAL_ID",
    "type"     : "phone",
    "os"       : "ios",
    "version"  : "6.0"
}
```
**type**: computer, tablet, phone, tv
**os**: windows, macos, linux, ios, android, blackberry, firefoxos, windowsphone, other

You need use this resource when a user wishes to unsubscribe from your application.


### Subscriptions
#### POST /subscription
Registered e-mail of users who want to receive information from your application or who want be invited to your application. You just need send like parameter the e-mail address:

```json
{
	"mail": "USER_MAIL"
}
```

Responses are returned with `200 Ok` and the object:

```json
{
	"message": "Request accepted."
}
```


### Support
#### POST /ticket
Utiliza este recurso como sistema de gestión de tickets para la resolución de las consultas e incidencias de tus usuarios. Envía como parámetro junto a la petición el texto de la consulta:
```json
{
	"question": "[SUGGESTION] Bigger buttons."
}
```


[Network](id:network)
-------
Use this resource to create a social network into your application: find friends, follow people, unfollow, get list of friends… To do so, all request have to go to:

    http://api.appnima.com/network/{RESOURCE}

Remember all requests to App/nima should be identified by your `Appnima.key` or key pair `client` and `secret`.

So, the first parameter is the type of request (GET, POST, UPDATE, DELETE …) and the second the name of resource.



### Relationships
#### GET /search
Search people into App/nima using this resource. You just need one parameter:
```json
{
	"query": "USER_MAIL"
}
```

Responses are returned with `200 Ok` and a list of users that match the search:
```json
[
	{
		"id"        : "USER_ID",
      	"username"  : "USER_NICKNAME",
       	"name"      : "Javi",
       	"avatar"    : "AVATAR_URL",
       	"follower"  : "true",
       	"following" : "false"
    },
    {
       	"id"        : "USER_ID",
       	"username"  : "USER_NICKNAME",
       	"name"      : "Catalina",
       	"avatar"    : "AVATAR_URL",
       	"follower"  : "true",
       	"following" : "true"
    },
    {
       	"id"        : "USER_ID",
       	"username"  : "USER_NICKNAME",
       	"name"      : "Iñigo",
       	"avatar"    : "AVATAR_URL",
       	"follower"  : "false",
    	"following" : "false"
    }
    ]
```
The variable *following* indicate that user is loggued is follow or not that user, and the variable *follower* indicate that user follow or not loggued user.

#### POST /follow
To follow a user you can use this resource with the ID user:
```json
    {
        "user": "USER_ID"
    }
```

Responses are returned with `200 Ok` and the object:
```json
    {
        "status": "ok"
    }
```

#### POST /unfollow
As **POST /follow** to unfollow a person you just need send with the request the ID user:
```json
    {
        "user": "USER_ID"
    }
```

Responses are returned with `200 Ok` and the object:

```json
    {
        "status": "ok"
    }
```

#### GET /following
Retrieves the list of followings of a user. Sends the request and ID user:
```json
    {
        "user": "USER_ID"
    }
```

App/nima returns `200 Ok` and the list:
```json
[
	{
       	"id"       : "USER_ID",
       	"username" : "USER_NICKNAME",
       	"name"     : "Javi",
       	"avatar"   : "AVATAR_URL"
    },
    {
       	"id"       : "USER_ID",
       	"username" : "USER_NICKNAME",
       	"name"     : "Catalina",
       	"avatar"   : "AVATAR_URL"
    }
]
```

Like in *timeline* of resource ```MESSENGER```, there is the option of getting this data using pagination. To do this, you have to add the following parameters to your call:


```json
{
	"user"        : "USER_ID",
	"page"        : "0",
	"num_results" : "5"
}
```


The meaning of the variable *page* and *num_results* is the same as in the case of the call to **Timeline**. The only difference between the two calls is that in this case dont need to send the variable of the last date.

#### GET /followers
As **GET /following** you can retrieve a list of followers of a user.


Retrieves the list of followings of a user. Sends the request and ID user:
```json
{
	"user": "USER_ID"
}
```

App/nima returns `200 Ok` and the list:
```json
[
	{
		"id"        : "USER_ID",
		"username"  : "USER_NICKNAME",
		"name"      : "Javi",
		"avatar"    : "AVATAR_URL"
		"is_follow" : "true"
    },
    {
		"id"        : "USER_ID",
		"username"  : "USER_NICKNAME",
		"name"      : "Iñigo",
		"avatar"    : "AVATAR_URL",
		"is_follow" : "false"
    }
]
```

Like has been explained in **GET /followings**, there is also the option of getting the data using pagination. The dynamic is the same.

As can be seen, in this case, the call returns one more variable in each object. This variable indicate that user is loggued is follow or not that user.

### Statistics
#### GET /info
Get an overview about users network. Use this resource and ID user:
```json
{
	"user": "USER_ID"
}
```

App/nima returns `200 Ok` and user total *followers* and *followings* and list of both:
```json
following:
	"users":
		[
			{
				"name"     : "Javi",
				"username" : "USER_NICKNAME",
				"bio"      : "USER_BIO",
				"mail"     : "USER_MAIL"
			}
		]
	"count": "1",
	"followers": {
		"users": [],
		"count": "0"
	}
```


#### GET /check
If you need to know about relationship with another user, use this resource and his ID user:
```json
{
	"user": "USER_ID"
}
```

If the query was successful App/nima returns `200 Ok` and the object which describes the relationship between both users:
```json
{
	"following" : "true",
	"follower"  : "false"
}
```


[Messenger](id:message)
---------
This resource provide you all resources to send and receive messages like e-mails, SMS and private messaging between users on your application. To do so, all request have to go to:

    http://api.appnima.com/messenger/{RESOURCE}

Remember all requests to App/nima should be identified by your `Appnima.key` or key pair `client` and `secret`.

So, the first parameter is the type of request (GET, POST, UPDATE, DELETE …) and the second the name of resource.


### Mail
#### POST /mail
With this resource your users can send e-mail. You just need sends the request with the next parameters (subject is optional):
```json
{
	"user"    : "USER_ID",
	"subject" : "Appnima.com",
	"message" : "Welcome to appnima.messenger [MAIL]"
}
```

Responses are returned with `201 Created` and the object:
```json
{
	"message": "E-mail sent successfully."
}
```

### SMS
#### POST /sms
Also, App/nima provides SMS messaging. Your application can send messages to users who have phone number registered into the platform. Sends the request and the following parameters:
```json
{
	"user"    : "USER_ID",
	"message" : "Welcome to appnima.messenger [SMS]"
}
```

Responses are returned with `201 Created` and the object:
```json
{
	"message": "SMS sent successfully."
}
```


### Message
#### POST /message
If you need, App/nima gives private messaging between users on your application. Sends the request with the next parameters:
```json
{
	"user"    : "USER_ID",
	"subject" : "Appnima.com",
	"body"    : "Welcome to appnima.messenger [MESSAGE]"
}
```

The field subject is optional and App/nima returns `201 Created` and the object:
```json
{
	"message": "Message sent successfully."
}
```

#### GET /message/outbox
Users of your application can retrieves messages sent. Just use this resource with the next parameter:
```json
{
	"context": "outbox"
}
```

App/nima returns `200 Ok` and a list of messages:
```json
{
	"_id"         : "MESSAGE_ID",
	"from"        : "USER_FROM_ID",
	"to"          : "USER_TO_ID",
	"application" : "APPLICATION_ID",
	"subject"     : "Appnima.com",
	"body"        : "Welcome to appnima.messenger [MESSAGE]",
	"state"       : "SENT"
}
```


#### GET /message/inbox
As **GET /message/outbox** shown to your users a list received messages. Just change de context:
```json
{
	"context": "inbox"
}
```

And App/nima returns `200 Ok` and a list of messages:
```json
{
	"_id"         : "MESSAGE_ID",
	"from"        : "USER_FROM_ID",
	"to"          : "USER_TO_ID",
	"application" : "APPLICATION_ID",
	"subject"     : "Appnima.com",
	"body"        : "Welcome to appnima.messenger [MESSAGE]",
	"state"       : "READ"
}
```

#### PUT /message
Modify the state of a message using this resource. Sends the request and the following parameters:
```json
{
	"message" : "MESSAGE_ID",
	"state"   : "READ"
}
```
**read**: READ or DELETED

Responses are returned with `200 Ok` and the object:
```json
{
	"message": "Resource READ."
}
```

### Post
#### POST/post
A post is a public message and the user can create with this resource. Only have to send this parameters with the request:

```json
{
	"title"   : "Lorem Ipsum",
	"content" : "Lorem ipsum dolor sit amet, consectetur adipisicing elit.",
	"image"   : "http://IMAGE_URL
}
```
The only required field when creating a post is ```content``` that it is the content of the message.

If all goes well you only have to wait for the answer `200 ok` and APP/NIMA will returns the following parameters:
```json
{
	"id"          : "POST_ID",
	"application" : "APPLICATION_ID",
	"content"     : "Lorem ipsum dolor sit amet, consectetur adipisicing elit.",
	"title"       : "Lorem ipsum",
	"create_at"   : "2013-12-02 08:00:58.784Z",
	"image"       : "http://IMAGE_URL",
	"owner"       :
		{
			"_id"        : "OWNER_ID",
			"avatar"     : "http://AVATAR_URL",
			"created_at" : "2013-12-02 08:00:58.784Z",
			"mail"       : "OWNER_MAIL",
			"name"       : "Javi",
			"username"   : "OWNER_NICKNAME"
		}
}
```

#### POST /put
This resource is used to modify a previously created post. To do this, the user must send parameters with the request:

```json
{
	"id"      : "POST_ID",
	"title"   : "Lorem Ipsum",
	"content" : "Lorem ipsum dolor sit amet, consectetur adipisicing elit.",
	"image"   : "http://IMAGE_URL
}
```

If all goes well you only have to wait for the answer `200 ok` and APP/NIMA returns the same parameters as in the `POST`.

#### GET /post
This resource is used to obtain a specific post. To do this, the user must only send `id` of the post you want to get, and if all goes well, APP/NIMA return the `200 OK` and the concrete post with same style as in `POST `.

#### GET /user/post
This resource is used to obtain counter of user's list of posts. If you want to get your own counter, you dont have to send anything. However, if you want to get counter of other user, you may send this user's *id* in the call:

```json
{
	"user": "USER_ID"
}
```

#### GET /post/search
This resource is used to find the *posts* that having in its content a particular word. You have to send the word that you want to search:
```json
{
	"query": "Lorem"
}
```
In this example, APP/NIMA will return all *posts* in its field *content* have the word "Lorem". An example would be:
```json
[
	{
		"_id"         : "POST_ID",
		"application" : "APPLICATION_ID",
		"content"     : "Lorem ipsum dolor sit amet, consectetur adipisicing elit.",
		"title"       : "Lorem ipsum",
		"create_at"   : "2013-12-02 08:00:58.784Z",
		"image"       : "http://IMAGE_URL",
		"owner":
			{
				"_id"        : "OWNER_ID",
				"avatar"     : "http://AVATAR_URL",
				"created_at" : "2013-12-02 08:00:58.784Z",
				"mail"       : "OWNER_MAIL",
				"name"       : "Javi",
				"username"   : "OWNER_NICKNAME"
			}
	},
	{
		"_id"         : "POST_ID",
		"application" : "APPLICATION_ID",
		"content"     : "Loremipsum es un ejemplo.",
		"title"       : "Lorem ipsum",
		"create_at"   : "2013-12-02 08:00:58.784Z",
		"image"       : "http://IMAGE_URL",
		"owner":
			{
				"_id"        : "OWNER_ID",
				"avatar"     : "http://AVATAR_URL",
				"created_at" : "2013-12-02 08:00:58.784Z",
				"mail"       : "OWNER_MAIL",
				"name"       : "Javi",
				"username"   : "ONWER_NICKNAME"
			}
	}
]
```

There also search through *pagination* to be explained in the following resource.

### Timeline
#### GET/user/timeline
This resource is used to get the list of posts of concrete user. If you want to get the posts of your session user you not have send any parameters with the request. APP/NIMA will return the list of your posts both and the posts of users you follow (Following) sorted from oldest to most recent.

But if you want to get another user posts or only your own, you must send the following parameters:
```json
{
	"id": "USER_ID" 
}
```

This is the id of the user you want to get the post. In this case, it will only return the list of the post that created this user.

If all goes well you only have to wait for the `200 OK` response and the list of posts that will return APP/NIMA.

There is also the option for you to return the list of posts with pagination, that is, that in each API call it returning part of the list of posts chronologically.

To do this, you must send the following parameters:
```json
{
	"page"        : "0",
	"num_results" : "5",
	"last_data"   : "2013-12-02 08:00:58.784Z"
}
```

To this object, if you want, must be added the user *id*.

*page* variable is the page number you want to obtain, that is, the part of the list you want to get. *num_results* is the number of results you want to obtain. In the first call, this variable will be multiplied by 2, and in other cases, this variable is the same. Finally, *last_data* variable is the creation date of the last post received in the last call. This date is important because it will be the starting point of the next part of posts.


[Location](id:places)
--------
With this resource you can work with geolocation. Retrieve information about places around a point or get information about people near someone or someplace. To do so, all request have to go to:

    http://api.appnima.com/location/{RESOURCE}

Remember all requests to App/nima should be identified by your `Appnima.key` or key pair `client` and `secret`.


So, the first parameter is the type of request (GET, POST, UPDATE, DELETE …) and the second the name of resource.


### Places
#### GET /places
Use this resource to get places around a point. You can determinate the range result with precision or radius. If you like to work with precision, send 0 (hight precision), 1 or 2 (less precision). If you prefere work with radius make the request like:
```json
{
	"latitude"  : "-33.9250334",
	"longitude" : "18.423883499999988",
	"radio"     : "500"
}
```

To work with precision, type:
```json
{
	"latitude"  : "-33.9250334",
	"longitude" : "18.423883499999988",
	"precision" : "1"
}
```


Responses are returned with `200 Ok` and the list of places:
```json
[
	{
		"address"  : "Neurketa Kalea, 8, Mungia, Spain",
		"country"  : "ES",
		"id"       : "PLACE_ID",
		"locality" : "Mungia",
		"name"     : "Frus Surf",
		"phone"    : "+34 946 15 57 71",
		"position" :
			{
				"latitude"  : 43.356091,
				"longitude" : -2.847759
			},
		"postal_code" : "POSTAL_CODE",
		"reference"   : null,
		"types":
			{
				"0": "establishment"
			},
		"website": "http://shop.frussurf.com/"
	},
	{
		"address"  : "Neurketa Kalea, 3, Mungia, Spain",
		"country"  : "ES",
		"id"       : "PLACE_ID",
		"locality" : "Mungia",
		"name"     : "Inmobiliaria Urrutia",
		"phone"    : "PLACE_PHONE",
		"position" :
			{
				"latitude"  : 43.35618,
				"longitude" : -2.847939
			},
		"postal_code" : "POSTAL_CODE",
		"reference"   : null,
		"types":
			{
				"0": "establishment"
            },
		"website": "http://www.inmobiliariaurrutia.com/"
	},
	{
		"address"  : "Neurketa Kalea, 8, Mungia",
		"country"  : null,
		"id"       : "PLACE_ID",
		"locality" : null,
		"name"     : "Bar Aketxe",
		"phone"    : null,
		"position" :
			{
				"latitude"  : 43.356091,
				"longitude" : -2.847759
           	},
		"postal_code" : null,
		"reference"   : "REFERENCE_ID",
		"types"       : [],
		"website"     : null
	}
]
```

#### GET /place
With this resource you can request more details about a particular establishment. If the place has `reference` attribute, sends the request like:

```json
{
	"id"        : "PLACE_ID",
	"reference" : "REFERENCE_ID"
}
```

If the place has not `reference` sends the request like:

```json
{
	"id": "PLACE_ID"
}
```

Responses are returned with `200 Ok` and the place detail:
```json
{
	"address"  : "Neurketa Kalea, 8, Mungia, Spain",
	"country"  : "ES",
	"id"       : "PLACE_ID",
	"locality" : "Mungia",
	"name"     : "Bar Aketxe",
	"phone"    : "PLACE_PHONE",
	"position" :
		{
			"latitude"  : 43.356091,
			"longitude" : -2.847759
		},
	"postal_code" : "POSTAL_CODE",
	"reviews"     :
		{
			"aspects":
				[
					{
						"rating": 1,
						"type"  : "food"
					},
					{
						"rating": 1,
						"type"  : "decor"
					},
         			{
						"rating": 1,
						"type"  : "service"
					}
				],
			"author_name": "AUTHOR_NAME",
			"author_url" : "https://plus.google.com/101519756922440365704",
			"text"       : "BUENAS CORTEZAS DE CERDO, Y MUY BUENAS RABAS."
		},
	"types":
		{
			"0": "bar",
           	"1": "establishment"
		}
}
```
#### POST /place
Use this resource to create place data profile. . Sends the request with the next parameters:
```json
{
	"name"        : "San Mames",
	"address"     : "Felipe Serrate, s/n",
	"locality"    : "Bilbao",
	"postal_code" : "POSTAL_CODE",
	"country"     : "ES",
	"latitude"    : "43.26",
	"longitude"   : "-2.94"
}
```

Optionally you can add the next information:
```json
{
	"mail"    : "PLACE_MAIL",
	"phone"   : "PLACE_PHONE",
	"website" : "www.sanmames.com"
}
```

Responses are returned with `201 Created`.


### Checkins
#### POST /checkin
Users from your applicaction can register visits into a place. Use this resource with the parameter id:
```json
{
	"id": "PLACE_ID"
}
```

Responses are returned with `200 Ok` and the object:
```json
{
	"status": "ok"
}
```

#### GET /checkin
Get a list of saved places by your users. Just send the user id:
```json
{
	"id": "USER_ID"
}
```

Responses are returned with `200 Ok` and the object:
```json
{
	"address"  : "Calle de Trobika, 1, Mungia, Spain",
	"country"  : "ES",
	"id"       : "PLACE_ID",
	"locality" : "Mungia",
	"name"     : "Policía Municipal",
	"phone"    : "PLACE_PHONE",
	"position" :
		{
			"latitude" : 43.354551,
			"longitude": -2.846533
		},
	"postal_code" : "POSTAL_CODE",
	"reviews"     : [],
	"types":
		{
			"0": "police",
			"1": "establishment"
		}
}
```

### Search
#### GET /friends
Shown to your users information about friends near to a point. You just need send with de request the following parametrs:
```json
{
	"latitude"  : "-33.9250334",
	"longitude" : "18.423883499999988",
	"radio"     : "500"
}
```

If the request was successful App/nima returns `200 Ok` and the user data:
```json
{
	"avatar"   : "http://appnima-dashboard.eu01.aws.af.cm/static/images/avatar.jpg",
	"id"       : "USER_ID",
	"name"     : "Cata",
	"username" : "USER_MAIL"
}
```

#### GET /people
As **GET /friends** you can provides information about people who are not into users list friends. The request is made with the same parameters:
```json
{
	"latitude"  : "-33.9250334",
	"longitude" : "18.423883499999988",
	"radio"     : "500"
}
```

### User
#### GET /user
You can use this resource to obtain the last user location stored. Just sends the request without parameters. App/nima will return the following object:

```json
{
	"latitude"  : "-33.9250334",
	"longitude" : "18.423883499999988"
}
```

#### POST /user
If you need save user position use this resource. Sends the request with latitude and longitude like:

```json
{
	"latitude"  : "-33.9250334",
	"longitude" : "18.423883499999988"
}
```
If the request was successful, App/nima will return `201 Created` message.


[Socket](id:sockets)
------
App/nima provides sockets environments to get chat rooms. To do so, all request have to go to:

    http://socket.appnima.com/{RESOURCE}

Remember all requests to App/nima should be identified by your `Appnima.key` or key pair `client` and `secret`.

So, the first parameter is the type of request (GET, POST, UPDATE, DELETE …) and the second the name of resource.


### Getting started
To do real time communication, appnima connects to the rooms that have been created using socket.io. Now you will see the methods that are exposed to interact with them. Although we recommend using our library `Appnima.js` for this purpose.

#### open
To create a room we will call to the `open` method. This one will receive the following parameters:

* User's token
* Context (it will be used to identify the room for future connections)
* Room name
* Room Type
* If we want that the room persist in time
* Allowed users' id array

If everythng is right the room will be created and the author will be automatically connected to it.

#### join
A user can be connected to a room if it is created and he has permission to connect to it. To do this `join` method must be called with the following parameters:

* User's token (for anonymous session not required)
* Context (identification of the room to connect)
* Application id (in case of anonymous session it is mandatory)

#### leave
To disconnect from a room just call the method `leave`.

#### sendmessage
To send a message to a room just call the method `sendMessage` with an object as parameter (a message can be any type of data). This message will be received by all the room's users, also the sender, through a listener called `onMessage` and it will have the following format:
```json
{
	"user"       : "User that sends the message",
	"message"    : "Message sent",
	"created_at" : "2013-05-23T12:01:02.736Z"
}
```

#### sendbroadcast
This methods works as `sendMessage`, the unique difference is that the sender won't receive the message.

#### allowusers
Users can be added to the allowed user list using `allowUsers` method, to do this just call this method and use users' id array as parameter.

#### disallowusers
Users can be removed from the allowed user list using `disallowUsers` method, to do this just call this method and use users' id array as parameter.


### Types and permissions
There are several room types, each one has different permissions depending the user.

* Private: Only users in the allowed list can connect, send and receive messages in this room.
* Public: All the people can connect to this room, but only the owner can post.
* Inbox: Only the owner can read messages, but any user can send messages.
* Application: This room exists in all the applications so no one can create it, everyone can connect, receive or send messages in it.

### HTTP Request

#### GET /rooms
Get a list of rooms where a user is participant. You just need call the resource and if the query was successful App/nima returns `200 Ok` and the list like this:
```json
[
	{
		"id"         : "ROOM_ID",
		"name"       : "Amigos",
		"created_at" : "2013-05-23T12:01:02.736Z"
	},
	{
		"id"         : "ROOM_ID",
		"name"       : "Appnima friends",
		"created_at" : "2013-02-23T12:01:02.736Z"
	}
]
```


[Push](id:push)
----
Use this resource to send push notifications to users devices. To do so, all request have to go to:

    http://api.appnima.com/push/{RESOURCE}

Remember all requests to App/nima should be identified by your `Appnima.key` or key pair `client` and `secret`.

So, the first parameter is the type of request (GET, POST, UPDATE, DELETE …) and the second the name of resource.


#### POST /push
This resource allows you to send push notifications to the user device. Sends the request and the following parameters:
```json
{
	"user"    : "USER_ID",
	"title"   : "Texto a mostrar en la notificación",
	"message" : "El message de la push"
}
```
If the notification was successful App/nima returns `200 Ok`.
