App/nima
========
Descubre como dar alma a tus aplicaciones con el primer LaaS en el mundo.

*Version Actual: [1.0.0]()*


Introducción
------------
Simplemente por leer este documento declara que eres un desarrollador que quiere mejorar constantemente y quiere crear proyectos cada vez más eficientes. App/nima es la primera plataforma que ofrece servicios lógicos para cualquier tipo de proyecto, da igual si quieres crear una aplicación o un site, App/nima te ayudará en ambas situaciones.

Un poco de historia, hace ya casi 3 en [**Tapquo**](http://tapquo.com) nos encontramos con un problema común en el mundo del desarrollo y no era más que cada nuevo producto que creabamos debiamos repetir una y otra vez la misma funcionalidad básica. App/nima surgio de la necesidad de ser cada vez más eficientes y de querer desarrollar únicamente el negocio implicito de nuestro nuevo producto y no tanto de la lógica horizontal:

+ Servicio OAuth 2 para la autentificación
+ Gestión de usuarios
+ Red social de usuarios
+ Servicio de mensajeria para enviar emails, SMS, llamadas y mensajes privados
+ Localizar/Geoposicionar lugares
+ Localizar/Geoposicionar a otros usuarios
+ Tener Real-time por medio de sockets
+ Notificaciones Push para las plataformas: iOS, Android o Blackberry
+ Saber como se comportan nuestros usuarios

Si por lo menos alguna vez has tenido que desarrollar alguna de estas funcionalidades en alguno de tus proyectos, eres bienvenido a nuestra plataforma ya que App/nima esta pensada para ayudarte como desarrollador. Queremos ofrecerte una plataforma centrada en facilitarte la vida, queremos el software tanto como tu y buscamos ser cada día mejores. Usa App/nima.


### Arquitectura
Seguramente en este momento tendrás dudas de como App/nima puede ayudarte a ser más eficiente pero tranquilo vamos a explicarte cómo funciona. App/nima esta basada por completo en el protocolo de autentificación [**Oauth 2**](http://oauth.net/2/) y en la serialización mediante REST-style con objetos JSON. Si nunca has utilizado este paradigma no te preocupes te explicarémos paso a paso como conectarte a la plataforma y te ofreceremos herramientas específicas como [**AppnimaJS**](http://) que te facilitarán el proceso de conexión.

Todo el backend está montado a lo largo del mundo utilizando plataformas como Amazon o Google Cloud Engine las cuales nos dan la capacidad de ofrecer siempre la mejor calidad de respuesta para nuestros usuarios. No te preocupes no tendrás que ser un *Jedy SysAdmin* ya que tu unicamente te tienes que comunicar con nuestro REST la tarea de escalabilidad corre de nuestra cuenta, tu preocupate en crear el mejor proyecto y nosotros de ofrecerte cada día una mejor plataforma.

Resumiendo App/nima es una plataforma en forma de API REST que te provee de los servicios logicos de tu proyecto, y dependiendo de la naturaleza de tu proyecto puede que no necesites ni tu propio Backend. Cada servicio lógico esta alojado en servidores diferentes para obtener la mayor escalabilidad independiente, a lo largo de la documentación conoceremos las rutas de cada servicio.


### Oauth 2
Todos los servicios de App/nima utilizan el protocolo  de autentificación [**OAuth 2**](http://oauth.net/2/) buscando la mayor compatibilidad con herramientas de terceros. Si no eres un experto en OAuth 2 te recomendamos que lo estudies ya que esta siendo a llamar el protocolo de autentificación por excelencia y empresas como Google, Facebook o Twitter lo utilizan para conectarse a sus APIs. Lee la [documentación](http://) para comenzar hoy mismo con App/nima.


### No XML, solo JSON
Sólo permitimos la serialización de datos tipo JSON. Nuestro formato es no tener ningún elemento raíz y utilizar *snake_case* para describir las claves de atributos. Esto significa que en cada petición a App/nima tienes que enviar el sufijo:

    Content-Type: application / json; charset = utf-8

Recibirás una respuesta 415 *Unsupported Media Type* si intentas utilizar otro tipo de sufijo URL.


### Usa el HTTP cache
Tienes que hacer uso de las cabeceras HTTP *freshness* para disminuir la carga de nuestros servidores (¡y aumentar la velocidad de tu aplicación!). La mayoría de las peticiones que devolvemos incluirán un *ETag* o una cabecera *Last-Modified*. Al solicitar por primera vez un recurso, almacena este valor y envianoslo en las posteriores peticiones como *If-None-Match* y *If-Modified-Since*. Si el recurso no ha cambiado, obtendrás un código 304 *Not Modified* como respuesta, que te ahorra tiempo y ancho de banda (porque ya tienes ese recurso).


### Manejando errores
Si App/nima tiene algun problema, es posible que veas un error 5xx. 500 significa que la aplicación esta totalmente caída, pero tambien puedes ver un 502 *Bad Gateway* (entrada erronea), 503 *Service Unavailable* (Servicio no disponible), o un 504 *Gateway Timeout* (límite de tiempo de espera). En todos estos casos es tu responsabilidad reintentar la petición más tarde.


Clientes
--------

Ayudanos
--------
Por favor no dudes en ponerte en contacto con nosotros si crees que puedes hacer una mejor API. Si crees que deberíamos soportar una nueva funcionalidad o si has encontrado un bug, usa GitHub issues. Haz un fork de esta documentación y mandanos tus *pulls* con las mejoras.

Para hablar con nosotros o con otros desarrolladores sobre la API, suscribete a nuestra [**lista de correo**](https://groups.google.com/forum/#!forum/appnima).


API REST
========
Autentificación
---------------
Este módulo recoge toda la funcionalidad para obtener el token para un usuario a través del sistema denominado Oauth de 2 pasos, la ruta del recurso es:

    http://appnima.com/{RECURSO}

A continuación se explica el proceso de obtención de token a través de este sistema.


#### Paso 1: GET /oauth/authorize
Se redirigirá al usuario a una url previamente mencionada y a este recurso, pasando como parámetros:
```json
`response_type` : El tipo de solicitud, deberá ser "CODE"
`client_id`     : El identificador público de tu aplicación
`scope`         : Los permisos que tendrá el token
`redirect_uri`  : La url de redirección. Será la misma que diste de alta en tu aplicación.
`state`         : Variable de estado que acompañará a la respuesta para que se pueda identificar la operación.
```

La url sería algo similar a esto:

    http://api.appnima.com/oauth2/authorize?scope=profile,push&response_type=code&client_id=519b84f0c1881dc1b3000002&redirect_uri=http://myapp.com&state=user48

El usuario verá una página en la que se le presentan los datos de la aplicación y los permisos que requiere, si lo rechaza o algo va mal, se redirigirá al usuario a la página con un campo `error` que especifica el error.

Si ha ido todo bien se redirigirá a la página con un campo `code` que especifica un código con el que continuar el proceso.

#### Paso 2: POST /oauth2/token
Una vez tengamos el `code` se solicitará el token. Para ello se llamará al recurso /oauth2/token a través de una petición POST con la cabecera "http basic authorization" con el key de appnima y los siguientes parámetros:
```header
    {
        Authorization: "basic APPNIMA-KEY"
    }
```

```json
    {
        grant_type: "code",
        code:       "h02h40g208ghop2envg2h9h2epne2eh092he0b2",
        client_id:  "532023h2308h2839h"
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
#### Refresh Token: POST /oauth2/token
Cuando se trata de acceder a un recurso y este devuelve un código de error `480` significa que el token ha expirado y que hay que refrescarlo, para ello usa este método que renueva tanto el `access_token` como el `refresh_token`. Para ello envía la cabecera "http basic authorization" con el key de appnima y el parámetro `refresh_token` actual:
```header
    {
        Authorization: "basic APPNIMA-KEY"
    }
```


```json
    {
        refresh_token:       "n72c03ty202ugx2gu2u"
    }
```

Si todo ha salido bien App/nima devuelve el siguiente objeto:

```json
    {
        access_token:       'cd776kk02g2ata629',
        expires_in:         '2013-08-06T06:58:37.298Z',
        refresh_token:      'kuo54jk02g9lmoovp9',
        scope:              [ 'profile' ]
    }
```


USER
----
Este módulo recoge toda la funcionalidad para incluir un usuario de tu proyecto dentro la plataforma App/nima. Para ello ten en cuenta que todas las peticiones que hagas tendrán que ir a:

    http://api.appnima.com/user/{RECURSO}

Recuerda que todas las peticiones que hagas a App/nima tienen que ir identificadas con tu `Appnima.key` o bien con el par de datos `client` y `secret`. Ahora veamos los recursos que puedes utilizar, para ello el primer parametro indica el tipo de petición (GET, POST, UPDATE, DELETE …) y el segundo parametro el nombre del recurso.


### Seguridad
#### POST /signup
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

#### POST /token
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

#### POST /login
Cada vez que quieras validar si el usuario tiene permisos para acceder a tu aplicación podrás usar este recurso. Únicamente necesita los parámetros:
```json
    {
        mail:       "javi@tapquo.com",
        password:   "USER_PASSWORD"
    }
```

En caso de que la validación haya sido correcta App/nima devolver un `200 Ok` con los datos del usuario


### Info
#### GET /info
Si necesitas obtener los datos del usuario de tu sesión debes utilizar este recurso y como estás utilizando el protocolo de autentificación OAuth 2 no es necesario que envíes ningún parámetro a no ser der que desees obtener la información de cualquier usuario de App/nima.

En ese caso solo tendrás que enviar la id de dicho usuario junto con la llamada al método.

En ambos casos, solo deberás esperar a la respuesta `200 Ok` y APP/NIMA te devuelve los siguientes parámetros:
```json
    {
        id:            28319319832
        mail:          "javi@tapquo.com",
        username:      "soyjavi",
        name:          "Javi Jimenez",
        avatar:        "http://USER_AVATAR_URL",
        language:       "spanish",
        country:        "ES",
        bio:           "Founder & CTO at @tapquo",
        phone:         "PHONE_NUMBER",
        site:           "http://USER_URL"
    }
```

#### PUT /update
Este recurso sirve para modificar los datos personales de un usuario dentro de tu aplicación, al igual que en el recurso **GET /user/info** no es necesario identificar al usuario por parámetro. Puedes enviar todos los parámetros que aparecen a continuación (aunque no es obligatorio enviarlos todos):
```json
    {
        mail:       "javi@tapquo.com",
        username:   "soyjavi",
        name:       "Javi Jimenez",
        avatar:     "http://USER_AVATAR_URL",
        picture:    "http://USER_PICTURE_URL",
        bio:        "Founder & CTO at @tapquo",
        phone:      "PHONE_NUMBER"
    }
```

En el caso de que haya ido todo bien se devolverá el código `200 OK` junto con el mismo objeto **GET /user**. En el caso de que el usuario no tenga permiso para modificar sus datos App/nima devolverá un `403 Forbidden`.


#### POST /password
Este recurso sirve para cambiar la contraseña, para ello enviaremos los siguientes parámetros:
```json
    {
        old_password:       "old_password",
        new_password:       "new_password"
    }
```


#### POST /avatar
Este recurso sirve para subir un avatar, para ello enviaremos los siguientes parámetros:
```json
    {
        avatar:       "dhsgaohgoiagangaogisah89t2h3ugb2g2b",    /* avatar data coded in base 64 */
    }
```

En el caso de que haya ido todo bien se devolverá el código `201 RESOURCE CREATED`.

### Post (Mensaje)

#### POST/post
Un post se trata de un mensaje público y el usuario con este recurso puede crearlo. Para ello debe enviar los parámetros junto con la petición:

```json
    {
        title: "Lorem Ipsum",
        content:  "Lorem ipsum dolor sit amet, consectetur adipisicing elit.",
        image: "http://IMAGE_URL
    }
```
El único campo obligatorio a la hora de crear un post es el ```content``` que se trata del contenido del mensaje.

Si va todo bien, solo deberás esperar a la respuesta `200 Ok` y APP/NIMA te devuelve los siguientes parámetros:
```json
    {
        _id:            28319319832
        application:    34246895433,
        content:        "Lorem ipsum dolor sit amet, consectetur adipisicing elit.",
        title:          "Lorem ipsum",
        create_at:      "2013-12-02 08:00:58.784Z"
        image:          "http://IMAGE_URL",
        owner:          {
        _id:        "57592807235"
        avatar:      "http://AVATAR_URL",
        created_at:   "2013-12-02 08:00:58.784Z",
        mail:      "soyjavi@tapquo.com",
        name:    "javi",
        username:    "soyjavi"
    }
   }
```

#### POST/put
Este recurso sirve para modificar un post creado anteriormente. Para ello, el usuario debe enviar los parámetros junto con la petición:

```json
    {
        id: POST_ID,
        title: "Lorem Ipsum",
        content:  "Lorem ipsum dolor sit amet, consectetur adipisicing elit.",
        image: "http://IMAGE_URL
    }
```

Si va todo bien, solo deberás esperar a la respuesta `200 Ok` y APP/NIMA te devuelve los mismos parámetros que en el `POST`.

#### GET/post
Este recurso sirve para obtener un post concreto. Para ello, el usuario solamente debe enviar la ```id```del post que desea obtener y, si va todo bien, APP/NIMA devolverá la respuesta `200 OK`y el post concreto del mismo estilo que en el `POST`.

#### GET/post/info
Este recurso sirve para obtener el contador de los post del usuario. Si deseas obtener el contador de tus propios post, no debes enviarle ningún parámetro, pero en cambio si lo que deseas obtener es el contador de post de otro usuario, tienes que enviarle la *id* de dicho usuario junto con la llamada.

```json
    {
        user: 4231312321312323
    }
```

### Favoritos
#### POST /like/post
Este recurso sirve para hacer favorito un post en concreto o para quitar un favorito ya hecho. Para ello, solo hay que enviar el *id* de dicho post.
```
    json{
            post: 87438957439857439853
        }
```

Si es la primera vez que marca como favorito, APP/NIMA devolverá la respuesta `200 OK`. En cambio, si ya había marcado como favorito antes, se borrará dicho favorito y APP/NIMA devolverá un mensaje: *unliked*.

#### GET /like/post
Este recurso, si todo va bien, devolverá la lista de los *post* a los que el usuario logueado ha marcado como favorito. Para ello, no hace falta enviar nada.

#### GET /post/likers
Este recurso sirve para obtener todos los usuarios que han hecho favorito a un *post* en concreto. Para ello, solamente hay que enviar la *id* de dicho post.
```
    json{
            post: 87438957439857439853
        }
```

### Timeline
#### GET/timeline
Este recurso sirve para obtener la lista de posts de un determinado usuario. Si lo que deseas es obtener los posts de tu usuario de la sesión no tienes que enviar ningún parámetro junto con la petición. Te devolverá la lista de posts tanto tuyos como de los usuarios a los que sigues (following) ordenados del más antiguo al más reciente.

En cambio, si lo que deseas es obtener los posts de otro usuario o solamente los tuyos propios, debes enviar los siguientes parámetros:
```json
    { id: 4234324432432}
```
Se trata de la id del usuario del que quieres obtener los post. En este caso, unicamente te devolverá la lista de los post que ha creado ese usuario.

Si va todo bien, solo deberás esperar a la respuesta `200 OK`y la lista de posts que te devolverá APP/NIMA.

También existe la opción de que te devuelva la lista de post con paginación; esto es, que en cada llamada a la API te vaya devolviendo parte de la lista de posts cronologicamente.

Para ello, debes enviar los siguientes parámetros:
```json
    { page: 0,
      num_results: 5
      last_data: "2013-12-02 08:00:58.784Z"
    }
```
A ese objeto se le debe añadir, si se desea, lo explicado anteriormente de la *id* del usuario.

La variable *page* se trata del número de página que deseas obtener; esto es, el trozo de la lista de post que deseas. *num_results* es el numero de resultados que quieres obtener. En la primera llamada, esa variable será multiplicada por 2, y en los demás casos, se devulverá dicha cifra de posts. Por último, la variable *last_data* se trata de la fecha de creación del último post recibido en la última llamada realizada. Es importante esta fecha ya que será el punto de comienzo de la siguiente tanda de posts.

### Comment
#### POST/comment
Este recurso sirve para crear comentarios sobre un `post`. La idea de este recurso es que se puedan crear discusiones sobre los post. Para crear un comentario hay que enviar los siguientes parámetros:
```json
    { text: "Lorem ipsum"
      post: 2131434543543
      message: 4325436457645
    }
```

#### GET/post/comment
Este recurso sirve para obtener todos los comentarios de un post. Solamente hay que enviar la id de dicho *post* para que éste te devuelva la lista de los comentarios.

### Terminal
#### POST /terminal
Este recurso sirve para que en el caso de que necesites registrar el terminal con el que se está accediendo a tu aplicación. Para ello junto con la petición tienes que enviar los parámetros:
```json
    {
        type:       "phone",    /* computer, tablet, phone, tv */
        os:         "ios",      /* windows, macos, linux, ios, android, blackberry, firefoxos, windowsphone, other */
        version:    "6.0"       /* ?.? */
    }
```

En el caso de que haya ido todo bien se devolverá el código `201 RESOURCE CREATED`.


#### GET /terminal
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

#### PUT /terminal
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


### Suscripciones
#### POST /subscription
Registra los e-mail de los usuarios que desean recibir información o invitación de tu aplicación. Solo necesitas pasar como parámetro la dirección e-mail:
```json
    {
        mail:       "javi@tapquo.com"
    }
```

Si el parámetro es correcto se recibe un `200 Ok` junto con el objeto:
```json
    {
        message:    'Request accepted.'
    }
```

### Soporte
#### POST /ticket
Utiliza este recurso como sistema de gestión de tickets para la resolución de las consultas e incidencias de tus usuarios. Envía como parámetro junto a la petición el texto de la consulta:
```json
    {
        question:   '[SUGGESTION] Bigger buttons'
    }
```


Network
-------
Este modulo recoge toda la funcionalidad para crear una red social dentro de tu aplicación; buscar usuarios, seguirlos (o no seguirlos, tu decides), listas de seguidores... Para ello ten en cuenta que todas las peticiones que hagas tendrán que ir a:

    http://api.appnima.com/network/{RECURSO}

Recuerda que todas las peticiones que hagas a App/nima tienen que ir identificadas con tu `Appnima.key` o bien con el par de datos `client` y `secret`. Ahora veamos los recursos que puedes utilizar, para ello el primer parámetro indica el tipo de petición (GET, POST, UPDATE, DELETE …) y el segundo parámetro el nombre del recurso.


### Relaciones
#### GET /search
Si necesitas buscar usuarios dentro de tu aplicación debes utilizar este recurso el cual únicamente recibe un único parámetro y con el cual App/nima hará todo el trabajo difícil buscando los mejores resultados:
```json
    {
        query:      "javi@tapquo.com"
    }
```

En el caso de que la respuesta haya sido satisfactoria se devolverá un `200 Ok` junto con una lista de usuarios coincidentes a esa búsqueda:
```json
    [{
        id:          120949303434,
        username:    "soyjavi",
        name:        "Javi",
        avatar:      "AVATAR_URL",
        follower:    true,
        following:   false
    },
    {
        id:         120949303433,
        username:   "cataflu",
        name:       "Catalina",
        avatar:     "AVATAR_URL",
        follower:   false,
        following:  false
    },
    {
        id:         120949303431,
        username:   "haas85",
        name:       "Iñigo",
        avatar:     "AVATAR_URL",
        follower:   true,
        following:  true
    }
    ]
```

La variable *following* devuelve si el usuario logueado sigue a esa persona, mientras que la variable *follower* devuelve si dicho usuario sigue a la persona logueada.

#### POST /follow
Si necesitas seguir a un usuario utiliza este recurso junto con el parámetro:
```json
    {
        user:       23094392049024
    }
```

Devolverá un `200 Ok` junto con el objeto:
```json
    {
        status:     'ok'
    }
```


#### POST /unfollow
Al igual que el recurso **POST /follow** funciona de igual manera y únicamente tendrás que enviar el parámetro:
```json
    {
        user:       23094392049024
    }
```

Devolverá un `200 Ok` junto con el objeto:
```json
    {
        status:     'ok'
    }
```

#### GET /following
Funciona de igual manera que **POST /follow**  y únicamente tendrás que enviar como parámetro el usuario del que quieres conocer la lista de usuarios que esta siguiendo:
```json
    {
        user:       23094392049024
    }
```

Devolverá un `200 Ok` junto con lista de usuarios que sigue el usuario indicado:
```json
    [{
        id:         120949303434,
        username:   "soyjavi",
        name:       "Javi",
        avatar:     "AVATAR_URL"
    },
    {
        id:         120949303433,
        username:   "cataflu",
        name:       "Catalina",
        avatar:     "AVATAR_URL"
    },
    {
        id:         120949303431,
        username:   "haas85",
        name:       "Iñigo",
        avatar:     "AVATAR_URL"
    }
    ]
```

Al igual que en el *timeline*, existe la opción de obtener estos datos mediante paginación. Para ello, simplemente tienes que añadir los siguientes parámetros a tu llamada:

```json
    {
     user: 543534534534534534
     page: 0
     num_results: 5
    }
```

El significado de las variables *page* y *num_results* es el mismo que en el caso de la llamada al **Timeline**. La única diferencia de ambas llamadas es que en este caso no hace falta enviar la variable de la última fecha del último dato obtenido.

#### GET /followers
Funciona de igual manera que **GET /following**  y esta vez tendrás que enviar como parámetro el usuario del que quieres conocer la lista de usuarios que le están siguiendo:
```json
    {
        user:       23094392049024
    }
```

Devolverá un `200 Ok` junto con lista de usuarios que siguen al usuario indicado:
```json
    [{
        id:         120949303434,
        username:   "soyjavi",
        name:       "Javi",
        avatar:     "AVATAR_URL",
        is_follow:   true
    },
    {
        id:         120949303433,
        username:   "cataflu",
        name:       "Catalina",
        avatar:     "AVATAR_URL",
        is_follow:   false
    },
    {
        id:         120949303431,
        username:   "haas85",
        name:       "Iñigo",
        avatar:     "AVATAR_URL",
        is_follow:   false
    }
    ]
```

Al igual que lo que se ha explicado anteriormente en **GET/ followings**, también existe la opción de obtener los datos mediante paginación. La dinámica es la misma.

Como se puede observar, en este caso, la llamada devuelve una variable más en cada objeto. Dicha variable indica si el usuario logueado sigue a esa persona o no.

### Estadísticas
#### GET /info
Si quieres tener una visión general de un determinado usuario dentro de la red social de tu aplicación utiliza este recurso, que al igual que en los recursos anteriores solo necesita del id del usuario como parámetro:
```json
    {
        user:       23094392049024
    }
```

Devolverá un `200 Ok` junto con los totales de *followers* y *followings* que tiene el usuario indicado y la lista de ambos:
```json
    {
        following:
            users: [
                {name: javi
                 username: soyjavi
                 bio: Lorem ipsum
                 mail: soyjavi@tapquo.com
                }
                ]
            count: 1,
        followers:
            users: []
            count: 0
    }
```

#### GET /check
Este recurso sirve para saber la relación que tienes con un determinado usuario (tal vez te interese seguirlo o no) para ello tenemos que enviar el id del usuario a consultar de la siguiente manera:
```json
    {
        user:       23094392049024
    }
```

En el caso de que todo haya ido correctamente devolverá un `200 Ok` junto con el objeto que representa la relación que tiene el usuario consultado con el usuario de tu aplicación (TOKEN):
```json
    {
        following:  true,
        follower:   false
    }
```


Messenger
---------
Este módulo recoge toda la funcionalidad de mensajería: enviar e-mail, SMS y mensajes privados entre usuarios de tu misma aplicación.

Para ello ten en cuenta que todas las peticiones que hagas tendrán que ir a:

    http://api.appnima.com/messenger/{RECURSO}

Recuerda que todas las peticiones que hagas a App/nima tienen que ir identificadas con tu `Appnima.key` o bien con el par de datos `client` y `secret`. Ahora veamos los recursos que puedes utilizar, para ello el primer parámetro indica el tipo de petición (GET, POST, UPDATE, DELETE …) y el segundo parámetro el nombre del recurso.


### Mail
#### POST /mail
Con este recurso los usuarios de tu aplicación podrán enviar e-mails. Para ello debes pasar los siguientes parámentros (el campo subject es opcional):
```json
    {
        user:       23094392049024,
        subject:    "Appnima.com",
        message:    "Welcome to appnima.messenger [MAIL]"
    }
```

Si todo ha salido bien, devolverá un `201 Created` junto con el objeto:
```json
    {
        message:    'E-mail sent successfully.'
    }
```

### SMS
#### POST /sms
Con este recurso tu aplicación podrá enviar mensajes de texto a los dispositivos móviles registrados de tus usuarios. Tan solo debes enviar junto con la petición los parámetros:
```json
    {
        user:       23094392049024,
        message:    "Welcome to appnima.messenger [SMS]"
    }
```

En el caso de que la respuesta haya sido satisfactoria se devolverá un `201 Created` junto con el objeto:
```json
    {
        message:    'SMS sent successfully.'
    }
```

### Message
#### POST /message
Si lo necesitas, Appnima te provee de un sistema de mensajería interno entre los usuarios de tu aplicación. Los parámetros que necesita la petición son:
```json
    {
        user:       23094392049024,
        subject:    "Appnima.com",
        message:    "Welcome to appnima.messenger [MESSAGE]"
    }
```

El campo subject es opcional y si la petición ha salido bien se devolverá un `201 Created` junto con el objeto:
```json
    {
        message:    'Message sent successfully.'
    }
```


#### GET /message/outbox
Los usuarios de tu aplicación pueden recuperar los mensajes enviados. Para ello basta con llamar al recurso pasando como parámetro outbox.
```json
    {
        context:    outbox
    }
```

Si todo ha salido bien, devolverá un `200 Ok` junto con lista de mensajes de la bandeja indicada:
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
Los usuarios de tu aplicación pueden recuperar los mensajes recibidos. Para ello basta con llamar al recurso pasando como parámetro inbox.

```json
    {
        context:    inbox
    }
```

Si todo ha salido bien, devolverá un `200 Ok` junto con lista de mensajes de la bandeja indicada:

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
Para que se refleje que el usuario ha leído un mensaje o que desea eliminarlo de su sistema, basta con enviar en la petición los siguientes parámetros:
```json
    {
        message:    23094392049024,
        state:      "READ" /*READ o DELETED*/
    }
```

Si todo ha salido bien, devolverá un `200 Ok`junto con el mensaje de confirmación:
```json
    {
        message:            "Resource READ."
    }
```


Location
--------
Este módulo te permite obtener toda la funcionalidad respecto a la geolocalización de usuarios y lugares. Mediante latitud y longitud obtén los sitios en un radio determinado así como los detalles de un lugar concreto. Si tu aplicación lo requiere, también puedes registrar un sitio mediante checkins y además ofrecer a tus usuarios información sobre amigos cercanos.

Para ello ten en cuenta que todas las peticiones que hagas tendrán que ir a:

    http://api.appnima.com/location/{RECURSO}

Recuerda que todas las peticiones que hagas a App/nima tienen que ir identificadas con tu `Appnima.key` o bien con el par de datos `client` y `secret`. Ahora veamos los recursos que puedes utilizar: el primer parámetro indica el tipo de petición (GET, POST, UPDATE, DELETE …) y el segundo el nombre del recurso.

### Places
#### GET /places
Con este recurso puedes obtienes una lista de lugares alrededor de un punto. Envía como parámetros la `latitud`, `longitud` y opcionalmente la `precisión` o el `radio` para acotar el resultado. La precisión varía de 0 a 2 siendo 0 más preciso y 2 menos preciso:

Filtro por `radio`:

```json
    {
        latitude:      "-33.9250334",
        longitude:     "18.423883499999988",
        radio:         "500"
    }
```
Filtro por `precisión`:
```json
    {
        latitude:      "-33.9250334",
        longitude:     "18.423883499999988",
        precision:      "1"
    }
```

En el caso de que haya respuesta, se devuelve un `200 Ok` junto con una lista de lugares e información relacionada:
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
        postal_code:    "48100"
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
Utiliza este recurso para obtener información detallada de un sitio en concreto. Envía junto con la petición los parámetros `id` y `reference`.

Si el place no tienes reference la consulta es se realia de la siguiente forma:
```json
    {
        id:             "51e92bfab68307fe59000030"
    }
```

Si el place tiene `reference` envía la petición de la siguiente forma:
```json
    {
        id:             "51e92bfab68307fe59000030",
        reference:      "CoQBcwAAAGFG6LT0qt4U3kxuIuywXrFmvZaUvAZ5zjhZWXMQJtGlL1afAla1RS6PYuANvkWVPzGMh3YMWThOM1NFUm5satOXxKKmUb7_H19Tfep"
    }
```

Si la consulta ha obtenido respuesta se devuelve un `200 Ok` junto con el objeto:
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
Utiliza este recurso para dar de alta un sitio. Envía junto con la petición los siguientes parámetros:
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
Opcionalmente se pueden añadir los siguientes parámetros:
```json
    {
        mail: "hello@sanmames.com",
        phone: "944411445",
        website: "www.sanmames.com"
    }
```

Si la consulta ha obtenido respuesta se devuelve un `201 Created`.


### Checkins
#### POST /checkin
Los usuarios de tu aplicación pueden registrar visitas a sitios concretos. Para ello utiliza este recurso pasando el parámetro id:
```json
    {
        id:            "51e92bfab68307fe59000030"
    }
```

Si todo ha salido bien, se devuelve un `200 Ok` junto con el objeto:
```json
    {
        status:     'ok'
    }
```

#### GET /checkin
Obtén la lista de sitios guardados por tus usuarios con este recurso. Para ello únicamente tienes que enviar el id del usuario en la petición:
```json
    {
        id:            "51e930cad2eeaea678000010"
    }
```

Si ha salido todo bien, obtienes un `200 Ok` junto con el objeto:
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
Proporciona a tus usuarios información sobre amigos cercanos a un punto determinado. Para ello utiliza este recurso pasando los siguientes parámetros:
```json
    {
        latitude:       "-33.9250334",
        longitude:      "18.423883499999988",
        radio:          "500"
    }
```

Si todo ha salido bien, se recibe un `200 Ok` junto con el objeto:
```json
    {
        avatar: "http://appnima-dashboard.eu01.aws.af.cm/static/images/avatar.jpg"
        id: "51aef6f4560d261d15000001"
        name: Cata
        username: "catalina@tapquo.com"
    }
```

#### GET /people
De la misma forma que puedes mostrar a un usuarios qué amigos están a su alrededor, puedes por ejemplo proponer gente fuera de sus listas de amigos. La petición se realiza con los mismos parámetros:


```json
    {
        latitude:       "-33.9250334",
        longitude:      "18.423883499999988",
        radio:          "500"
    }
```

Si todo ha salido bien, se recibe un `200 Ok` junto con el objeto:
```json
    {
        avatar: "http://appnima-dashboard.eu01.aws.af.cm/static/images/avatar.jpg"
        id: "51aef6f4560d261d15000001"
        name: Cata
        username: "catalina@tapquo.com"
    }
```

### User
#### GET /user
Este recurso te será útil cuando quieras obtener la última posición almacenada de tu usuario. Para ello solo tienes que realizar la petición y no es necesario enviar ningún parámetro. Si todo ha ido bien App/nima devolverá el siguiente objeto:

```json
    {
        latitude:       "-33.9250334",
        longitude:      "18.423883499999988"
    }
```
#### POST /user
Utiliza este recurso para almacenar la posición de tu usuario. Envía junto con la petición su latitud y longitud:
```json
    {
        latitude:       "-33.9250334",
        longitude:      "18.423883499999988"
    }
```
Si todo ha salido bien, App/nima devolverá un `201 Created`.


Socket
------
App/nima te permite trabajar con sockets donde puedes tener con salas de conversaciones. Para ello ten en cuenta que todas las peticiones que hagas tendrán que ir a:

    http://socket.appnima.com/{RECURSO}

Recuerda que todas las peticiones que hagas a App/nima tienen que ir identificadas con tu `Appnima.key` o bien con el par de datos `client` y `secret`. Ahora veamos los recursos que puedes utilizar, para ello el primer parametro indica el tipo de petición (GET, POST, UPDATE, DELETE …) y el segundo parametro el nombre del recurso.


### Comenzando
Para la comunicación en tiempo real, appnima utiliza socket.io para conectarse a las diferentes salas creadas. A continuación se exponen los siguientes métodos de interacción con ellas. Aunque recomendamos el uso de nuestra librería `Appnima.js` para esta labor.

#### open
Para crear una sala se llamará al método `open`. Esté recibirá los siguientes parametros:

* Token del usuario
* Contexto (que se usará para identificar la sala para futuras conexiones)
* Nombre de sala
* Tipo de sala
* Si queremos que la información de la sala sea persistente
* Array de los ids de los usuarios permitidos

Si todo está correcto la sala se creará y automáticamente el autor estará conectado a ella.

#### join
Un usuario puede conectarse a una sala siempre y cuando ya esté creada y tenga permiso de conexión a la misma. Para ello se llamará al método `join` y se le pasarán los siguientes parámetros:

* Token del usuario (obligatorio a no ser que sea sesión anónima)
* Contexto (identificador de la sala a la que se quiere conectar)
* Id de aplicación (en caso de sesión anónima se requiere de este campo)

#### leave
En caso de querer desconectarse de una sala se llamará al método `leave`.

#### sendmessage
Para enviar un mensaje a una sala basta con llamar al método `sendMessage` y pasarle como parámetro un objeto (el mensaje puede ser cualquier tipo de dato). Este mensaje llegará a todos los usuarios conectados a la sala, emisor incluido, se recibirá a través del listener `onMessage` y tendrá el siguiente formato:
```json
    {
        user:           {"usuario que envía el mensaje"},
        message:        {"mensaje enviado"},
        created_at:     "2013-05-23T12:01:02.736Z"
    }
```

#### sendbroadcast
Este método funciona igual que el método `sendMessage` con la única diferencia de que el emisor no recibirá el mensaje enviado.

#### allowusers
Se pueden añadir usuarios a la lista de admitidos a través del método `allowUsers`, para ello bastará con llamar a este método y pasarle un array con los ids de los usuarios a admitir.

#### disallowusers
Se pueden eliminar usuarios de la lista de admitidos a través del método `disallowUsers`, para ello bastará con llamar a este método y pasarle un array con los ids de los usuarios a eliminar de la lista de admitidos.


### Tipos y permisos
Existen distintos tipos de salas, cada una tiene unos permisos distintos dependiendo del usuario.

* Privadas: Solo se pueden conectar, leer y escribir usuarios que se encuentren en la lista de admitidos.
* Públicas: Todo el mundo se puede conectar y leer, pero solo el dueño puede publicar en ella.
* Inbox: Solo el dueño puede leer lo que se escribe, pero cualquier usuario puede ecribir.
* Aplicación: Esta sala existe en todas las aplicaciones, por lo que nadie tiene que crearla, todos pueden conectarse, leer y escribir en ella.


### Peticion HTTP
#### GET /rooms
Un usuario puede obtener el listado de salas de sockets en las que participa, para ello bastaría con llamar al recurso y se obtendría un mensaje `200 Ok` junto con un listado como el siguiente:
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


WebRTC
------
App/nima dispone de un servidor que se encarga de gestionar conexiones entre usuarios de appnima para así poder realizar comunicaciones webRTC.

    http://webrtc.appnima.com/

WebRTC (Web Real-Time Communication) es una API que está siendo elaborada por la World Wide Web Consortium (W3C) para permitir a las aplicaciones del navegador realizar llamadas de voz, chat de vídeo y uso compartido de archivos P2P sin necesidad de instalar ningún plugin.

### Comenzando
Para crear una conexión webRTC Appnima dispone de un servidor socket.io que se encarga de realizar el `signaling` entre 2 usuarios de appnima. Se recomienda utilizar la libreria `Appnima.js` para este propósito, o en su defecto, utilizar la libreria de cliente de socket.io.

Más información sobre [**socket.io**](http://socket.io/)

### Métodos de llamada al servidor WebRTC:

#### connect
Para conectar con el servidor de WebRTC se llamará al método `connect`. Este recibirá un único parámetro que será el token proporcionado en el login de usuario.

Una vez se haya conectado el usuario, podrá tanto realizar llamadas como recibir llamadas de otros usuarios.

#### offer
Para solicitar una llamada a otro usuario se debe utilizar el método `offer` y para ello se le pasarán los siguientes parámetros:

* Token del usuario (obligatorio)
* Id de usuario al que se quiere llamar (obligatorio)
* SessionDescription: objeto devuelto mediante el método de la API de WebRTC PeerConnection.createOffer (obligatorio).

#### answer
Para aceptar o responder a una llamada de otro usuario se debe utilizar el método `answer` y para ello se le pasarán los iguientes parámetros:

* Token del usuario (obligatorio)
* Id de usuario del que quiere responder la llamada (obligatorio)
* SessionDescription: objeto devuelto mediante el método de la API de WebRTC PeerConnection.createAnswer (obligatorio). Debe enviarse el objeto completo y serializado: JSON.serialize(objetoDescription)

#### ice
Una vez creada la conexión entre dos usuarios, el usuario que recibe la llamada deberá emitir al servidor los cambios de sus servidores "ice" asociados a su conexión P2P.

La API de webRTC dispone de un evento `onicecandidate` al que el cliente deberá suscribirse y emitir la información recibida a este método `ice` con los siguientes parámteros:

* Token del usuario (obligatorio)
* Candidato: El parámetro candidate del objeto recibido en la suscripcion a `onicecandidate`. Este objeto debe enviarse serializado.

### Métodos de vuelta desde el servidor WebRTC:

#### offer
El usuario recibe una oferta de conversación de algún amigo suyo. Como parámetro recibirá la información del usuario

#### answer
Cuando un amigo acepta una conversación, este evento será lanzado en el cliente que ha realizado la llamada. Dispondrá como parámetro el amigo que ha contestado serializado.

#### connected
Se ha conectado algún amigo. Como parámetro llegará la información de este amigo serializada.

#### disconnected
Se ha desconectado algún amigo. Como parámetro llegará la información de este amigo serializada.

#### ice
Cuando hay cambios de los servidores del que recibe la llamada, al que ha realizado la llamada le llegará la información de los nuevos servidores ICE.

#### error
Método lanzado cuando ocurre algún error en el servidor.


Push
----
Con este módulo puedes enviar notificaciones a los terminales registrados de los usuarios de tu aplicación.

    http://api.appnima.com/push{RECURSO}

Recuerda que todas las peticiones que hagas a App/nima tienen que ir identificadas con tu `Appnima.key` o bien con el par de datos `client` y `secret`. Ahora veamos los recursos que puedes utilizar, para ello el primer parametro indica el tipo de petición (GET, POST, UPDATE, DELETE …) y el segundo parametro el nombre del recurso.


#### POST /push
Envía notificaciones push mediante este recurso. Junto con la petición, envía los siguientes parámetros:
```json
    {
        user:       23094392049024,
        alert:      ""Texto a mostrar en la notificación",
        content":   {"title": "JSON con los campos necesarios", "text": "Hola App/nima!"}
    }
```
En caso de éxito se devolverá el código `200 Ok`.
