USER
====

Este módulo recoge toda la funcionalidad para incluir un usuario de tu proyecto dentro la plataforma App/nima. Para ello ten en cuenta que todas las peticiones que hagas tendrán que ir a:

    http://appnima.com/{RECURSO}

Recuerda que todas las peticiones que hagas a App/nima tienen que ir identificadas con tu `Appnima.key` o bien con el par de datos `client` y `secret`. Ahora veamos los recursos que puedes utilizar, para ello el primer parametro indica el tipo de petición (GET, POST, UPDATE, DELETE …) y el segundo parametro el nombre del recurso.


## POST /user/signup
--------------------
Todos los usuarios de tu aplicación tienen que ser usuarios App/nima y por lo tanto lo primero que tendrás que hacer es registrarlos en la plataforma para así poder obtener su token. Para ello debes enviar los siguientes parametros:

```json
    {
        mail:       "javi@tapquo.com",
        password:   "USER_PASSWORD"
    }
```

Y opcionalmente también puedes enviar:

```json
    {
        ...
        username:   "soyjavi",
        name:       "Javi Jimenez",
        avatar:     "http://USER_AVATAR_URL"
    }
```

Si ha ido todo bien retorna un `201 Created` junto con el objeto:

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
Una vez que tenemos un usuario registrado para tu aplicación ahora tienes que pedir el token Oauth 2 que a partir de ahora será la clave para hacer todas las peticiones a App/nima. Para ello debes enviar los siguientes parámetros con la cabecera "http authorization basic"(con el client_id:client_secret codificados en Base64):

```Authorization: basic client_id:client_secret```

```json
    {
        grant_type: "password",
        mail:       "javi@tapquo.com",
        password:   "USER_PASSWORD"
    }
```
Si ha ido todo bien retorna un `201 Created` junto con el objeto:

```json
    {
        token_type:      "bearer",
        refresh_token:   "n72c03ty202ugx2gu2u",
        access_token:    "eh024hg02g2onvev29"
    }
```

## POST /user/login
-------------------
Cada vez que quieras validar si el usuario tiene permisos para acceder a tu aplicación podrás usar este recurso. Únicamente necesita los parámetros:

```json
    {
        mail:       "javi@tapquo.com",
        password:   "USER_PASSWORD"
    }
```

En caso de que la validación haya sido correcta App/nima devolver un `200 Ok` con los datos del usuario


## GET /user/info
-----------------
Si necesitas obtener los datos del usuario debes utilizar este recurso y como estás utilizando el protocolo de autentificación OAuth 2 no es necesario que envies ningún parámetro. Solo deberás esperar a la respuesta `200 Ok` con los siguientes parámetros:

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
Este recurso sirve para modificar los datos personales de un usuario dentro de tu aplicación, al igual que en el recurso **GET /user/info** no es necesario identificar al usuario por parámetro. Puedes enviar todos los parámetros que aparecen a continuación (aunque no es obligatorio enviarlos todos):

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

En el caso de que haya ido todo bien se devolverá el código `200 OK` junto con el mismo objeto **GET /user**. En el caso de que el usuario no tenga permiso para modificar sus datos App/nima devolverá un `403 Forbidden`.

## POST /user/avatar
----------------------
Este recurso sirve para subir un avatar, para ello enviaremos los siguientes parámetros:


```json
    {
        avatar:       "dhsgaohgoiagangaogisah89t2h3ugb2g2b",    /* avatar data coded in base 64 */
    }
```

En el caso de que haya ido todo bien se devolverá el código `201 RESOURCE CREATED`.

## POST /user/terminal
----------------------
Este recurso sirve para que en el caso de que necesites registrar el terminal con el que se está accediendo a tu aplicación. Para ello junto con la petición tienes que enviar los parámetros:


```json
    {
        type:       "phone",    /* computer, tablet, phone, tv */
        os:         "ios",      /* windows, macos, linux, ios, android, blackberry, firefoxos, windowsphone, other */
        version:    "6.0"       /* ?.? */
    }
```

En el caso de que haya ido todo bien se devolverá el código `201 RESOURCE CREATED`.


## GET /user/terminal
----------------------
Si necesitas saber los terminales desde los que se ha accedido a tu aplicación simplemente tienes que utilizar este recurso y al igual que **GET /user/info** no hace falta que envies ningún parámetro. La respuesta si ha ido todo correctamente será un `200 Ok` junto con los parámetros:

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
Este recurso sirve para que en el caso de que necesites actualizar un terminal con el que se está accediendo a tu aplicación. Para ello junto con la petición tienes que enviar los parámetros:


```json
    {
        terminal:   "5719721071057"
        type:       "phone",    /* computer, tablet, phone, tv */
        os:         "ios",      /* windows, macos, linux, ios, android, blackberry, firefoxos, windowsphone, other */
        version:    "6.0"       /* ?.? */
    }
```

Si un usuario quiere darse de baja de tu aplicación tendrás que hacer uso de este recurso.

## POST /user/subscription
--------------
Registra los e-mail de los usuarios que desean recibir información o invitación de tu aplicación. Solo necesitas pasar como parámetro la dirección e-mail:

```json
    {
		mail:		"javi@tapquo.com"
	}
```

Si el parámetro es correcto se recibe un `200 Ok` junto con el objeto:

```json
    {
		message:	'Request accepted.'
	}
```

## POST /user/ticket
--------------
Utiliza este recurso como sistema de gestión de tickets para la resolución de las consultas e incidencias de tus usuarios. Envía como parámetro junto a la petición el texto de la consulta:

```json
    {
		question:	'[SUGGESTION] Bigger buttons'
	}
```
