USER
====

Use this module to add users of your project within App/nima platform. To do so, all request have to go to:

    http://appnima-user.eu01.aws.af.cm/{RESOURCE}

Remember all requests to App/nima should be identified by your `Appnima.key` or key pair `client` and `secret`.

So, the first parameter is the type of request (GET, POST, UPDATE, DELETE …) and the second the name of resource.


## POST /user/signup
--------------------
All users of your application must be App/nima users. So the first step is register the user to get his token. Sends the request with the next parameters:

```json
    {
        mail:       "javi@tapquo.com",
        password:   "USER_PASSWORD"
    }
```

You can send additional fields like:

```json
    {
        ...
        username:   "soyjavi",
        name:       "Javi Jimenez",
        avatar:     "http://USER_AVATAR_URL"
    }
```
Responses are returned with `201 Created` and the object: 

```json
    {
        id:         "939349943434",
        mail:       "javi@tapquo.com",
        username:   "soyjavi",
        name:       "Javi Jimenez",
        avatar:     "http://USER_AVATAR_URL"
    }
```


## POST /oauth2/token
---------------------
The second step is get the token Oath 2 authentication. It will be the key for all requests into App/nima. So, send the following parameters within the header "http basic authorization"(client_id:client_secret Base64 encoded):

```Authorization: basic client_id:client_secret```

```json
    {
        grant_type: "password",
        mail:       "javi@tapquo.com",
        password:   "USER_PASSWORD"
    }
```

Responses are returned with `201 Created` and the object:

```json
    {
        token_type:      "bearer",
        refresh_token:   "n72c03ty202ugx2gu2u",
        access_token:    "eh024hg02g2onvev29"
    }
```


## POST /user/login
-------------------
Use this resource to find out user permissions into your application. Sends the request with the next parameters:

```json
    {
        mail:       "javi@tapquo.com",
        password:   "USER_PASSWORD"
    }
```

If the validation was successful App/nima returns `200 Ok` and the user data.


## GET /user/info
-----------------

Get user data with this resource. Due to Oath 2 Authentication you do not need any parameter, just wait the response `200 Ok` and the object:


```json
    {
        _id:            28319319832
        mail:           "javi@tapquo.com",
        username:       "soyjavi",
        name:           "Javi Jimenez",
        avatar:         "http://USER_AVATAR_URL",
        bio:            "Founder & CTO at @tapquo",
        phone:          "PHONE_NUMBER",
        token:          "USER_TOKEN",
        refresh_token:  "REFRESH_TOKEN"
    }
```

## PUT /user/info
-----------------
Use this resource to update user data profile. Like **GET /user/info** you do not need identified the user. Just send the parameters you want modify: 

```json
    {
        mail:       "javi@tapquo.com",
        password:   "PASSWORD",
        username:   "soyjavi",
        name:       "Javi Jimenez",
        avatar:     "http://USER_AVATAR_URL",
        bio:        "Founder & CTO at @tapquo",
        phone:      "PHONE_NUMBER"
    }
```

If the request was successful App/nima returns `200 Ok` and the same object **GET /user**. If the user has not permission to modify his data App/nima returns `403 Forbidden`.

## POST /user/avatar
----------------------
Upload user avatar with this resource. Sends the request and the following parameters:


```json
    {
        avatar:       "dhsgaohgoiagangaogisah89t2h3ugb2g2b",    /* avatar data coded in base 64 */
    }
```

Responses are returned with `201 RESOURCE CREATED`.


## POST /user/terminal
----------------------
This resource allows you register the user device. Sends the request and the following parameters:

```json
    {
        type:       "phone",    /* computer, tablet, phone, tv */
        os:         "ios",      /* windows, macos, linux, ios, android, blackberry, firefoxos, windowsphone, other */
        version:    "6.0"       /* ?.? */
    }
```

Responses are returned with `201 RESOURCE CREATED`.




## GET /user/terminal
----------------------
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

## PUT /user/terminal
----------------------
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


## POST /user/subscription
--------------
Registered e-mail of users who want to receive information from your application or who want be invited to your application. You just need send like parameter the e-mail address: 

```json
    {
		mail:		"javi@tapquo.com"
	}
```

Responses are returned with `200 Ok` and the object:

```json
    {
		message:	'Request accepted.'
	}
```
