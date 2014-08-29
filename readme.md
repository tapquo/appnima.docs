App/nima
========
Find out how to add soul to your apps with first LaaS in the world.

*Current version: [1.06.16]()*

Introduction
------------
Just for reading this document you are telling that you are a developer who wants to improve his skills continously and wants to create more efficient projects. App/nima is the first platform which offers logic services for any type of project, no matter you want to create an application or a site, App/nima will help you in both situations.

A bit of history, 3 years ago in [**Tapquo**](http://tapquo.com) we found a common problem in the development area, it was sthat each new product we create we had to repeat every time the same basic functionalities. App/nima borns from the need of being more efficients and the desire to develop only the business features of our new product and no more the horizontal l

+ OAuth 2 services for authentication
+ User management
+ User social network
+ Messaging service to send emails, SMS, calls and private messages
+ Find/geolocate places
+ Find/geolocate other users
+ Real-time communications via sockets
+ Push notifications for ioS, Android and Blackberry
+ Get our users' behaviour

If sometime you have had to develop some of this functionalities in any project, you are welcome to our platform because App/nima is thougth to help you as a developer. We want to give you a platform made to ease your life, we love software as much as you and we seek being better. Use App/nima.


### Architecture
Probably now you have doubts of how App/nima can help you in being more efficient, but calm down we are going to explain you how it works. App/nima is completely based in the authentication protocol [**Oauth 2**](http://oauth.net/2/) and in the serialization via REST-style with JSON objects. If you have never used this paradigm don't worry, we are explaining step by step how to connect to the platform and will offer you explicit tools such as [**AppnimaJS**](https://github.com/tapquo/appnima.docs/blob/master/EN/appnimajs.md) that will ease you the connection process.

All the backend is deployed around the world using platforms such as Amazon or Google Cloud Engine, the ones which give us the ability to give allways the best response quality for our users. Don't worry, you won't have to be a *Jedy SysAdmin* because you will only have to communicate with our REST. The scalability is our duty, you will only have to worry about creating the best project and us in providing every day the best platform.

In summary, App/nima is a platform in a REST API way that provide you with the logical services of your projects, and depending the nature of your project maybe you won't need your own backend. Each logical service is hosted in different servers to obtain the biggest independent scalability, along the documentation we will watch the routes of each service.

### OAuth 2
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

To talk with us or with other developers about the API, suscribe to our [**mailing list**](http://tapquo.com/forum).


API REST
========

CONNECTION
----
To connect with the platform necessary to authenticate requests. Exists two ways: sending the application `KEY` or the `ACCESS_TOKEN` of user. You can get the application `KEY` from your [**dashboard**](http://appnima.tapquo.com). You can get the `ACCESS_TOKEN` of user at user's attributes after `signup`.

So, for requests with application `KEY` your headers must be like:

```json
{
  "authorization" : "basic APPLICATION_KEY"
}
```

And for requests with `ACCESS_TOKEN` of user:

```json
{
  "authorization" : "bearer USER_ACCESS_TOKEN"
}
```


User
----
Use this resource to add users of your project within App/nima platform. To do so, all request have to go to:

    http://api.appnima.com/user/{{resource}}

Remember all requests to App/nima should be identified by your application `KEY` or `ACCESS_TOKEN` of user. So, the first parameter is the type of request (GET, POST, UPDATE, DELETE …) and the second the name of resource.

*Example*

    [GET] http://api.appnima.com/user/info


### Security
#### POST /signup
All users of your application must be App/nima users. So the first step is register the user to get his token. This request needs "authorization" : "basic APPLICATION_KEY" and parameters like:

```json
{
  "mail"      : "javi@tapquo.com",
  "password"  :  "USER_PASSWORD"
}
```

You can send additional fields like:

```json
{
  ...
  "username"  : "soyjavi",
  "name"      : "Javi Jimenez",
}
```

Responses are returned with `201 Created` and the object:

```json
{
  "id"            : "939349943434",
  "application"   : "478054177842",
  "access_token"  : "bBaMrMUlIRelDL5s5399b74",
  "refresh_token" : "TY4BOjc0E1Ds1WBzQCGFG539",
  "expire"        : "2014-06-24T15:19:13.138Z"
}
```

#### POST /login
Use this resource  to provide access permissions to your application. This request needs "authorization" : "basic APPLICATION_KEY" at headers and parameter like:

```json
{
  "mail"        : "javi@tapquo.com",
  "password"    :  "USER_PASSWORD"
}
```

If the validation was successful App/nima returns `200 Ok` and the user data.

```json
{
  "id"            : "5398979f21f4b9eee73e324a",
  "mail"          : "javi@tapquo.com",
  "usenarme"      : "soyjavi",
  "name"          : "Javi Jimenez",
  "avatar"        : "http://appnima.com/img/avatar.jpg",
  "access_token"  : "bBaMrMUlIRelDL5s5399b74",
  "refresh_token" : "TY4BOjc0E1Ds1WBzQCGFG539",
  "expire"        : "2014-06-23T17:53:35.697Z"
}
```

#### POST /token
From now, to access any resource from App/nima you must send at headers the attribute `authorization` with `ACCESS_TOKEN` value:

```json
{
  "authorization" : "bearer ACCESS_TOKEN"
}
```

If the `ACCESS_TOKEN` is expired or it is necessary a new one, you can use this resource to get a new `ACCESS_TOKEN`. In this case the header must be like:


```json
{
  "authorization": "basic KEY",
}
```

And the parameters mus be `grant_type` with string "refresh_token" and the value of `"REFRESH_TOKEN"` of user:

```json
{
  "grant_type"    : "REFRESH_TOKEN"
  "refresh_token" : "refresh_token"
}
```

Server responds with `200` and the following object:

```json
{
  "id"            : "939349943434",
  "application"   : "478054177842",
  "access_token"  : "bBaMrMUlIRelDL5s5399b74",
  "refresh_token" : "TY4BOjc0E1Ds1WBzQCGFG539",
  "expire"        : "2014-06-24T15:19:13.138Z"
}
```

### Info
#### GET /info
If you need to get data from the user, you should use this resource and how you're using the OAuth authentication protocol 2 is not required to send any parameter unless you want to get the information about another user from your application:

```json
{
  "id"            : "939349943434"
}
```

In both cases, App/nima returns:

Get user data with this resource. Then, just wait the response `200 Ok` and APP/NIMA will returns:

```json
{
  "id"              : "28319319832"
  "mail"            : "javi@tapquo.com",
  "username"        : "soyjavi",
  "name"            : "Javi Jimenez",
  "avatar"          : "http://appnima.com/img/avatar.jpg",
  "language"        : "spanish",
  "country"         : "ES",
  "bio"             : "Founder & CTO at @tapquo",
  "phone"           : "PHONE_NUMBER",
  "site"            : "http://USER_URL"
}
```

#### PUT /update
This resource is used to modify the personal data of a user within your application, as in the resource **GET/user/info** is not necessary to identify the user as parameter. You can send all the parameters below (but are not required to send them all):

```json
{
  "name"        : "Javi",
  "mail"        : "javi@tapquo.com",
  "avatar"      : "AVATAR",
  "language"    : "spanish",
  "country"     : "Spain",
  "phone"       : "PHONE_NUMBER",
  "site"        : "http://USER_URL",
  "bio"         : "Founder & CTO at @tapquo"
}
```

The `AVATAR` attribute can be sent in two ways: like base64 or like `new Image()` format.

If the request was successful App/nima returns `200 Ok` and the same object **GET /user**. If the user has not permission to modify his data App/nima returns `403 Forbidden`.


### Password
APP/NIMA offers its users two ways to deal passwords, remember it or change it.

#### POST /remember/password
This resource serves to remember password. If one backend application to remember password is necessary to do so in two steps, if not have, or do not wish to use their own backend, can be made in one step.

If you want to do in one step, you must send the following parameters:

```json
    {
        mail: "mail@mail.com",
        application: "4234324233"
    }
```

This parameter is user ```mail``` APP/NIMA. APP/NIMA is responsible for managing the URL that is sent in the email, as explained below.

On the other hand, if you want to do it in two steps, the parameters to be sent along with the request, and in addition to the user token, are the following:


```json
    {
        mail: "mail@mail.com",
        application: "4234324233",
        url: "http://domain"
    }
```
The second parameter is the id of application and the latter is the domain of the application that calls this function.

This function sends a mail to the user who owns the token from APP / NIMA with a URL as follows:

    http://domain/forgot?forgot_key=CODE

The code is generated by APP/NIMA and serves to identify which user request asked to remember the password.

#### POST /reset/password
As mentioned in the previous appeal, remember password can be done in two steps. If doing so, the second part is to call this resource. This feature alone can not be used because it requires the code generated in the previous appeal.

For this, as you can see, it is necessary to generate an endpoint in the backend of the application with this URL where there is a form where you fill in the desired new password. Therefore we should send the application the following information:

```json
    {
        code: "jk23j4k32knm423klh6j56jhjl",
        password: "1234509"
    }
```

The first parameter is the URL code generated in the previous action and the second parameter is the new password that the user wants to regenerate. Once done, APP/NIMA user sends an email telling you that your password has been changed.

#### PUT /password
This resource is used to change the password, the only requirement is to be identified with APP/NIMA. Along with the petition must give the following parameters:

```json
    {
        old_password: 123456
        new_password: 098763
    }
```


### Terminal
#### POST /terminal
This resource allows you register the user device. Sends the request and the following parameters:
```json
    {
        type:       "phone",    /* computer, tablet, phone, tv */
        os:         "ios",      /* windows, macos, linux, ios, android, blackberry, firefoxos, windowsphone, other */
        version:    "6.0"       /* ?.? */
    }
```

Responses are returned with `201 RESOURCE CREATED`.


#### GET /terminal
Retrieves information about what devices accessed to your application. Like **GET /user/info** you do not need any parameter. App/nima returns `200 Ok` and the following object:
```json
    [{
        _id:        "5719721071057"
        type:       "phone",
        os:         "ios",
        token:      "OHSAF9075HFQ",
        version:    "6.0"
    },
    {
        _id:        "57592807235"
        type:       "desktop",
        os:         "macos",
        token:      "89YWEOFOGH022GB",
        version:    "10.8"
    }]
```

#### PUT /terminal
Updadtes the device information with this resource. Just send:
```json
    {
        terminal:   "5719721071057"
        type:       "phone",    /* computer, tablet, phone, tv */
        os:         "ios",      /* windows, macos, linux, ios, android, blackberry, firefoxos, windowsphone, other */
        version:    "6.0"       /* ?.? */
    }
```

You need use this resource when a user wishes to unsubscribe from your application.


### Subscriptions
#### POST /subscription
Registered e-mail of users who want to receive information from your application or who want be invited to your application. You just need send like parameter the e-mail address:
```json
    {
        mail:       "javi@tapquo.com"
    }
```

Responses are returned with `200 Ok` and the object:
```json
    {
        message:    'Request accepted.'
    }
```


### Support
#### GET /user/ticket
Use this resource to obtain a ticket specifically. Simply you have to send the ticket ID to be obtained.

```json
     parameters = {id: "1356f43524fa4"}
```

#### GET /user/ticket/search
Use this resource to find a set of tickets. You can filter by the following fields:

- User: For tickets for this particular user.
- Reference: For tickets for this reference.
- Type: For tickets of this type.
- Solved: (true or false) For tickets resolved (true) or earrings (false)

It can search for a particular field or by mixing several. The following example would be a query with all the fields and send the object would be:

```json
     parameters = {
     user: "1356f43524fa4"
     reference: "3542j5425i44d"
     type 2
     solved: true
    }
```

In this case you can use the pagination. This option is the same as in the `POST` methods except that not send `last_data`attribute.

#### POST /user/ticket
Use this resource as ticket managing system to resolve incidences or attend consults from users. The request requires an object as follows:

```json
    parameters = {
        title       : "[QUESTION]: How can I do this?",
        description : "Lorem ipsum dolor sit amet, consectetur adipisicing elit",
        reference   : "1356f43524fa4",
        type        : "2"
    }
```

The reference field is used if you want to add the ID of any other model, either APPNIMA or another database .
The type field can be 0, 1 or 2. If this field is not sent, the default is 0 .

0 - > "question"
1 - > " bug"
3 -> "support"

    Appnima.User.ticket(parameters);
    
#### PUT /user/ticket
On the other hand, if you modify a ticket you have two options :

The first would be to change the ticket. This is only possible when the ticket is not responded . To do this you just have to send the same data to create when adding the ID of the ticket to be modified .

The other option is to answer a ticket. This would have to send the following criteria:

```json
    parameters = {
        response : "Lorem ipsum"
    }
```

Once replied to ticket, an email is sent to the creator of that ticket.
In both cases you have to send the data to the following call:

    Appnima.User.updateTicket(parameters);

#### DELETE /user/ticket
You also can delete a comment sending a following parameters.

```json
    parameters = {
        id : "40928300482390"
    }
```

Network
-------
Use this resource to create a social network into your application: find friends, follow people, unfollow, get list of friends… To do so, all request have to go to:

    http://api.appnima.com/network/{RESOURCE}

Remember all requests to App/nima should be identified by your `Appnima.key` or key pair `client` and `secret`.

So, the first parameter is the type of request (GET, POST, UPDATE, DELETE …) and the second the name of resource.



### Relationships
#### GET /search
Search people into your application. Sends any string for mail or username attribute to search:
```json
    {
        query:      "javi"
    }
```

The server returns `200 Ok` and the list that contains the query:
```json
    [{
        avatar      : "http://appnima.com/img/avatar.jpg"
        id          : "59f34ac11a7e121b112b431f"
        name        : "javi"
        username    : "javi@javi.com"
    },
    {
        avatar      : "http://appnima.com/img/avatar.jpg"
        id          : "59f34ac11a7e121b112b431e"
        name        : "javier"
        username    : "a3@appnima.com"
    },
    {
        avatar      : "http://appnima.com/img/avatar.jpg"
        id          : "59f34ac11a7e121b112b431d"
        name        : null
        username    : "j.villar@javi.com"
    }]
```

The variable *following* indicate that user is loggued is follow or not that user, and the variable *follower* indicate that user follow or not loggued user.

#### POST /follow
To follow a user you can use this resource with the ID user:
```json
    {
        user:   "23094392049024112b431d"
    }
```

Responses are returned with `200 Ok` and the object:
```json
    {
        message: "Successful"
    }
```
If you want to follow a shielded relation, ie, you're a friend of the user to send and he is yours in a single call, simply send the following parameters:
```json
    {
        user:   "23094392049024112b431d",
        shield: true
    }
```

#### POST /unfollow
As **POST /follow** to unfollow a person you just need send with the request the ID user:
```json
    {
        user:   "23094392049024112b431d"
    }
```

Responses are returned with `200 Ok` and the object:

```json
    {
        message: "Successful"
    }
```

### Information
With these resources you can get the list of followings and followers of a user.

#### GET /following
Retrieves the list of followings of a user. Sends the request and ID user:
```json
    {
        user:   "23094392049024112b431d"
    }
```

App/nima returns `200 Ok` and the list:
```json
    count: 4
    [{
        avatar  : "http://cata.jpg"
        id      : "52f34ac66a7e665b666b6617"
        mail    : "cata@cata.com"
        name    : "cata"
        username: "cata"
    },
    {
        avatar  : "http://jany.jpg"
        id      : "52f34ac66a7e665b222b6617"
        mail    : "jany@jany.com"
        name    : "jany"
        username: "janixy91"
    },
    {
        avatar  : "http://a1.jpg"
        id      : "52f34ac66a7e444b666b6617"
        mail    : "a1@appnima.com"
        name    : "a1"
        username: "a1@appnima.com-1391099964446-1391100156004"
    },
    {
        avatar  : "http://avatar.jpg"
        id      : "52f34ac66a7e665b666b6618"
        mail    : "a2@appnima.com"
        name    : "a2"
        username: "a2@appnima.com"
    }]
```

Like in *timeline*, there is the option of getting this data using pagination. To do this, you have to add the following parameters to your call:


```json
    {
     username       : username
     page           : 0
     num_results    : 5
    }
```


The meaning of the variable *page* and *num_results* is the same as in the case of the call to **Timeline**. The only difference between the two calls is that in this case dont need to send the variable of the last date.

#### GET /followers
As **GET /following** you can retrieve a list of followers of a user.


Retrieves the list of followings of a user. Sends the request and ID user:
```json
    {
        user:       "23094392049024112b431d"
    }
```

App/nima returns `200 Ok` and the list:
```json
    count: 3
    [{
        avatar  : "http://cata.jpg"
        id      : "52f34ac66a7e665b666b6617"
        mail    : "cata@cata.com"
        name    : "cata"
        username: "cata"
    },
    {
        avatar  : "http://jany.jpg"
        id      : "52f34ac66a7e665b222b6617"
        mail    : "jany@jany.com"
        name    : "jany"
        username: "janixy91"
    },
    {
        avatar  : "http://a1.jpg"
        id      : "52f34ac66a7e444b666b6617"
        mail    : "oihane@oihane.com"
        name    : "oihane"
        username: "oihane"
    }]
```

Like has been explained in **GET /followings**, there is also the option of getting the data using pagination. The dynamic is the same.

As can be seen, in this case, the call returns one more variable in each object. This variable indicate that user is loggued is follow or not that user.

#### GET /friends

With this resource you can get the list of friends of a user or user session. Sends the request with user id and obtain his friends. Sends without parameter and get friends of user session. Users of your application will become friends when they follow each other.

Request structure:

```json
    {
        user:       "23094392049024112b431d"
    }
```

App/nima returns `200 Ok` and the list:
```json
    [{
        avatar  : "http://jany.jpg"
        id      : "52f34ac66a7e665b222b6617"
        mail    : "jany@jany.com"
        name    : "jany"
        username: "janixy91"
    }]
```

#### GET /check
If you need to know about relationship with another user, use this resource and his ID user:
```json
    {
        user:       "23094392049024112b431d"
    }
```

If the query was successful App/nima returns `200 Ok` and the object which describes the relationship between both users:
```json
    {
        following:  true,
        follower:   false
    }
```


### Post
#### POST/post
A post is a public message and the user can create with this resource. Only have to send this parameters with the request:

```json
    {
        title: "Lorem Ipsum",
        content:  "Lorem ipsum dolor sit amet, consectetur adipisicing elit.",
        image: "http://IMAGE_URL
    }
```
The only required field when creating a post is ```content``` that it is the content of the message.

If all goes well you only have to wait for the answer `200 ok` and APP/NIMA will returns the following parameters:
```json
     post = {
        id         : 4234325425234,
        content    : "Lorem Ipsum",
        image      : "http://IMAGE_URL",
        owner      : {
            id       : 423423432423,
            name     : user1,
            username : username1,
            avatar   : http://AVATAR_URL
        },
        comments   : [],
        likes      : [],
        is_liked   : false,
        created_at : POST_CREATED_DATA
    }
```

#### POST /put
This resource is used to modify a previously created post. To do this, the user must send parameters with the request:

```json
    {
        id: POST_ID,
        title: "Lorem Ipsum",
        content:  "Lorem ipsum dolor sit amet, consectetur adipisicing elit.",
        image: "http://IMAGE_URL
    }
```
If all goes well you only have to wait for the answer `200 ok` and APP/NIMA returns `message: "Successful"`.

#### GET/ post
This resource is used to obtain a specific post. To do this, the user must only send `id` of the post you want to get, and if all goes well, APP/NIMA return the `200 OK` and the concrete post with same style as in `POST `.

#### DELETE/post
This resource is used to delete a specific post.To do this, the user must only send `id` of the post you want to get, and if all goes well, APP/NIMA return the `200 OK` and the concrete post with same style as in `POST `.

#### GET /post/user
This resource is used to obtain counter of user's list of posts. If you want to get your own counter, you dont have to send anything. However, if you want to get counter of other user, you may send this user's *id* in the call:

```json
    {
        user: 42342354543543
    }
```

#### GET /post/search
This resource is used to find the *posts* that having in its content a particular word. You have to send the word that you want to search:
```json
    {
        query: "Lorem"
    }
```
In this example, APP/NIMA will return all *posts* in its field *content* have the word "Lorem". An example would be:
```json
    [id         : 5453435345,
        content    : "Lorem Ipsum",
        image      : "http://IMAGE_URL",
        owner      : {
            id       : 423423432423,
            name     : user1,
            username : username1,
            avatar   : http://AVATAR_URL
        },
        comments   : [
                {
                    id: 4324234,
                    content: "Comment 1",
                    created_at: comment_created_data,
                    owner: {
                        avatar: http://AVATAR_URL,
                        id: 3425425425,
                        name: user,
                        username: username
                    }
                }
            ],
        likes      : [
            {
                avatar: http://AVATAR_URL,
                id: 3425425425,
                name: user,
                username: username
            },
            {
                avatar: http://AVATAR_URL,
                id: 54236435767453,
                name: user1,
                username: username1
            }
        ],
        is_liked   : false,
        created_at : POST_CREATED_DATA
        },id         : 4234325425234,
        content    : "Lorem Ipsum",
        image      : "http://IMAGE_URL",
        owner      : {
            id       : 423423432423,
            name     : user1,
            username : username1,
            avatar   : http://AVATAR_URL
        },
        comments   : [
                {
                    id: 4324234,
                    content: "Comment 1",
                    created_at: comment_created_data,
                    owner: {
                        avatar: http://AVATAR_URL,
                        id: 3425425425,
                        name: user,
                        username: username
                    }
                }
            ],
        likes      : [
            {
                avatar: http://AVATAR_URL,
                id: 3425425425,
                name: user,
                username: username
            },
            {
                avatar: http://AVATAR_URL,
                id: 54236435767453,
                name: user1,
                username: username1
            }
        ],
        is_liked   : false,
        created_at : POST_CREATED_DATA
        }
    ]
   }
```
There also search through *pagination* to be explained in the following resource.

### Timeline
#### GET/timeline
This resource is used to get the list of posts of concrete user. If you want to get the posts of your session user you not have send any parameters with the request. APP/NIMA will return the list of your posts both and the posts of users you follow (Following) sorted from oldest to most recent.

But if you want to get another user posts or only your own, you must send the following parameters:
```json
    { username: username}
```
This is the id of the user you want to get the post. In this case, it will only return the list of the post that created this user.

If all goes well you only have to wait for the `200 OK` response and the list of posts that will return APP/NIMA.

There is also the option for you to return the list of posts with pagination, that is, that in each API call it returning part of the list of posts chronologically.

To do this, you must send the following parameters:
```json
    { page: 0,
      num_results: 5
      last_data: "2013-12-02 08:00:58.784Z"
    }
```
To this object, if you want, must be added the user *id*.

*page* variable is the page number you want to obtain, that is, the part of the list you want to get. *num_results* is the number of results you want to obtain. In the first call, this variable will be multiplied by 2, and in other cases, this variable is the same. Finally, *last_data* variable is the creation date of the last post received in the last call. This date is important because it will be the starting point of the next part of posts.

### Likes
#### POST /post/like
This resource is used to do favorite particular post or to remove a favorite already done. To do this, you just have to send the *id* of that post.

```json
    {
        post: 498342893788734
    }
```
If this is the first time you mark as favorite, APP NIMA return the `200 OK` response. But if you had liked before, delete that favorite and APP/NIMA return a message: *unliked*.

#### GET /post/user/like
This resource, if all goes well, return the list of the *posts* of the user has logged liked. To do this, you must send the following parameter:
```json
    { username: username}
```
Here there is also the possibility of using *pagination* to list post as explained in the resource **TIMELINE**.

#### GET /post/like/users
This resource used to get all users who have liked an specific *post*. To do this, you only need to send the *id* of that post.

```json
    {
        post: "Lorem Impsum…"
        id: 84935746435693
    }
```

### Comment
#### POST /post/comment
This resource is used to create comments on a `post`. The idea of ​​this resourse is that you can create discussions on the post. To make a comment you must send the following parameters:
```json
    {
      id      : "post_id"
      content : "Lorem ipsume",
      title   : "Lorem impsum"
    }
```
#### GET /post/comment
This resource is used to get all the comments from a post. You just have to send the id of the *post* so that it will return the list of comments.

#### PUT /post/comment
This resource update a comment and you only have to send id of comment and want to update attributes.
```json
    {
      id      : "comment_id"
      content : Lorem ipsum updated
      title   : Lorem ipsum updated
    }
```
#### DELETE /post/comment
This resource drops a comment, you have to send the comment id.


Messenger
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
        user:       23094392049024,
        subject:    "Appnima.com",
        message:    "Welcome to appnima.messenger [MAIL]"
    }
```

Responses are returned with `201 Created` and the object:
```json
    {
        message:    'E-mail sent successfully.'
    }
```

### SMS
#### POST /sms
Also, App/nima provides SMS messaging. Your application can send messages to users who have phone number registered into the platform. Sends the request and the following parameters:
```json
    {
        user:       23094392049024,
        message:    "Welcome to appnima.messenger [SMS]"
    }
```

Responses are returned with `201 Created` and the object:
```json
    {
        message:    'SMS sent successfully.'
    }
```


### Message
#### POST /message
If you need, App/nima gives private messaging between users on your application. Sends the request with the next parameters:
```json
    {
        user:       23094392049024,
        subject:    "Appnima.com",
        message:    "Welcome to appnima.messenger [MESSAGE]"
    }
```

The field subject is optional and App/nima returns `201 Created` and the object:
```json
    {
        message:    'Message sent successfully.'
    }
```

#### GET /message/outbox
Users of your application can retrieves messages sent. Just use this resource with the next parameter:
```json
    {
        context:    outbox
    }
```

App/nima returns `200 Ok` and a list of messages:
```json
    {
        _id:            120949303434,
        from:           120949303434,
        to:             120949303433,
        application     220949303432
        subject         "Appnima.com",
        body            "Welcome to appnima.messenger [MESSAGE]",
        state           SENT
    }
```



#### GET /message/inbox
As **GET /message/outbox** shown to your users a list received messages. Just change de context:
```json
    {
        context:    inbox
    }
```

And App/nima returns `200 Ok` and a list of messages:
```json
    {
        _id:            120949303434,
        from:           120949303434,
        to:             120949303433,
        application     220949303432
        subject         "Appnima.com",
        body            "Welcome to appnima.messenger [MESSAGE]",
        state           READ
    }
```

#### PUT /message
Modify the state of a message using this resource. Sends the request and the following parameters:
```json
    {
        message:    23094392049024,
        state:      "READ" /*READ or DELETED*/
    }
```

Responses are returned with `200 Ok` and the object:
```json
    {
        message:            "Resource READ."
    }
```


Location
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
        latitude:      "-33.9250334",
        longitude:     "18.423883499999988",
        radio:         "500"
    }
```
To work with precision, type:
```json
    {
        latitude:      "-33.9250334",
        longitude:     "18.423883499999988",
        precision:      "1"
    }
```


Responses are returned with `200 Ok` and the list of places:
```json
    [{
        address:        "Neurketa Kalea, 8, Mungia, Spain",
        country:        "ES",
        id:             "51e9290db68307fe5900001d",
        locality:       "Mungia",
        name:           "Frus Surf",
        phone:          "+34 946 15 57 71",
        position:
            latitude:       43.356091
            longitude:      -2.847759
        postal_code:    "48100",
        reference:      null,
        types:
            0:              "establishment"
        website:        "http://shop.frussurf.com/"
    },
    {
        address:        "Neurketa Kalea, 3, Mungia, Spain"
        country:        "ES"
        id:             "51e92893b68307fe59000017"
        locality:       "Mungia"
        name:           "Inmobiliaria Urrutia"
        phone:          "+34 946 15 66 95"
        position:
            latitude:       43.35618
            longitude:      -2.847939
        postal_code:    "48100",
        reference:      null,
        types:
            0:          "establishment"
        website: "http://www.inmobiliariaurrutia.com/"
    },
    {
        address:        "Neurketa Kalea, 8, Mungia"
        country:        null
        id:             "cd547ea9e3c4fe9d8f8883942a6fa8ac73130905"
        locality:       null
        name:           "Bar Aketxe"
        phone:          null
        position:
            latitude:       43.356091
            longitude:      -2.847759
        postal_code:    null
        reference:      "CnRoAAAAUV3iCS__"
        types:
        website:        null
    }
    ]
```

#### GET /place
With this resource you can request more details about a particular establishment. If the place has `reference` attribute, sends the request like:

```json
    {
        id:             "51e92bfab68307fe59000030",
        reference:      "CoQBcwAAAGFG6LT0qt4U3kxuIuywXrFmvZaUvAZ5zjhZWXMQJtGlL1afAla1RS6PYuANvkWVPzGMh3YMWThOM1NFUm5satOXxKKmUb7_H19Tfep"
    }
```

If the place has not `reference` sends the request like:

```json
    {
        id:             "51e92bfab68307fe59000030"
    }
```

Responses are returned with `200 Ok` and the place detail:
```json
    {
        address: "Neurketa Kalea, 8, Mungia, Spain"
        country: "ES"
        id: "51e92bfab68307fe59000030"
        locality: "Mungia"
        name: "Bar Aketxe"
        phone: "+34 946 74 18 40"
        position: Object
        latitude: 43.356091
        longitude: -2.847759
        postal_code: "48100"
        reviews:
            aspects:
                0: Object
                    rating: 1
                    type: "food"
                1: Object
                    rating: 1
                    type: "decor"
                2: Object
                    rating: 1
                    type: "service"
            author_name: "jc ce"
            author_url: "https://plus.google.com/101519756922440365704"
            text: "BUENAS CORTEZAS DE CERDO, Y MUY BUENAS RABAS."
        types:
            0: "bar"
            1: "establishment"
    }
```
#### POST /place
Use this resource to create place data profile. . Sends the request with the next parameters:
```json
    {
        name: "San Mames",
        address: "Felipe Serrate, s/n",
        locality: "Bilbao",
        postal_code: "48013",
        country: "ES",
        latitude: " ﻿43.26",
        longitude: "-2.94"
    }
```

Optionally you can add the next information:
```json
    {
        mail: "hello@sanmames.com",
        phone: "944411445",
        website: "www.sanmames.com"
    }
```

Responses are returned with `201 Created`.


### Checkins
#### POST /checkin
Users from your applicaction can register visits into a place. Use this resource with the parameter _id:
```json
    {
        id:            "51e92bfab68307fe59000030"
    }
```

Responses are returned with `200 Ok` and the object:
```json
    {
        status:     'ok'
    }
```

#### GET /checkin
Get a list of saved places by your users. Just send the user id:
```json
    {
        id:            "51e930cad2eeaea678000010"
    }
```

Responses are returned with `200 Ok` and the object:
```json
    {
        address: "Calle de Trobika, 1, Mungia, Spain"
        country: "ES"
        id: "51e930cad2eeaea678000010"
        locality: "Mungia"
        name: "Policía Municipal"
        phone: "+34 946 15 66 77"
        position:
            latitude: 43.354551
            longitude: -2.846533
        postal_code: "48100"
        reviews: Array[0]
        types:
            0: "police"
            1: "establishment"
    }
```

### Search
#### GET /friends
Shown to your users information about friends near to a point. You just need send with de request the following parametrs:
```json
    {
        latitude:       "-33.9250334",
        longitude:      "18.423883499999988",
        radio:          "500"
    }
```

If the request was successful App/nima returns `200 Ok` and the user data:
```json
    {
        avatar: "http://appnima-dashboard.eu01.aws.af.cm/static/images/avatar.jpg"
        id: "51aef6f4560d261d15000001"
        name: Cata
        username: "catalina@tapquo.com"
    }
```

#### GET /people
As **GET /friends** you can provides information about people who are not into users list friends. The request is made with the same parameters:
```json
    {
        latitude:       "-33.9250334",
        longitude:      "18.423883499999988",
        radio:          "500"
    }
```

### User
#### GET /user
You can use this resource to obtain the last user location stored. Just sends the request without parameters. App/nima will return the following object:

```json
    {
        latitude:       "-33.9250334",
        longitude:      "18.423883499999988"
    }
```

#### POST /user
If you need save user position use this resource. Sends the request with latitude and longitude like:

```json
    {
        latitude:       "-33.9250334",
        longitude:      "18.423883499999988"
    }
```
If the request was successful, App/nima will return `201 Created` message.


Calendar
--------

App/nima provides a calendar module. It is based on a user can create calendars, add events, and share them with others.


#### POST /calendar

With this feature the user creates a calendar. Along with the petition have to send your name and the color you want to assign:
```json
    {
        name:       "Calendario de trabajo",
        color:      "#3300FF"
    }
```

if all goes well, returns the calendar created

```json
    caledar: {
        id: 28319319833,
        name: 'mi calendario',
        color: '#FF66CC',
        created_at: Tue Feb 04 2014 13:19:06 GMT+0100 (CET),
        owner:
            {
                id: 52eb667ab71cd7e4be00000c,
                username: 'a1@appnima.com-1391158906892',
                mail: 'a1@appnima.com',
                avatar: 'http://appnima.com/img/avatar.jpg',
                name: 'name'
            },
        shared: [ ]
    }
```

#### PUT /calendar

We also have the option to change the name and color of a calendar already created. You have to send the "id" of the calendar, the new name and the new color.
```json
    {
        id:       "28319319833",
        color:    "#3300FF"
        nombre:   "Calendario de trabajo modificado"
    }
```

If the calendar does not exist, it returns a 404 error. If on the contrary exists, returns the calendar with the changed fields:
```json
    calendar : {
        id: 28319319833,
        name: 'mi nuevo calendario',
        color: '#FF66CC',
        created_at: Tue Feb 04 2014 13:19:06 GMT+0100 (CET),
        owner:
                {
                    id: 52eb667ab71cd7e4be00000c,
                    username: 'a1@appnima.com-1391158906892',
                    mail: 'a1@appnima.com',
                    avatar: 'http://appnima.com/img/avatar.jpg',
                    name: 'name'
                },
        shared: [ ]
    }
```

#### PUT /calendar/shared
It is possible to share a calendar with another user, so that you can view events for this calendar. In turn, you can also remove a user from a calendar. To do this, you must send to the API the "id", the calendar, the user profile id to invite, and the "state" field, which will "add", if you want to invite or "remove" if you want eliminate.
```json
    {
        id      : "28319319833",
        profile : "28319364941"
        state   : "add"
    }
```

If the calendar does not exist, it returns a 404 error. If on the contrary exists, return the updated calendar. The "shared" attribute corresponds to the list of users who have shared their calendar.
```json
    calendar   : {
                    id: 28319319833,
                    name: 'slid.us',
                    color: '#FF66CC',
                    created_at: Tue Feb 04 2014 12:52:55 GMT+0100 (CET),
                    owner: {
                        id: 52eb667ab71cd7e4be00000c,
                        mail: 'a1@appnima.com',
                        username: 'a1@appnima.com-1391158906892',
                        name: 'name',
                        avatar: 'http://appnima.com/img/avatar.jpg',
                    },
                    shared: [ 52eb667ab71cd7e4be000008 ]
                }
```
#### GET /calendar
With this resource we can get all the calendars they own the user, and those you have shared. This returns an "array" of calendars.
```json
    calendar   : [
                    {
                        id: 28319319833,
                        name: 'slid.us',
                        color: '#FF66CC',
                        created_at: Tue Feb 04 2014 12:52:55 GMT+0100 (CET),
                        owner: {
                            id: 52eb667ab71cd7e4be00000c,
                            mail: 'a1@appnima.com',
                            username: 'a1@appnima.com-1391158906892',
                            name: 'name',
                            avatar: 'http://appnima.com/img/avatar.jpg',
                        },
                        shared: [ 52eb667ab71cd7e4be000008 ]
                     }
                 ]
```

#### DELETE /calendar

There is also the possibility to delete a calendar, for that this resource is used. You only need to send as parameter "id" of the calendar.

```json
    {
        id      : "28319319833",
    }
```
If the calendar does not exist, it returns a 404 error. If on the contrary exists, return a message that everything gone successfully.

    {message: Successful}

#### GET /calendar/activity
App/nima can get the activities that have emerged in our calendar. For obtain a list of activities is used this resource  and sent as parameter the "id" of the calendar.
```json
    {
        id      : "28319319833",
    }
```
If the calendar does not exist, it returns a 404 error. If on the contrary exists, a list of activities with the structure shown below.
```json
    activities : [ {
                  id: 52f8ef8282652a000000000a,
                  message: 'u1net has created the event',
                  created_at: Mon Feb 10 2014 16:25:54 GMT+0100 (CET),
                  profile: {
                             username: 'u1net',
                             name: 'name',
                             mail: 'a1@appnima.com',
                             avatar: 'http://appnima.com/img/avatar.jpg',
                             id: 52eb667ab71cd7e4be00000c
                            },
                  event: {
                           id: 52f8ef8282652a0000000009,
                           calendar: 52f8ef8282652a0000000004,
                           date_init: Mon Apr 14 2014 09:00:00 GMT+0200 (CEST),
                           date_finish: Mon Apr 14 2014 11:00:00 GMT+0200 (CEST),
                           name: 'BilboStack updated',
                           description: 'This event is bilboStack',
                           place: 52f8ef8282652a0000000008,
                           assistents: [ 52eb667ab71cd7e4be000004 ],
                           created_at: Mon Feb 10 2014 16:25:54 GMT+0100 (CET),
                           tags: [ learn ],
                           guest: [ 52eb667ab71cd7e4be000004, 52eb667ab71cd7e4be000008 ]
                        },
                  calendar: {
                               id: 52f8ef8282652a0000000004,
                               name: 'Mi calendario updated',
                               color: '#FA58F4',
                               created_at: Mon Feb 10 2014 16:25:54 GMT+0100 (CET),
                               owner: 52eb667ab71cd7e4be00000b,
                               shared: [ ]
                            },
                  owner: {
                           id: 52eb667ab71cd7e4be00000c,
                           username: 'u1net',
                           mail: 'a1@appnima.com',
                           avatar: 'http://appnima.com/img/avatar.jpg',
                           name: 'name'
                         }
                }]
```
The event and calendar is where the activity was performed. If the event is null, is that only affects the calendar. The field "owner" is the person performing the activity and the "profile" field is the person who is targeted the activity.

#### POST calendar/event
Through this application, you can create an event to a calendar. Should be sent as a parameter the "id" of the calendar you want to belong to the new event, the event name, description, start and end date format mm-dd-yyyy hh: mm a string with a list of "id" separated "," corresponding to the users you want to share this event, a string with a list of tags separated by users "," taguear to the event, the address at which to hold the event, location, country, latitude and longitude:
```json
{
      calendar    : 52f0d497f4a9b16f47000002
      name        : "partido de futbol"
      description : "quedada para jugar un partido de fútbol"
      init        : "04-14-2014 09:00"
      finish      : "04-14-2014 11:00"
      address     : "c/ San Mames"
      locality    : "Bilbao
      country     : "España"
      latitude    : "23.23"
      longitude   : "-2.29"
      guest       : null
      tags        : "futbol,deporte"
}
```
This function returns the new event:

```json
    event: {
            id: 52f0e1e6d028ec6b6f000011,
            calendar: 28319319833,
            date_init: Mon Apr 14 2014 09:00:00 GMT+0200 (CEST),
            date_finish: Mon Apr 14 2014 11:00:00 GMT+0200 (CEST),
            description: 'quedada para jugar un partido de fútbol',
            name: 'partido de futbol',
            place:
                    {
                        address: 'c/ San Mames',
                        locality: 'Bilbao',
                        country: 'EspaÃ±a',
                        _id: 52f0e1e6d028ec6b6f000010,
                        __v: 0,
                        created_at: Tue Feb 04 2014 13:49:42 GMT+0100 (CET),
                        position: [ -2.29, 23.23 ]
                    },
            assistents: [ ],
            created_at: Tue Feb 04 2014 13:49:42 GMT+0100 (CET),
            tags: [futbol, deporte],
            owner:
                    {
                        id: 52eb667ab71cd7e4be00000c,
                        username: 'a1@appnima.com-1391158906892',
                        mail: 'a1@appnima.com',
                        avatar: 'http://appnima.com/img/avatar.jpg',
                        name: 'name'
                    }
            }
```
#### PUT calendar/event

It also allows us to modify an event through this resource. You must send at an object that takes as parameters the "id" of the event to be modified, the event name, description, start and end date in the format mm-dd-yyyy hh: mm a string with a list "id" separated "," corresponding to the users you want to share this event, a string with a list separated by "," address at which to hold the event tags users locally , country, latitude and longitude.
```json
{
      event       : 52f0e1e6d028ec6b6f000011
      calendar    : 52f0d497f4a9b16f47000002
      name        : "partido de baloncesto"
      description : "quedada para jugar un partido de baloncesto"
      init        : "04-14-2014 09:00"
      finish      : "04-14-2014 11:00"
      address     : "c/ San Mames"
      locality    : "Bilbao
      country     : "España"
      latitude    : "23.23"
      longitude   : "-2.29"
      guest       : null
      tags        : "futbol,deporte"
}
```
If the event does not exist, it returns a 404 error. If on the contrary exists, returns the event to the fields modified the structure of the object that is returned in the create event.

#### GET calendar/event
Through this resource can be obtained events calendars on the user owns , events calendars that have been shared , and the events to which you have been invited. Must filter events by time so required the "time" parameter, which will be "month" if you get events for a specific month , so there will also send the parameter "year" with the year in the format "YYYY" and "month" parameter to the desired month in "mm" format. If you want the contrareo only get events a week, the "time" parameter must have a value of "week" , and should be sent as parameter "year" , "month" and " day" that will value the year as month and day of a date which is within the desired week. Or if instead , you want to get the events for a particular day "time" should have a value of " day" and should also send "year" , "month" and "day" which will have the value date of the day desired
```json
{
      time  : day
      year  : 2014
      month : 04
      day   : 20
}
```
As result is obtained a list of events

    ```json
            events: [{
                id: 52f0ed7893888c029200000f,
                calendar: 52f0ed7893888c0292000002,
                date_init: Sun Apr 20 2014 09:00:00 GMT+0200 (CEST),
                date_finish: Thu Mar 20 2014 11:00:00 GMT+0100 (CET),
                name: 'company dinner',
                description: 'This event is company dinner',
                place: 52f0ed7893888c029200000e,
                assistents: [ ],
                created_at: Tue Feb 04 2014 14:39:04 GMT+0100 (CET),
                tags: [ dinner,  enjoy ],
                owner:
                        {
                            id: 52eb667ab71cd7e4be00000c,
                            username: 'a1@appnima.com-1391158906892',
                            mail: 'a1@appnima.com',
                            avatar: 'http://appnima.com/img/avatar.jpg',
                            name: 'name'
                        }

            }]
    ```
#### PUT calendar/event/guest

Another feature that is possible through this resource is to invite a user to an event, so that he too can see the event. Or on the contrary, an invitation to remove that user no longer see the event. To do this, simply run the following function shown below, sending as parameters the "id" of the event, the "id" to invite the user, and "add" or "remove" if you want to add invitation , "add" is sent if instead you want to remove, is sent "remove".

```json
{
      event   : 52f0f4f313255536a8000005
      profile : 52eb667ab71cd7e4be00000c
      state   : add
}
```
If the event does not exist, it returns a 404 error. If on the contrary exists, returns the updated event. The "guest" attribute corresponds to the list of users that have been invited to the event.
```json
    event   : {
                    id: 52f0f4f313255536a8000005,
                    calendar: 52f0f4f213255536a8000002,
                    date_init: Sat Feb 15 2014 16:00:00 GMT+0100 (CET),
                    date_finish: Sat Feb 15 2014 17:00:00 GMT+0100 (CET),
                    name: 'meeting osakidetza updated',
                    description: 'meeting to discuss changes in the implementation',
                    place: 52f0f4f313255536a8000004,
                    assistents: [ ],
                    created_at: Tue Feb 04 2014 15:10:59 GMT+0100 (CET),
                    tags: [ app,  osakidetza ],
                    guest: [ 52eb667ab71cd7e4be000004 ],
                    owner:
                            {
                                id: 52eb667ab71cd7e4be00000c,
                                username: 'a1@appnima.com-1391158906892',
                                mail: 'a1@appnima.com',
                                avatar: 'http://appnima.com/img/avatar.jpg',
                                name: 'name'
                            }
                }
```
#### PUT calendar/event/assistent
To confirm your attendance at an event or delete  is used this resource. Is sent as parameter the "id" of the event, the "id" of the user, and "add" or "remove" parameter. If you want to confirm attendance, "add" is sent if instead you want to remove the confirmation of attendance is sent "remove".
```json
{
      event   : 52f0f4f313255536a8000005
      profile : 52eb667ab71cd7e4be00000c
      state   : add
}
```
If the event does not exist, it returns a 404 error. If on the contrary exists, returns  the updated event. The "assistents" attribute corresponds to the list of users who will attend the event.

```json
    event   : {
                    id: 52f0f84333e9d53db2000005,
                    calendar: 52f0f84233e9d53db2000002,
                    date_init: Sat Feb 15 2014 16:00:00 GMT+0100 (CET),
                    date_finish: Sat Feb 15 2014 17:00:00 GMT+0100 (CET),
                    name: 'meeting osakidetza updated',
                    description: 'meeting to discuss changes in the implementation',
                    place: 52f0f84333e9d53db2000004,
                    assistents: [ 52eb667ab71cd7e4be000004 ],
                    created_at: Tue Feb 04 2014 15:25:07 GMT+0100 (CET),
                    tags: [ app,  osakidetza ],
                    guest: [ 52eb667ab71cd7e4be000004 ],
                    owner:
                            {
                                id: 52eb667ab71cd7e4be00000c,
                                username: 'a1@appnima.com-1391158906892',
                                mail: 'a1@appnima.com',
                                avatar: 'http://appnima.com/img/avatar.jpg',
                                name: 'name'
                            }
                }
```
#### GET calendar/event/search

APP/NIMA allows you to search for events. By using this resource should be sent as a parameter a word, and looks for a match with that word in the name and description of the events that you have access. That is, those who are on a calendar where you're the owner or have you shared those events that you have invited:

```json
{
      query   : "futbol"
}
```

The function returns a list of events satisfying these matches:

```json
    events : [
                {
                    id: 52f0fa9eb70ed01fb9000018,
                    calendar: 52f0fa9eb70ed01fb9000013,
                    date_init: Sat Feb 22 2014 11:00:00 GMT+0100 (CET),
                    date_finish: Sat Feb 22 2014 12:00:00 GMT+0100 (CET),
                    name: 'meeting with juanjo',
                    description: 'meeting with Juanjo in Near',
                    place: 52f0fa9eb70ed01fb9000017,
                    assistents: [ ],
                    created_at: Tue Feb 04 2014 15:35:10 GMT+0100 (CET),
                    tags: [ near ],
                    guest: [ ],
                    owner:
                            {
                                id: 52eb667ab71cd7e4be00000c,
                                username: 'a1@appnima.com-1391158906892',
                                mail: 'a1@appnima.com',
                                avatar: 'http://appnima.com/img/avatar.jpg',
                                name: 'name'
                            }
                }
            ]
```
#### DELETE calendar/event
It is possible to delete an event, you just have to use this facility to send as parameter the "id" of the event to be deleted.
```json
{
      id   : 52f0fa9eb70ed01fb9000018
}
```
If the event does not exist, it returns a 404 error. If on the contrary exists, returns a message that everything gone successfully.
```json
    {message: Successful}
```

#### GET calendar/event/activity

As with a calendar, APP / NIMA also provides us with information on what has happened on a particular event. With this resource by sending as parameter the "id" of an event provides us with a list of activities that have happened in it, such as: they modify that event, invite someone or taking off of the guest list or attendance or poor medical assistance to a user .
```json
{
      id   : 52f0fa9eb70ed01fb9000018
}
```
If the event does not exist, it returns a 404 error. If on the contrary exists,returns a list of activities with the structure shown below
```json
    activities : [
                {
                id: 52f8f6a96946870000000034,
                message: 'Has invited the event to u3net',
                created_at: Mon Feb 10 2014 16:56:25 GMT+0100 (CET),
                profile: {
                           username: 'u3net',
                           name: 'name',
                           mail: 'a3@appnima.com',
                           avatar: 'http://appnima.com/img/avatar.jpg',
                           id: 52eb667ab71cd7e4be000004
                          },
                event: {
                         id: 52f8f6a86946870000000009,
                         calendar: 52f8f6a86946870000000004,
                         date_init: Mon Apr 14 2014 09:00:00 GMT+0200 (CEST),
                         date_finish: Mon Apr 14 2014 11:00:00 GMT+0200 (CEST),
                         name: 'BilboStack updated',
                         description: 'This event is bilboStack',
                         place: 52f8f6a86946870000000008,
                         assistents: [ 52eb667ab71cd7e4be000004 ],
                         created_at: Mon Feb 10 2014 16:56:24 GMT+0100 (CET),
                         tags: [ learn ],
                         guest: [ 52eb667ab71cd7e4be000004, 52eb667ab71cd7e4be000008 ]
                        },
                calendar: {
                            id: 52f8f6a86946870000000004,
                            name: 'Mi calendario updated',
                            color: '#FA58F4',
                            created_at: Mon Feb 10 2014 16:56:24 GMT+0100 (CET),
                            owner: 52eb667ab71cd7e4be00000b,
                            shared: [ ]
                          },
                owner: {
                         id: 52eb667ab71cd7e4be00000c,
                         username: 'u1net',
                         mail: 'a1@appnima.com',
                         avatar: 'http://appnima.com/img/avatar.jpg',
                         name: 'name'
                        }
              }
            ]
```
The event calendar is where the activity was performed. The field "owner" is the person performing the activity and the "profile" field is the person who is targeted activity.

Socket
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
* Allowed user's id array

If everythng is right the room will be created and the author will be automatically connected to it.

#### join
A user can be connected to a room if it is created and he has permission to connect to it. To do this `join` method must be called with the following parameters:

* User's token
* Context (identification of the room to connect)
* Type (Room's type)

#### leave
To disconnect from a room just call the method `leave`.

#### sendMessage
To send a message to a room just call the method `sendMessage` with an object as parameter (a message can be any type of data). This message will be received by all the room's users, also the sender, through a listener called `onMessage` and it will have the following format:
```json
    {
        user:           {"user that sends the message"},
        message:        {"message sent"},
        created_at:     "2013-05-23T12:01:02.736Z"
    }
```

#### broadcastMessage
This methods works as `sendMessage`, the unique difference is that the sender won't receive the message.

#### allowUsers
Users can be added to the allowed user list using `allowUsers` method, to do this just call this method and use users ids array as parameter.

#### disallowUsers
Users can be removed from the allowed user list using `disallowUsers` method, to do this just call this method and use users ids array as parameter.


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
    [{
        id:             "ba54",
        name:           "amigos",
        created_at:     "2013-05-23T12:01:02.736Z"
    },
    {
        id:             "asf9a76y2t3ub",
        name:           "appnima friends",
        created_at:     "2013-02-23T12:01:02.736Z"
    }
    ]
```


Push
----
Use this resource to send push notifications to users devices. To do so, all request have to go to:

    http://api.appnima.com/push/{RESOURCE}

Remember all requests to App/nima should be identified by your `Appnima.key` or key pair `client` and `secret`.

So, the first parameter is the type of request (GET, POST, UPDATE, DELETE …) and the second the name of resource.


#### POST /push
This resource allows you to send push notifications to the user device. Sends the request and the following parameters:
```json
    {
        user:       23094392049024,
        alert:      ""Texto a mostrar en la notificación",
        content":   {"title": "JSON con los campos necesarios",
        "text": "Hola App/nima!"
    }
```
If the notification was successful App/nima returns `200 Ok`.

Payments
--------
Use this resource to make purchases and payments through appnima payment gateway. To do so, all request have to go to:

    http://api.appnima.com/payments/{RESOURCE}

Remember all requests to App/nima should be identified by your `Appnima.key` or key pair `client` and `secret`.

So, the first parameter is the type of request (GET, POST, UPDATE, DELETE …) and the second the name of resource.

### Credit Cards
Credit cards are the most extendend payment system nowadays.To make purchases that involve money first you have to set a payment method to a user. Appnima has his own way to handle credit card information. The way to set a credit card for you application users is very easy.

#### POST /creditcard
To create a credit card for a user you must send the credit card information. Number, cvc (Optional) and expiration date.
```json
    {
         number: "4242424242424242"
         cvc: 123
         expiration_date: "11/2015"
    }
```
If all has gone correctly Appnima will confirm with `200 {id: "credit_card_ID",number: "xxxxxxxxxxxx4242"}` and your user now will have this credit_card information attached to his profile.

You can add optionally a alias parameter such as you can identify easily your cards

```json
    {
         number: "4242424242424242"
         cvc: 123
         expiration_date: "11/2015"
         alias: "my favourite card"
    }
```

#### GET /creditcard
You can attach as many credit cards to a user profile as you want, if you want to check how many credit cards a user you can call credit card without parameters.

And you will receive all the ofuscated credit cards attached to your profile.

#### DELETE /creditcard
You can also can delete credit cards from your profile, the way to do it is sending the id of the credit card you want to erase.

```json
    {
         id: "credit_card_ID"
    }
```
If the credit card information was successfuly deleted App/nima returns `200 Ok`.

#### PUT /creditcard
Also you can modify the values of a credit card attached to a profile, you only have to send the id with the parameters you want to update.
```json
    {
         id: "credit_card_ID",
         number: "4343434343434343"
    }
```
If the credit card information was successfuly updated App/nima returns `200 Ok`.


### Purchases
To make purchases through appnima you can do it in 3 different ways,free purchases, purchases using credit cards as payment method through Stripe and purchases with PayPal.

To choose between this 3 options you only have to change the provider parameter. To make free purchases you have to omit the provider parameter, just dont send it. To choose Stripe you must send provider:0 and to choose PayPal you should choose provider:1.



#### POST /purchase
Purchases without money exchange requirements should be done through purchase option. The purchases of appnima are made in a two step procedure. The way to generate a correct purchase you must follow this sequence. Generation of a purchase and confirmation.

With this method you generate a purchase into appnima,you can choose between 3 posible
Optionally you can send a reference object with the structure you want, just encode it with JSON.stringify method.

```json
    {
        reference: '{ "id":"example id", "content": "example content", "number": 666 }'
    }
```

If the purchase was successfuly created App/nima returns `200 confirmation {token: "purchase_secret_token", amount: 0 "}`.

Lets see how to choose between different providers

##### Create Stripe purchase
To make a purchase through stripe you should send provider parameter set to 0, and the credit card id with the cvc.

```json
    {
        provider     : 0,
        credit_card : credit_card_id,
        cvc         : 123,
        amount      : 600000,
        reference: '{ "id":"example id", "content": "example content", "number": 666 }'
    }
```

If the purchase was successfuly created App/nima returns `200 confirmation {token: "purchase_secret_token", amount: 600000"}`.

##### Create Paypal purchase
To create a paypal purchase you only have to set the provider parameter set to 1 and the amount needed, If you include the reference object with a description, description value will be shown in the Paypal gateway as purchase information.

```json
    {
        provider     : 1,
        amount      : 600000,
        reference: '{ "id":"example id", "description": "Purchase info"}'
    }
```
If the purchase was successfuly created App/nima returns `200 confirmation {token: "purchase_secret_token", amount: 600000"}`.

#### POST /confirm
To make efective the purchase generated in the previous step you must send a confirmation to Appnima with the security token and the amount set to 0


```json
    {
        token: "purchase_secret_token,
        amount: 0
    }
```
If the purchase is confirmed you must receive a `200 purchase {id: "purchase_ID",payed_at: purchase_confirmation_date, state: purchase_state "}`.

##### Confirm Stripe purchase
To confirm a stripe purchasing you must send provider parameter set to 0, purchase token and the amount.

```json
    {
        provider: 0,
        token: "purchase_secret_token,
        amount: 600000
    }
```

#### GET /purchase
Get purchase without sending any parameters will show you all your profile purchases


#### GET /purchase/search
Search method will find you in your profile purchases the one who match with reference parameters. For example assuming your purchases have a reference.id and a content your search should be like this.

```json
    {
        id: "example id",
        content: "example content"
    }
```
This method will return you all the purchases that match with te given reference parameters. Remind that this methos returns all type of purchases free and not free ones.




































