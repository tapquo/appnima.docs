Appnima
=======

Descubre como dar alma a tus aplicaciones con el primer LaaS en el mundo.

*Version Actual:* 2.1.14

Introducción
------------

Simplemente por leer este documento declara que eres un desarrollador que quiere
mejorar constantemente y quiere crear proyectos cada vez más eficientes. Appnima
es la primera plataforma que ofrece servicios lógicos para cualquier tipo de
proyecto, da igual si quieres crear una aplicación o un site, Appnima te ayudará
en ambas situaciones.

Un poco de historia, hace ya casi 4 años en [Tapquo][1] nos encontramos con un
problema común en el mundo del desarrollo y no era más que cada nuevo producto
que creabamos debiamos repetir una y otra vez la misma funcionalidad básica.
Appnima surgio de la necesidad de ser cada vez más eficientes y de querer
desarrollar únicamente el negocio implicito de nuestro nuevo producto y no tanto
de la lógica horizontal:

[1]: <http://tapquo.com>

-   [Servicio OAuth 2 para la autentificación](#oauth2)

-   [Gestión de usuarios](#usuarios)

-   [Red social de usuarios](#social)

-   [Servicio de mensajeria para enviar emails, SMS, llamadas y mensajes privados](#messenger)

-   [Localizar/Geoposicionar lugares](#location)

-   [Localizar/Geoposicionar a otros usuarios](#places)

-   [Tener Real-time por medio de sockets](#sockets)

-   [Notificaciones Push para las plataformas: iOS, Android o Blackberry](#push)

-   Saber como se comportan nuestros usuarios

Si por lo menos alguna vez has tenido que desarrollar alguna de estas
funcionalidades en alguno de tus proyectos, eres bienvenido a nuestra plataforma
ya que Appnima esta pensada para ayudarte como desarrollador. Queremos ofrecerte
una plataforma centrada en facilitarte la vida, queremos el software tanto como
tu y buscamos ser cada día mejores. Usa Appnima.

### Arquitectura

Seguramente en este momento tendrás dudas de como Appnima puede ayudarte a ser
más eficiente pero tranquilo vamos a explicarte cómo funciona. Appnima esta
basada por completo en el protocolo de autentificación [Oauth 2][2] y en la
serialización mediante REST-style con objetos JSON. Si nunca has utilizado este
paradigma no te preocupes te explicarémos paso a paso como conectarte a la
plataforma y te ofreceremos herramientas específicas como [AppnimaJS][3] que te
facilitarán el proceso de conexión.

[2]: <http://oauth.net/2/>

[3]: <https://github.com/tapquo/appnima.docs/blob/master/ES/appnimajs.md>

Todo el backend está montado a lo largo del mundo utilizando plataformas como
Amazon o Google Cloud Engine las cuales nos dan la capacidad de ofrecer siempre
la mejor calidad de respuesta para nuestros usuarios. No te preocupes no tendrás
que ser un *Jedy SysAdmin* ya que tu unicamente te tienes que comunicar con
nuestro REST la tarea de escalabilidad corre de nuestra cuenta, tu preocupate en
crear el mejor proyecto y nosotros de ofrecerte cada día una mejor plataforma.

Resumiendo Appnima es una plataforma en forma de API REST que te provee de los
servicios lógicos de tu proyecto, y dependiendo de su naturaleza puede que no
necesites ni tu propio Backend. Cada servicio lógico esta alojado en servidores
diferentes para obtener la mayor escalabilidad e independiente, a lo largo de la
documentación conoceremos las rutas de cada servicio.

### [Oauth 2](id:oauth2)

Todos los servicios de Appnima utilizan el protocolo de autentificación [OAuth
2][4] buscando la mayor compatibilidad con herramientas de terceros. Si no eres
un experto en OAuth 2 te recomendamos que lo estudies ya que esta siendo a
llamar el protocolo de autentificación por excelencia y empresas como Google,
Facebook o Twitter lo utilizan para conectarse a sus APIs. Lee la
[documentación][5] para comenzar hoy mismo con Appnima.

[4]: <http://oauth.net/2/>

[5]: <http://>

### No XML, solo JSON

Sólo permitimos la serialización de datos tipo JSON. Nuestro formato es no tener
ningún elemento raíz y utilizar *snake_case* para describir las claves de
atributos. Esto significa que en cada petición a Appnima tienes que enviar el
sufijo:

``` Content-Type: application / json; charset = utf-8 ```

Recibirás una respuesta 415 *Unsupported Media Type* si intentas utilizar otro
tipo de sufijo URL.

### Usa el HTTP cache

Tienes que hacer uso de las cabeceras HTTP *freshness* para disminuir la carga
de nuestros servidores (¡y aumentar la velocidad de tu aplicación!). La mayoría
de las peticiones que devolvemos incluirán un *ETag* o una cabecera
*Last-Modified*. Al solicitar por primera vez un recurso, almacena este valor y
envianoslo en las posteriores peticiones como *If-None-Match* y
*If-Modified-Since*. Si el recurso no ha cambiado, obtendrás un código 304 *Not
Modified* como respuesta, que te ahorra tiempo y ancho de banda (porque ya
tienes ese recurso).

### Manejando errores

Si Appnima tiene algun problema, es posible que veas un error 5xx. 500 significa
que la aplicación esta totalmente caída, pero tambien puedes ver un 502 *Bad
Gateway* (entrada erronea), 503 *Service Unavailable* (Servicio no disponible),
o un 504 *Gateway Timeout* (límite de tiempo de espera). En todos estos casos es
tu responsabilidad reintentar la petición más tarde.

Clientes
--------

Ayudanos
--------

Por favor no dudes en ponerte en contacto con nosotros si crees que puedes hacer
una mejor API. Si crees que deberíamos soportar una nueva funcionalidad o si has
encontrado un bug, usa GitHub issues. Haz un fork de esta documentación y
mandanos tus *pulls* con las mejoras.

Para hablar con nosotros o con otros desarrolladores sobre la API, suscribete a
nuestra [lista de correo][6].

[6]: <http://tapquo.com/forum>

API REST
========

CONEXIÓN
--------

Para comunicarte con la plataforma debes authenticar las peticiones. Existen dos
formas: enviando la `KEY` de la aplicación o enviando el `ACCESS_TOKEN` de tu
usuario. La `KEY` la obtienes cuando registras una aplicación en tu [panel de
gestión][7]. El `ACCESS_TOKEN` de un usuario lo obtienes una vez registrado el
usuario en tu aplicación después de un `signup`.

[7]: <http://appnima.tapquo.com>

Así, para peticiones mediante la `KEY` de la aplicación la cabecera debe ir
configurada de la siguiente forma:

```json
  "authorization" : "basic APPLICATION_KEY"
```


Y si son peticiones con el `ACCESS_TOKEN` de un usuario:

```json
  "authorization" : "bearer USER_ACCESS_TOKEN"
```

[USER](id:usuarios)
----

Este módulo recoge toda la funcionalidad para incluir y gestionar un usuario de
tu proyecto dentro la plataforma Appnima:

-   Registro y sesión de un usuario

-   Recuperar contraseña

-   Resgitro de un terminal (móvil, escritorio, tablet,…)

-   Registro y manejo de tickets (sistema de consulta para tu aplicación)

-   Sistema de suscripción, por ejemplo, para accesos mediante invitación

Para ello ten en cuenta que todas las peticiones que hagas tendrán que ir a:

```
  http://api.appnima.com/user/{RECURSO}
```

Recuerda que todas las peticiones que hagas a Appnima deben ir identificadas con
tu `KEY` de aplicación o bien con el `ACCESS_TOKEN` del usuario.

*Example*

```
  [GET] http://api.appnima.com/user/info
```

### Seguridad

#### POST /signup

Todos los usuarios de tu aplicación tienen que ser usuarios Appnima y por lo
tanto lo primero que tendrás que hacer es registrarlos para así poder obtener su
`ACCESS_TOKEN` que será la llave para acceder a cualquier recurso de la
plataforma. Puedes registrar un usuario por mail o por username. Utiliza este
recurso pasando como parámetros mail o username y el password:

```json
{
  "mail"    : "USER_MAIL",
  "password": "USER_PASSWORD"
}
```

Opcionalmente también puedes enviar:

```json
{
  ...
  "username"  : "USER_NICKNAME",
  "name"      : "USER_NAME"
}
```

Si ha ido todo bien el servidor retorna un `200` junto con el objeto:

```json
{
  "id"            : "USER_ID",
  "application"   : "APPLICATION_ID",
  "access_token"  : "ACCESS_TOKEN",
  "refresh_token" : "REFRESH_TOKEN",
  "expire"        : "2015-06-24T15:19:13.138Z"
}
```


#### POST /login

Cada vez que quieras validar si el usuario tiene permisos para acceder a tu
aplicación podrás usar este recurso. Únicamente necesita los parámetros:

```json
{
  "mail"      : "USER_MAIL",
  "password"  : "USER_PASSWORD"
}
```

```json
{
  "username" : "USER_NICKNAME",
  "password" : "USER_PASSWORD"
}
```

Dependerá de con qué se haya registrado el usuario.

En caso de que la validación haya sido correcta Appnima devolver un `200` con
los datos del usuario:

```json
{
  "id"            : "USER_ID",
  "mail"          : "USER_MAIL",
  "usenarme"      : "USER_NICKNAME",
  "name"          : "USER_NAME",
  "avatar"        : "http://api.appnima.com/avatar/default.jpg",
  "access_token"  : "ACCESS_TOKEN",
  "refresh_token" : "REFRESH_TOKEN",
  "expire"        : "2014-06-23T17:53:35.697Z"
}
```

#### POST /token

A partir de ahora, para acceder a cualquier recurso de Appnima es necesario
enviar en la cabecera de la petición el atributo `authorization` con el valor
del `ACCESS_TOKEN` del usuario:

```json
{
  "authorization" : "bearer ACCESS_TOKEN"
}
```

Si el token del usuario estuviera caducado o por alguna razón es necesario un
token nuevo, utiliza este recurso para obtener un `ACCESS_TOKEN` nuevo.

En este caso, en la cabecera de la petición el atributo `authorization` lleva la
`KEY` de la aplicación:

```json
{
  "authorization": "basic KEY"
}
```

Y los parámetros que tienes que enviar son el `grant_type` con el string
"refresh_token" y el valor del `"REFRESH_TOKEN"` del usuario:

```json
{
  "grant_type"    : "refresh_token",
  "refresh_token" : "REFRESH_TOKEN"
}
```

Si todo ha ido bien el servidor devuelve un `200` el siguiente objeto:

```json
{
  "id"            : "USER_ID",
  "application"   : "APPLICATION_ID",
  "access_token"  : "ACCESS_TOKEN",
  "refresh_token" : "REFRESH_TOKEN",
  "expire"        : "2014-06-24T15:19:13.138Z"
}
```

### Info

#### GET /info

Para obtener los datos de tu usuario, gracias al protocolo de autentificación
OAuth 2 no es necesario que envíes ningún parámetro a no ser que desees obtener
la información de otro usuario de tu aplicación.

Por lo tanto envía esta petición sin parámetros para que Appnima te devuelva los
datos del usuario logueado o envía como parámetro `id` la ID del usuario del
cual deseas obtener sus datos.

```json
{
  "id" : "USER_ID"
}
```

En ambos casos, solo deberás esperar a la respuesta `200 Ok` y Appnima te
devuelve los siguientes parámetros:

```json
{
  "id"      : "USER_ID",
  "mail"    : "USER_MAIL",
  "username": "USER_NICKNAME",
  "name"    : "USER_NAME",
  "avatar"  : "http://api.appnima.com/avatar/default.jpg",
  "language": "spanish",
  "country" : "ES",
  "bio"     : "Founder & CTO at [@tapquo]",
  "phone"   : "PHONE_NUMBER",
  "site"    : "USER_SITE"
}
```

#### PUT /update

Este recurso sirve para modificar los datos personales de un usuario dentro de
tu aplicación, al igual que en el recurso **GET /user/info** no es necesario
identificar al usuario por parámetro. Puedes enviar todos los parámetros que
aparecen a continuación (aunque no es obligatorio enviarlos todos):

```json
{
  "name"    : "USER_NAME",
  "mail"    : "USER_MAIL",
  "avatar"  : "USER_AVATAR",
  "language": "spanish",
  "country" : "Spain",
  "phone"   : "PHONE_NUMBER",
  "site"    : "USER_SITE",
  "bio"     : "USER_BIO"
}
```

El atributo `AVATAR` se envía como base64.

En el caso de que haya ido todo bien se devolverá el código `200 OK` junto con
el mismo objeto **GET /user**. En el caso de que el usuario no tenga permiso
para modificar sus datos Appnima devolverá un `403 Forbidden`.

### Contraseña

Appnima ofrece a sus usuarios dos formas de tratar contraseñas, recordarla o
cambiarla.

#### POST /remember/password

Este recurso sirve para recordar contraseña. Si se tiene backend de la
aplicación, para recordar contraseña es necesario hacerlo en dos pasos, en caso
de no tener, o no desear usar su propio backend, se podrá realizar en un paso.

Si se desea hacer en un paso, hay que enviar los siguientes parámetros:

```json
{
  "mail"        : "USER_MAIL",
  "application" : "APPLICATION_ID"
}
```

Este parámetro se trata del `mail` del usuario de Appnima. Appnima se encargará
de la gestión de la URL que se envia en el email, como se explica más adelante.

Por otro lado, si se desea hacer en dos pasos, es necesario enviar la URL a la
que se redirigirá al usuario:

```json
{
  "mail"        : "USER_MAIL",
  "application" : "APPLICATION_ID",
  "domain"      : "URL_CALLBACK"
}
```

Así, el susario que requiera recuperar su contraseña recibirá un correo desde
Appnima con el siguiente enlace:

``` http://URL_CALLBACK/forgot?forgot_key=CODE ```

#### POST /reset/password

Como se ha dicho en el recurso anterior, recordar contraseña se puede hacer en
dos pasos. En caso de hacerlo así, la segunda parte es llamar a este recurso.
Este recurso por sí solo no se puede utilizar, ya que requiere del código
generado en el recurso anterior.

Para esto, como se puede observar, es necesario generar un endpoint en el
backend de la aplicación con dicha URL_CALLBACK en la que haya un formulario
donde rellenar la nueva contraseña deseada. Por lo tanto, hay que enviar al
recurso los siguientes datos:

```json
{
  "code"    : "CODE",
  "password": "USER_PASSWORD"
}
```

El primer parámetro es el código de la URL generada en el recurso anterior y el
segundo parámetro es la nueva contraseña que el usuario quiere regenerar. Una
vez hecho esto, Appnima envía un email al usuario avisándole de que su
contraseña ha sido modificada.

#### PUT /password

Este recurso sirve para cambiar la contraseña y el único requisito es estar
identificado con Appnima. Junto a la petición hay que enviarle los siguientes
parámetros:

```json
{
  "old_password": "OLD_USER_PASSWORD",
  "new_password": "NEW_USER_PASSWORD"
}
```

### Terminal

#### POST /terminal

Este recurso sirve para que en el caso de que necesites registrar el terminal
con el que se está accediendo a tu aplicación. Para ello junto con la petición
tienes que enviar los parámetros:

```json
{
  "type"    : "phone",
  "os"      : "ios",
  "version" : "6.0"
}
```

**type**: computer, tablet, phone, tv
**os**: ios, windows, macos, linux, android, blackberry, firefoxOS, windowsphone, other

En el caso de que haya ido todo bien se devolverá el código `201`.

#### GET /terminal

Si necesitas saber los terminales desde los que se ha accedido a tu aplicación
simplemente tienes que utilizar este recurso y al igual que **GET /user/info**
no hace falta que envies ningún parámetro. La respuesta si ha ido todo
correctamente será un `200 Ok` junto con el objeto:

```json
[
  {
    "_id"     : "TERMINAL_ID",
    "type"    : "phone",
    "os"      : "ios",
    "token"   : "USER_TOKEN",
    "version" : "6.0"
  },
  {
    "_id"     : "TERMINAL_ID",
    "type"    : "desktop",
    "os"      : "macos",
    "token"   : "USER_TOKEN",
    "version" : "10.8"
  }
]
```

#### PUT /terminal

Es recurso actualiza los datos de un termina. Para ello junto con la petición
tienes que enviar los parámetros:

```json
{
  "terminal" : "TERMINAL_ID",
  "type"     : "phone",
  "os"       : "ios",
  "version"  : "6.0"
}
```
**type**: computer, tablet, phone, tv
**os**: windows, macos, linux, ios, android, blackberry, firefoxOS, windowsphone, other

### Suscripciones

#### POST /subscription

Utilizando este recurso puedes ofrecer un sistema de suscripción a tu
aplicación. Registra los e-mails de los usuarios que desean recibir información
o invitación. Solo necesitas pasar como parámetro la dirección e-mail:

```json
{
  "mail": "USER_MAIL"
}
```

Si el parámetro es correcto se recibe un `200 Ok` junto con el objeto:

```json
{
  "message": "Request accepted."
}
```

A partir de ese momento, desde tu [Panel de Control](http://tapquo.com/appnima)
podrás enviar las invitaciones a los usuarios registrados.

### Soporte

Con estos métodos pordrás ofrecer un sistema de tickets de consulta. El usuario
de tu aplicación podrá generar cosultas y tu o cualquier usuarior registrado
podrá contestarlos.

#### GET /user/ticket

Utiliza este recurso para obtener un ticket en concreto. Para ello simplemente
hay que mandar la ID del ticket que se quiere obtener.

```json
{
  "id" : "TICKET_ID"
}
```

Si todo ha ido bien, Appnima devuelve `200` y el siguiente objeto:

```json
  ticket:
      {
        "id"   : "TICKET_ID",
        "user" :
          {
            "id"      : "USER_ID",
              "name"    : "Bob",
              "username": "USER_NICKNAME",
              "mail"    : "USER_MAIL",
              "avatar"  : "http://api.appnima.com/img/avatar.jpg"
             },
        "type"       : "QUERY",
        "title"      : "Consulta: Scopes disponibles",
        "description": "¿Cuáles son los scopes disponibles en Appnima?",
        "updated_at" : "2015-01-14T06:45:05.625Z",
        "created_at" : "2015-01-14T06:45:05.625Z"
      }
```

#### GET /user/ticket/search

Utiliza este recurso para buscar un conjunto de tickets. Se puede filtrar por
los siguientes campos:

-   **`user`**: Para obtener los tickets de este usuario en concreto.

-   **`reference`**: Para obtener los tickets por esta referencia.

-   **`type`**: Para obtener los tickets de este tipo.

-   **`solved`**: (true o false) Para obtener los tickets resueltos (true) o los
    pendientes (false)

Se puede hacer búsqueda por un campo en concreto o por la mezcla de varios. En
el ejemplo siguiente sería una búsqueda con todos los campos y el objeto a
mandar seria el siguiente:

```json
{
  "user"      : "USER_ID",
  "reference" : "REFERENCE_MODEL",
  "type"      : 2,
  "solved"    : true
}
```

En este caso es posible usar la paginación. Esta opción es igual que en los
`POST` salvo que no hay que enviar el atributo `last_data`.

#### POST /user/ticket

Utiliza este recurso como sistema de gestión de tickets para la resolución de
las consultas e incidencias de tus usuarios. La petición necesita un objeto como
el siguiente:

```json
{
  "title"       : "How can I start with [Atoms]?",
  "description" : "Where can I find documentation related?",
  "reference"   : "REFERENCE_MODEL",
  "type"        : "2"
}
```

El campo reference se utiliza por si se quiere añadir la ID de cualquier otro
modelo, ya sea de APPNIMA o de otra base de datos. El campo type puede ser 0, 1
o 2. Si no se manda este campo, por defecto es 0.

``` 0 -> "query" 1 -> "bug" 2 -> "support" ```


#### PUT /user/ticket

Para modicifar un ticket solamente tienes que enviar la ID del ticket y los
atributos a cambiar. Los atributos permitidos son
["title", "description", "reference", "type"]:

```json
{
  "id"   : "TICKET_ID",
  "type" : "0"
}
```

Una vez respondido al ticket, se envía un email al creador de dicho ticket. Para
ambos casos habría que enviar los datos a la siguiente llamada:


#### DELETE /user/ticket

También se puede eliminar un ticket pasándole la ID de dicho ticket.

```json
{
  "id": "TICKET_ID"
}
```

#### POST /user/ticket/reply

Con este método podrás responder al un ticket creado por un usuario. Envía como
parámetros el ID del ticket y el texto con la respuesta de la siguiente manera:

```json
{
  "id"      : "TICKET_ID",
  "response": "Hello, you can find all documentation here: https://github.com/tapquo/atoms"
}
```

[Network](id:social)
-------

Este modulo recoge toda la funcionalidad para crear una red social dentro de tu aplicación:

-   Buscar usuarios
-   Añadir o quitar usuarios a la lista de amigos
-   Listar usuarios de la lista de followings y followers de un usuario
-   Comprobar la relación entre dos usuarios
-   Los usuarios podrán publicar mensajes al estilo de un timeline público
-   Buscar mensajes concretos o la lista de mensajes de un usuario
-   Registrar un comentario sobre un mensage
-   Marcar como favorito un mensaje
-   Obtener la lista de mensajes marcados como favoritos
-   Obtener los mensajes públicos del timeline de un usuario

Para ello ten en cuenta que todas las peticiones que hagas tendrán que ir a:

``` http://api.appnima.com/network/{RECURSO} ```

Recuerda que todas las peticiones que hagas a Appnima deben ir identificadas con
tu `KEY` de aplicación o bien con el `ACCESS_TOKEN` del usuario.

Ahora veamos los recursos que puedes utilizar, para ello el primer
parámetro indica el tipo de petición (GET, POST, UPDATE, DELETE …) y el segundo
el nombre del recurso.

### Relaciones

#### GET /search

Utiliza este recurso para buscar ususarios dentro de tu aplicación. Puedes
enviar como parámetro el mail o parte del mail de un usuario o su nickname o
parte de él.

```json
{
  "query": "javi"
}
```

En el caso de que la respuesta haya sido satisfactoria se devolverá un `200 Ok`
junto con una lista de usuarios que coinciden con la búsqueda:

```json
  [
    {
      "avatar"  : "USER_AVATAR",
      "id"      : "USER_ID",
      "name"    : "USER_NAME",
      "username": "USER_MAIL"
    },
    {
      "avatar"  : "USER_AVATAR",
      "id"      : "USER_ID",
      "name"    : "USER_NAME",
      "username": "USER_MAIL"
    },
    {
      "avatar"  : "USER_AVATAR",
      "id"      : "USER_ID",
      "name"    : null,
      "username": "USER_MAIL"
    }
  ]
```

#### POST /follow

Para seguir a un usuario utiliza este recurso pasando como parámetro su `id`:

```json
{
  "user": "USER_ID"
}
```

Si todo ha salido bien el servicio devolverá un `200 Ok`.

Si se desea realizar un follow blindado, esto es, que tú seas amigo del usuario
que envíar y que él sea el tuyo en una única llamada, simplemente hay que enviar
los siguientes parámetros:

```json
{
  "user"   : "USER_ID",
  "shield" : true
}
```

#### POST /unfollow

Para dejar de seguir un usuario utiliza este recurso de igual manera que **POST
/follow**:

```json
{
  "user" : "USER_ID"
}
```

Si todo ha salido bien el servicio devolverá un `200 Ok` junto con el objeto:

```json
{
  "message" : "Successful"
}
```

### Información

Con estos recursos podrás obtener la lista de followings y followers de un
usuario así como el contador.

#### GET /following

Con este recurso puedes obtener la lista de followings del usuario de sessión
llamando al recurso sin pasar parámetro o puedes obtener la lista de followings
de otro usuario pasando como parámetro su `id`:

```json
{
  "user": "USER_ID"
}
```

Si todo ha salido bien el servicio devolverá un `200 Ok` junto con el objeto:

```json
  "count" : 4
  [
    {
      "avatar"  : "USER_AVATAR",
      "id"      : "USER_ID",
      "mail"    : "USER_MAIL",
      "name"    : "USER_NAME",
      "username": "USER_NICKNAME"
    },
    {
      "avatar"  : "USER_AVATAR",
      "id"      : "USER_ID",
      "mail"    : "USER_MAIL",
      "name"    : "USER_NAME",
      "username": "USER_NICKNAME"
    },
    {
      "avatar"  : "USER_AVATAR",
      "id"      : "USER_ID",
      "mail"    : "USER_MAIL",
      "name"    : "USER_NAME",
      "username": "USER_NICKNAME"
    }
]
```

Al igual que en el *timeline* del recurso de `MESSENGER`, existe la opción de
obtener estos datos mediante paginación. Para ello, simplemente tienes que
añadir los siguientes parámetros a tu llamada:

```json
{
  "username"    : "username",
  "page"        : 0,
  "num_results" : 5
}
```

El significado de las variables *page* y *num_results* es el mismo que en el
caso de la llamada al **Timeline**. La única diferencia de ambas llamadas es que
en este caso no hace falta enviar la variable de la última fecha del último dato
obtenido.

#### GET /followers

Funciona de igual manera que **GET /following**, puedes enviar la `id` del
usuario del que quieres obtener la información o si no lo envías obtienes la
lista de followers del usuario de la sesión:

```json
{
  "user": "USER_ID"
}
```

Si todo ha salido bien el servicio devolverá un `200 Ok` junto con el objeto:

```json
  "count": 3
  [
    {
      "avatar"  : "USER_AVATAR",
      "id"      : "USER_ID",
      "mail"    : "USER_MAIL",
      "name"    : "USER_NAME",
      "username": "USER_NICKNAME"
    },
    {
      "avatar"  : "USER_AVATAR",
      "id"      : "USER_ID",
      "mail"    : "USER_MAIL",
      "name"    : "USER_NAME",
      "username": "USER_NICKNAME"
    },
    {
      "avatar"  : "USER_AVATAR",
      "id"      : "USER_ID",
      "mail"    : "USER_MAIL",
      "name"    : "USER_NAME",
      "username": "USER_NICKNAME"
    }
  ]
```

Al igual que lo que se ha explicado anteriormente en **GET/ followings**,
también existe la opción de obtener los datos mediante paginación. La dinámica
es la misma.

Como se puede observar, en este caso, la llamada devuelve una variable más en
cada objeto. Dicha variable indica si el usuario logueado sigue a esa persona o
no.

#### GET /friends

Con este recurso puedes obtener la lista de amigos de un usuario o del usuario
de la sesión. Si no pasas parámentro a la petición la lista será del usuario de
la sesión, en cambio si pasa la `id` de un usuario obtendrás sus amigos. Los
usuarios de tu aplicación pasarán a ser amigos cuando de forma recíprocra se
haga follow.

Parámetro para la petición:

```json
{
  "user": "USER_ID"
}
```

Si todo ha salido bien el servicio devolverá un `200 Ok` junto con el objeto:

```json
[
  {
    "avatar"  : "USER_AVATAR",
    "id"      : "USER_ID",
    "mail"    : "USER_MAIL",
    "name"    : "USER_NAME",
    "username": "USER_NICKNAME"
  }
]
```

#### GET /check

Este recurso sirve para saber la relación que tienes con un determinado usuario
(tal vez te interese seguirlo o no) para ello tenemos que enviar el `id` del
usuario a consultar de la siguiente manera:

```json
{
  "user": "USER_ID"
}
```

En el caso de que todo haya ido correctamente devolverá un `200 Ok` junto con el
objeto que representa la relación que tiene el usuario consultado con el usuario
de tu aplicación:

```json
{
  "following": true,
  "follower" : false
}
```

### Timeline

#### GET /user/timeline

Este recurso sirve para obtener la lista de posts de un determinado usuario. Si
lo que deseas es obtener los posts de tu usuario de la sesión no tienes que
enviar ningún parámetro junto con la petición. Te devolverá la lista de posts
tanto tuyos como de los usuarios a los que sigues (following) ordenados del más
antiguo al más reciente.

En cambio, si lo que deseas es obtener los posts de otro usuario o solamente los
tuyos propios, debes enviar los siguientes parámetros:

```json
{
  "username": "USERNAME"
}
```

Se trata de la id del usuario del que quieres obtener los post. En este caso,
unicamente te devolverá la lista de los post que ha creado ese usuario.

Si va todo bien, solo deberás esperar a la respuesta `200 OK`y la lista de posts
que te devolverá Appnima.

También existe la opción de que te devuelva la lista de post con paginación;
esto es, que en cada llamada a la API te vaya devolviendo parte de la lista de
posts cronologicamente.

Para ello, debes enviar los siguientes parámetros:

```json
{
  "page"        : 0,
  "num_results" : 5,
  "last_data"   : "2013-12-02 08:00:58.784Z"
}
```

A ese objeto se le debe añadir, si se desea, lo explicado anteriormente de la
*id* del usuario.

La variable *page* se trata del número de página que deseas obtener; esto es, el
trozo de la lista de post que deseas. *num_results* es el numero de resultados
que quieres obtener. En la primera llamada, esa variable será multiplicada por
2, y en los demás casos, se devulverá dicha cifra de posts. Por último, la
variable *last_data* se trata de la fecha de creación del último post recibido
en la última llamada realizada. Es importante esta fecha ya que será el punto de
comienzo de la siguiente tanda de posts.

### Favoritos

#### POST /post/like

Este recurso sirve para hacer favorito un post en concreto o para quitar un
favorito ya hecho. Para ello, solo hay que enviar el *id* de dicho post.

```json
{
  "post": "MESSAGE_ID"
}
```

Si es la primera vez que marca como favorito, Appnima devolverá la respuesta
`200 OK`. En cambio, si ya había marcado como favorito antes, se borrará dicho
favorito y Appnima devolverá un mensaje: *unliked*.

#### GET /post/user/like

Este recurso, si todo va bien, devolverá la lista de los *post* a los que el
usuario logueado ha marcado como favorito. Para ello, debes enviar el siguiente
parámetro:

```json
{
  "username": "username"
}
```

En este caso también está la posibilidad de obtener los post mediante
*paginación* como se explica en el método de **TIMELINE** en el recurso de
`MESSENGER`.

#### GET /post/like/users

Este recurso sirve para obtener todos los usuarios que han hecho favorito a un
*post* en concreto. Para ello, solamente hay que enviar la *id* de dicho post.

```json
{
  "post": "POST_ID"
}
```

### Comment

#### POST /comment

Este recurso sirve para crear comentarios sobre un `post`. La idea de este
recurso es que se puedan crear discusiones sobre los post. Para crear un
comentario hay que enviar los siguientes parámetros:

```json
{
  "id"      : "COMMENT_ID",
  "content" : "COMMENT_CONTENT"
}
```

#### GET /post/comment

Este recurso sirve para obtener todos los comentarios de un post. Solamente hay
que enviar la id de dicho *post* para que éste te devuelva la lista de los
comentarios.

#### PUT /comment

Este recurso modifica un comentario y hay que enviar el id del comentario junto
con los campos que se desea modificar.

```json
{
  "id"      : "COMMENT_ID",
  "content" : "COMMENT_CONTENT",
  "title"   : "COMMENT_TITLE"
}
```

#### DELETE /comment

Este recurso elimina un comentario, hay que enviar el id del comentario.

[Messenger](id:messenger)
---------

Este módulo recoge toda la funcionalidad de mensajería:

-   Enviar e-mail
-   SMS
-   Mensajes privados entre usuarios de tu misma aplicación.

Para ello ten en cuenta que todas las peticiones que hagas tendrán que ir a:

``` http://api.appnima.com/messenger/{RECURSO} ```

Recuerda que todas las peticiones que hagas a Appnima deben ir identificadas con
tu `KEY` de aplicación o bien con el `ACCESS_TOKEN` del usuario.

Ahora veamos los recursos que puedes utilizar, para ello el primer
parámetro indica el tipo de petición (GET, POST, UPDATE, DELETE …) y el segundo
el nombre del recurso.


### Mail

#### POST /mail
Para utilizar este servicio deberás configurar en tu [Panel de Control](http://tapquo.com/appnima)
en el apartado de "Scopes diponibles" los datos correspondientes al servidor de
correo que desees utilizar. Así, tu aplicación podrá enviar e-mails tus usuarios
que tengan su dirección de correo registrado. Los datos que se envían al método son
(el asunto es opcional):

```json
{
  "user"    : "USER_ID",
   "subject": "Appnima.com",
   "message": "Welcome to appnima.messenger!!"
}
```

Si todo ha salido bien, el servidor devolverá un `200`.


### SMS

#### POST /sms

Con este recurso tu aplicación podrá enviar mensajes de texto a los dispositivos
móviles registrados de tus usuarios. Tan solo debes enviar junto con la petición
los parámetros:

```json
{
  "user"    : "USER_ID",
  "message" : "Welcome to appnima.messenger"
}
```
Si todo ha salido bien, el servidor devolverá un `200`.


### Message

#### POST /message

Si lo necesitas, Appnima te provee de un sistema de mensajería interno entre los
usuarios de tu aplicación. Los parámetros que necesita la petición son:

```json
{
  "user"    : "USER_ID",
  "subject" : "Appnima.com",
  "body"    : "Welcome to appnima.messenger"
}
```

El campo subject es opcional y si la petición ha salido bien se devolverá el código
`200` junto con el objeto:

```json
{
  "message" : "Message sent successfully."
}
```

#### GET /message/outbox

Los usuarios de tu aplicación pueden recuperar los mensajes enviados. Para ello
basta con llamar al recurso pasando como parámetro outbox.

``` { "context": "outbox" } ```

Si todo ha salido bien, devolverá un `200 Ok` junto con lista de mensajes de la
bandeja indicada:

```json
{
  "id"          : "MESSAGE_ID",
  "from"        : "USER_ID",
  "to"          : "USER_UD",
  "application" : "APPLICATION_ID",
  "subject"     : "Appnima.com",
  "body"        : "Welcome to appnima.messenger",
  "state"       : "SENT"
}
```

#### GET /message/inbox

Los usuarios de tu aplicación pueden recuperar los mensajes recibidos. Para ello
basta con llamar al recurso pasando como parámetro inbox.

```json
{
  "context": "inbox"
}
```

Si todo ha salido bien, devolverá un `200 Ok` junto con lista de mensajes de la
bandeja indicada:

```json
{
  "id"          : "MESSAGE_ID",
  "from"        : "USER_ID",
  "to"          : "USER_UD",
  "application" : "APPLICATION_ID",
  "subject"     : "Appnima.com",
  "body"        : "Welcome to appnima.messenger",
  "state"       : "READ"
}
```

#### PUT /message

Para que se refleje que el usuario ha leído un mensaje o que desea eliminarlo de
su sistema, basta con enviar en la petición los siguientes parámetros:

```json
{
  "message" : "MESSAGE_ID",
  "state"   : "READ"
}
```
**state**: read, deleted

Si todo ha salido bien, devolverá un `200 Ok`junto con el mensaje de
confirmación:

```json
{
  "message" : "Resource READ."
}
```


[Location](id:location)
--------

Este módulo te permite obtener toda la funcionalidad respecto a la
geolocalización de usuarios y lugares. Mediante latitud y longitud obtén los
sitios en un radio determinado así como los detalles de un lugar concreto. Si tu
aplicación lo requiere, también puedes registrar un sitio mediante checkins y
además ofrecer a tus usuarios información sobre amigos cercanos.

Para ello ten en cuenta que todas las peticiones que hagas tendrán que ir a:

``` http://api.appnima.com/location/{RECURSO} ```

Recuerda que todas las peticiones que hagas a Appnima tienen que ir
identificadas con tu `Appnima.key` o bien con el par de datos `client` y
`secret`. Ahora veamos los recursos que puedes utilizar: el primer parámetro
indica el tipo de petición (GET, POST, UPDATE, DELETE …) y el segundo el nombre
del recurso.

### [Places](id:places)

#### GET /places

Con este recurso puedes obtienes una lista de lugares alrededor de un punto.
Envía como parámetros la `latitud`, `longitud` y opcionalmente la `precisión` o
el `radio` para acotar el resultado. La precisión varía de 0 a 2 siendo 0 más
preciso y 2 menos preciso:

Filtro por `radio`:

```json
{
  "latitude"  : "-33.9250334",
  "longitude" : "18.423883499999988",
  "radio"     : "500"
}
```

Filtro por `precisión`:

```json
{
  "latitude"  : "-33.9250334",
  "longitude" : "18.423883499999988",
  "precision" : "1"
}
```

En el caso de que haya respuesta, se devuelve un `200 Ok` junto con una lista de
lugares e información relacionada:

```json
[
  {
    "address"  : "Neurketa Kalea, 8, Mungia, Spain",
    "country"  : "ES",
    "id"       : "PLACE_ID",
    "locality" : "Mungia",
    "name"     : "Frus Surf",
    "phone"    : "+34 946 15 57 71",
    "position" : {
      "latitude" : "43.356091",
      "longitude": "-2.847759"
    },
    "postal_code" : "POSTAL_CODE",
    "reference"   : null,
    "website" : "http://shop.frussurf.com/"
  },
  {
    "address"  : "Neurketa Kalea, 3, Mungia, Spain",
    "country"  : "ES",
    "id"       : "PLACE_ID",
    "locality" : "Mungia",
    "name"     : "Inmobiliaria Urrutia",
    "phone"    : "+34 946 15 66 95",
    "position" : {
      "latitude" : 43.35618,
      "longitude": -2.847939
    },
    "postal_code" : "POSTAL_CODE",
    "reference"   : null,
    "website"     : "http://www.inmobiliariaurrutia.com/"
  },
  {
    "address"  : "Neurketa Kalea, 8, Mungia",
    "country"  : null,
    "id"       : "PLACE_ID",
    "locality" : null,
    "name"     : "Bar Aketxe",
    "phone"    : null,
    "position" : {
      "latitude" : 43.356091,
      "longitude": -2.847759
    },
    "postal_code": null,
    "reference"  : "REFERENCE_ID",
    "website"    : null
  }
]
```

#### GET /place

Utiliza este recurso para obtener información detallada de un sitio en concreto.
Envía junto con la petición los parámetros `id` y `reference`.

Si el place no tienes reference la consulta es se realia de la siguiente forma:

```json { id: "PLACE_ID" } ```

Si el place tiene `reference` envía la petición de la siguiente forma:

```json
{
  "id"        : "PLACE_ID",
  "reference" : "REFERENCE_ID"
}
```

Si la consulta ha obtenido respuesta se devuelve un `200 Ok` junto con el objeto:

```json
{
  "address"  : "Neurketa Kalea, 8, Mungia, Spain",
  "country"  : "ES",
  "id"       : "PLACE_ID",
  "locality" : "Mungia",
  "name"     : "Bar Aketxe",
  "phone"    : "+34 946 74 18 40",
  "position" : {
    "latitude": 43.356091,
    "longitude": -2.847759
  },
  "postal_code" : "POSTAL_CODE",
  "reviews"     : [
        {
            "time"     : "REVIEW_TIME",
            "text"     : "REVIEW_TEXT",
            "rating"   : 4,
            "language" : "en",
            "author_name" : "A Google User",
            "aspects" : [
                {
                    "type" : "overall",
                    "rating" : 2
                }
            ]
        }
      ]
}
```

#### POST /place

Utiliza este recurso para dar de alta un sitio. Envía junto con la petición los
siguientes parámetros:

```json
{
  "name"        : "San Mames",
  "address"     : "Felipe Serrate, s/n",
  "locality"    : "Bilbao",
  "postal_code" : "POSTAL_CODE",
  "country"     : "ES",
  "latitude"    : "﻿43.26",
  "longitude"   : "-2.94"
}
```

Opcionalmente se pueden añadir los siguientes parámetros:

```json
{
  "mail"    : "PLACE_MAIL",
  "phone"   : "+34 946 74 18 40",
  "website" : "www.sanmames.com"
}
```

Si la consulta ha obtenido respuesta se devuelve un `201 Created`.

### Checkins

#### POST /checkin

Los usuarios de tu aplicación pueden registrar visitas a sitios concretos. Para
ello utiliza este recurso pasando el parámetro id:

```json
{
  "id": "USER_ID"
}
```

Si todo ha salido bien, se devuelve un `200 Ok` junto con el objeto:

```json
{
  "status": "ok"
}
```

#### GET /checkin

Obtén la lista de sitios guardados por tus usuarios con este recurso. Para ello
únicamente tienes que enviar el id del usuario en la petición:

```json
{
  "id": "USER_ID"
}
```

Si ha salido todo bien, obtienes un `200 Ok` junto con el objeto:

```json
{
  "address"  : "Calle de Trobika, 1, Mungia, Spain",
  "country"  : "ES",
  "id"       : "PLACE_ID",
  "locality" : "Mungia",
  "name"     : "Policía Municipal",
  "phone"    : "+34 946 15 66 77",
  "position" : {
    "latitude"  : 43.354551,
    "longitude" : -2.846533
  },
  "postal_code" : "POSTAL_CODE",
  "reviews"     : [],
  "types": {
    "0": "police",
    "1": "establishment"
  }
}
```

### Search

#### GET /friends

Proporciona a tus usuarios información sobre amigos cercanos a un punto
determinado. Para ello utiliza este recurso pasando los siguientes parámetros:

```json
{
  "latitude"  : "-33.9250334",
  "longitude" : "18.423883499999988",
  "radio"     : "500"
}
```

Si todo ha salido bien, se recibe un `200 Ok` junto con el objeto:

```json
{
  "avatar"   : "FRIEND_AVATAR",
  "id"       : "FRIEND_ID",
  "name"     : "FRIEND_NAME",
  "username" : "FRIEND_NICKNAME"
}
```

#### GET /people

De la misma forma que puedes mostrar a un usuarios qué amigos están a su
alrededor, puedes por ejemplo proponer gente fuera de sus listas de amigos. La
petición se realiza con los mismos parámetros:

```json
{
  "latitude"  : "-33.9250334",
  "longitude" : "18.423883499999988",
  "radio"     : "500"
}
```

Si todo ha salido bien, se recibe un `200 Ok` junto con el objeto:

```json
{
  "avatar"   : "FRIEND_AVATAR",
  "id"       : "FRIEND_ID",
  "name"     : "FRIEND_NAME",
  "username" : "FRIEND_NICKNAME"
}
```

### User

#### GET /user

Este recurso te será útil cuando quieras obtener la última posición almacenada
de tu usuario. Para ello solo tienes que realizar la petición y no es necesario
enviar ningún parámetro. Si todo ha ido bien Appnima devolverá el siguiente
objeto:

```json
{
  "latitude"  : "-33.9250334",
  "longitude" : "18.423883499999988"
}
```

#### POST /user

Utiliza este recurso para almacenar la posición de tu usuario. Envía junto con
la petición su latitud y longitud:

```json
{
  "latitude"  : "-33.9250334",
  "longitude" : "18.423883499999988"
}
```

Si todo ha salido bien, Appnima devolverá un `201 Created`.

Calendar
--------

Appnima proporciona un módulo de calendarios. Se basa en que un usuario puede
crear calendarios, añadirles eventos, y compartirlos con otros usuarios. A
continuación se explica la manera de hacerlo.

#### POST /calendar

Con este recurso el usuario crea un calendario. Junto con la petición hace falta
enviar su nombre y el color que le quieras asignar:

```json
{
  "name"  : "Calendario de trabajo",
  "color" : "#3300FF"
}
```

si todo va bien, devueve el calendario creado:

```json
  caledar:
    {
      "id"         : "CALENDAR_ID",
      "name"       : "CALENDAR_NAME",
      "color"      : "#FF66CC",
      "created_at" : "Tue Feb 04 2014 13:19:06 GMT+0100 (CET)",
      "owner":
        {
          "id"       : "OWNER_ID",
          "username" : "OWNER_NICKNAME",
          "mail"     : "OWNER_MAIL",
          "avatar"   : "OWNER_AVATAR",
          "name"     : "OWNER_NAME"
        },
      "shared": []
    }
```

#### PUT /calendar

Se puede modificar un calendario ya creado, tanto su nombre como su color. Para
ello hay que mandar a la API la "id" de dicho calendario, el nuevo nombre y el
nuevo color.

```json
{
  "id"    : "CALENDAR_ID",
  "color" : "#3300FF",
  "name"  : "CALENDAR_NAME"
}
```

En caso de que el calendario no exista, devuelve un error 404. Si por el
contrario existe, devuelve el calendario con los campos modificados:
```json
 calendar:
    {
      "id"         : "CALENDAR_ID",
      "name"       : "Calendario de trabajo modificado",
      "color"      : "#FF66CC",
      "created_at" : "Tue Feb 04 2014 13:19:06 GMT+0100 (CET)",
      "owner":
        {
          "id"       : "OWNER_ID",
          "username" : "OWNER_NICKNAME",
          "mail"     : "OWNER_MAIL",
          "avatar"   : "OWNER_AVATAR",
          "name"     : "ONWER_NAME"},
          "shared"   : []
}
```

#### PUT /calendar/shared

Cabe la posibilidad de compartir un calendario con otro usuario, para que así
pueda ver los eventos de dicho calendario. A su vez, también se puede eliminar a
un usuario de un calendario. Para ello, hay que enviar a la API la "id", del
calendario, la id del perfil del usuario a invitar, y el campo "state", que será
add, si se le quiere invitar o "remove" si se le quiere eliminar.

```json
{
  "id"      : "CALENDAR_ID",
  "profile" : "USER_ID",
  "state"   : "add"
}
```

En caso de que el calendario no exista, devuelve un error 404. En caso de que
haya ido bién devolverá el calendario actualizado. El atributo "shared"
corresponde con la lista de usuarios a los que se les ha compartido el
calendario.

```json
  calendar:
    {
      "id"         : "CALENDAR_ID",
      "name"       : "slid.us",
      "color"      : "#FF66CC",
      "created_at" : "Tue Feb 04 2014 12:52:55 GMT+0100 (CET)",
      "owner" :
        {
          "id"       : "OWNER_ID",
          "mail"     : "OWNER_MAIL",
          "username" : "OWNER_NICKNAME",
          "name"     : "ONWER_NAME",
          "avatar"   : "OWNER_AVATAR"
        },
      "shared": [ "USER_ID_1", "USER_ID_2" ]
    }
```

#### GET /calendar

Con este recurso podemos obtener todos los calendarios de los que el usuario
logueado es dueño, y aquellos que se le han compartido. Esto devuelve un "array"
de calendarios.

```json
  calendar:
    [
      {
        "id"         : "CALENDAR_ID",
        "name"       : "CALENDAR_NAME",
        "color"      : "#FF66CC",
        "created_at" : "Tue Feb 04 2014 12:52:55 GMT+0100 (CET)",
        "owner" :
          {
            "id"       : "OWNER_ID",
            "mail"     : "OWNER_MAIL",
            "username" : "OWNER_NICKNAME",
            "name"     : "ONWER_NAME",
            "avatar"   : "OWNER_AVATAR"
          },
        "shared": [ "USER_ID_1", "USER_ID_2" ]
      }
    ]
```

#### DELETE /calendar

También hay la posibilidad de eliminar un calendario, para esto se utiliza este
recurso. Únicamente hay que enviar como parámetro la "id" de dicho calendario.

```json
{
  "id": "CALENDAR_ID"
}
```

En caso de que el calendario no exista, devuelve un error 404. En caso de que
vaya bien, devuelve un mensaje indicando que todo ha ido satisfactoriamente.
```json
{
  "message": "Successful"
}
```

#### GET /calendar/activity

Appnima nos permite saber que actividades han surgido en nuestro calendario.
Para obtener la lista de ellas se utiliza este recurso y se envía como parámetro
la "id" del calendario.

```json
{
  "id": "CALENDAR_ID"
}
```

En caso de que el calendario no exista, devuelve un error 404. En caso de que
haya ido bien, nos devuelve un listado de actividades con la estructura que se
muestra a continuación

```json
  activities:
    [
      {
        "id"         : "ACTIVITY_ID",
        "message"    : "The user has created the event",
        "created_at" : "Mon Feb 10 2014 16:25:54 GMT+0100 (CET)",
        "profile"    :
          {
            "username" : "USER_NICKNAME",
            "name"     : "USER_NAME",
            "mail"     : "USER_MAIL",
            "avatar"   : "USER_AVATAR",
            "id"       : "USER_ID"
          },
        "event":
          {
            "id"          : "EVENT_ID",
            "calendar"    : "CALENDAR_ID",
            "date_init"   : "Mon Apr 14 2014 09:00:00 GMT+0200 (CEST)",
            "date_finish" : "Mon Apr 14 2014 11:00:00 GMT+0200 (CEST)",
            "name"        : "EVENT_NAME",
            "description" : "EVENT_DESCRIPTION",
            "place"       : "PLACE_ID",
            "assistents"  : ["USER_ID_1", "USER_ID_2"],
            "created_at"  : "Mon Feb 10 2014 16:25:54 GMT+0100 (CET)",
            "tags"        : ["learn"],
            "guest"       : ["USER_ID_1", "USER_ID_2", "USER_ID_3"]
          },
        "calendar":
          {
            "id"         : "CALENDAR_ID",
            "name"       : "CALENDAR_NAME",
            "color"      : "#FA58F4",
            "created_at" : "Mon Feb 10 2014 16:25:54 GMT+0100 (CET)",
            "owner"      : "OWNER_ID",
            "shared"     : []
          },
        "owner":
          {
            "id"       : "OWNER_ID",
            "username" : "OWNER_NICKNAME",
            "mail"     : "OWNER_MAIL",
            "avatar"   : "OWNER_AVATAR",
            "name"     : "OWNER_NAME"
          }
        }
      ]
```

El evento y el calendario, es dónde se ha realizado la actividad. En caso de que
el evento es null, es por que la actividad únicamente afecta al calendario. El
campo "owner" es la persona que realiza la actividad y el campo "profile", es la
persona a la que va dirigida la actividad.

#### POST calendar/event

A través de este recurso, se puede crear un evento para un calendario. Se le
debe envíar como parametro la "id" del calendario al que se desea que pertenezca
el nuevo evento, el nombre del evento, la descripción, la fecha inicial y final
en formato mm-dd-yyyy hh:mm, una string con una lista de "id" de usuarios
separados por "," que corresponde con los usuarios con los que quieres compartir
dicho evento, una string con una lista de tags separados por "," para poder
taguear el evento, la dirección de donde se va a realizar el evento, la
localidad, el país, la latitud y la longitud:

```json
{
  "calendar"    : "CALENDAR_ID",
  "name"        : "Partido de fútbol",
  "description" : "Quedada para jugar un partido de fútbol",
  "init"        : "04-14-2014 09:00",
  "finish"      : "04-14-2014 11:00",
  "address"     : "c/ San Mames",
  "locality"    : "Bilbao",
  "country"     : "España",
  "latitude"    : "23.23",
  "longitude"   : "-2.29",
  "guest"       : null,
  "tags"        : "futbol,deporte"
}
```

Esta función devuelve el nuevo evento:

```json
  event:
    {
      "id"          : "EVENT_ID",
      "calendar"    : "CALENDAR_ID",
      "date_init"   : "Mon Apr 14 2014 09:00:00 GMT+0200 (CEST)",
      "date_finish" : "Mon Apr 14 2014 11:00:00 GMT+0200 (CEST)",
      "description" : "EVENT_DESCRIPTION",
      "name"        : "EVENT_NAME",
      "place":
        {
          "address"    : "c/ San Mames",
          "locality"   : "Bilbao",
          "country"    : "Spain",
          "_id"        : "PLACE_ID",
          "created_at" : "Tue Feb 04 2014 13:49:42 GMT+0100 (CET)",
          "position"   : [ -2.29, 23.23 ]
        },
      "assistents" : [],
      "created_at" : "Tue Feb 04 2014 13:49:42 GMT+0100 (CET)",
      "tags"       : ["futbol", "sport"],
      "owner"      :
        {
          "id"       : "OWNER_ID",
          "username" : "OWNER_NICKNAME",
          "mail"     : "OWNER_MAIL",
          "avatar"   : "OWNER_AVATAR",
          "name"     : "OWNER_NAME"
        }
      }
```

#### PUT calendar/event

También se nos permite modificar un evento a través de este recurso. Se le debe
envíar un objeto que lleve como parámetros la "id" del evento que se desea
modificar, el nombre del evento, la descripción, la fecha inicial y final en
formato mm-dd-yyyy hh:mm, una string con una lista de "id" de usuarios separados
por "," que corresponde con los usuarios con los que quieres compartir dicho
evento, una string con una lista de tags separados por ",",la dirección de donde
se va a realizar el evento, la localidad, el país, la latitud y la longitud.

```json
{
  "event"       : "EVENT_ID",
  "calendar"    : "CALENDAR_ID",
  "name"        : "Partido de baloncesto",
  "description" : "Quedada para jugar un partido de baloncesto",
  "init"        : "04-14-2014 09:00",
  "finish"      : "04-14-2014 11:00",
  "address"     : "c/ San Mames",
  "locality"    : "Bilbao",
  "country"     : "Spain",
  "latitude"    : "23.23",
  "longitude"   : "-2.29",
  "guest"       : null,
  "tags"        : "futbol,sport"
}
```

En caso de que el evento no exista, devuelve un error 404. Si por el contrario
existe, devuelve el evento con los campos modificados con la estructura del
objeto que se devuelve en la función de crear evento.

#### GET calendar/event

A través de este recurso se pueden obtener los eventos de los calendarios en los
que el usuario logueado es dueño, los eventos de los calendarios que le han
compartido, y los eventos a los que se le han invitado. Se deben filtrar los
eventos por tiempo por lo que es requerido el parámetro "time", que será "month"
si se quiere obtener los eventos de un mes en concreto, por lo que habrá que
enviar también el parametro "year" con el año en formato "YYYY", y el parámetro
"month" con el mes que se desea en formato "mm". Si por el contrareo se quiere
sólo obtener los eventos de una semana, el parámetro "time" deberá tener como
valor "week", y se deberá enviar como parámetros "year", "month" y "day" que
tendrán como valor el año, mes y día de una fecha que corresponda dentro de la
semana deseada. O si por el contrario, se quiere obtener los eventos de un día
en concreto "time" deberá tener como valor "day" y se debe enviar también
"year", "month" y "day" que tendrán como valor la fecha del día deseado

```json
{
  "time"  : "day",
  "year"  : "2014",
  "month" : "04",
  "day"   : "20"
}
```

Como resultado se obtiene una lista de eventos:

```json
    events:
      [
        {
                "id"          : "EVENT_ID",
                "calendar"    : "CALENDAR_ID",
                "date_init"   : "Sun Apr 20 2014 09:00:00 GMT+0200 (CEST)",
                "date_finish" : "Thu Mar 20 2014 11:00:00 GMT+0100 (CET)",
                "name"        : "EVENT_NAME",
                "description" : "EVENT_DESCRIPTION",
                "place"       : "PLACE_ID",
                "assistents"  : [],
                "created_at"  : "Tue Feb 04 2014 14:39:04 GMT+0100 (CET)",
                "tags"        : ["dinner", "enjoy"],
                "owner":
                      {
                          "id"       : "OWNER_ID",
                          "username" : "OWNER_NICKNAME",
                          "mail"     : "OWNER_MAIL",
                          "avatar"   : "EVENT_AVATAR",
                          "name"     : "EVENT_NAME"
                      }
            }
          ]
```

#### PUT calendar/event/guest

Otra funcionalidad que es posible a través de este recurso, es la de invitar a
un usuario a un evento, para que así, él también pueda ver dicho evento. O por
el contrario, eliminar una invitación para que ese usuario deje de ver dicho
evento. Para ello, sólo hay que ejecutar la siguiente función que se muestra a
continuación, enviando como parámetros, la "id" del evento, la "id" del usuario
a invitar, y "add" o "remove", si se quiere añadir invitación, se envía "add" si
por el contrario se quiere eliminar, se envía "remove".

```json
{
  "event"   : "EVENT_ID",
  "profile" : "USER_ID",
  "state"   : "add"
}
```

En caso de que el evento no exista, devuelve un error 404. En caso de que haya
ido bién devuelve el evento actualizado. El atributo "guest" corresponde con la
lista de usuarios a los que se les ha invitado al evento.

```json
  event:
    {
      "id"          : "EVENT_ID",
      "calendar"    : "CALENDAR_ID",
      "date_init"   : "Sat Feb 15 2014 16:00:00 GMT+0100 (CET)",
      "date_finish" : "Sat Feb 15 2014 17:00:00 GMT+0100 (CET)",
      "name"        : "EVENT_NAME",
      "description" : "EVENT_DESCRIPTION",
      "place"       : "PLACE_ID",
      "assistents"  : [],
      "created_at"  : "Tue Feb 04 2014 15:10:59 GMT+0100 (CET)",
      "tags"        : ["app", "osakidetza"],
      "guest"       : ["USER_ID"],
      "owner"       :
        {
          "id"       : "OWNER_ID",
          "username" : "OWNER_NICKNAME",
          "mail"     : "OWNER_MAIL",
          "avatar"   : "OWNER_AVATAR",
          "name"     : "OWNER_NAME"
        }
      }
```

#### PUT calendar/event/assistent

Para confirmar la asistencia a un evento o para eliminarla se utiliza este
recurso. Se envía como parámetro la "id" del evento, la "id" del usuario, y
"add" o "remove". Si se quiere confirmar asistencia, se envía "add" si por el
contrario se quiere eliminar la confirmación de asistencia, se envía "remove".

```json
{
  "event"   : "EVENT_ID",
  "profile" : "USER_ID",
  "state"   : "add"
}
```

En caso de que el evento no exista, devuelve un error 404. En caso de que haya
ido bién devolverá el evento actualizado. El atributo "assistents" corresponde
con la lista de usuarios que van a asistir al evento.

```json
  event:
    {
      "id"          : "EVENT_ID",
      "calendar"    : "CALENDAR_ID",
      "date_init"   : "Sat Feb 15 2014 16:00:00 GMT+0100 (CET)",
      "date_finish" : "Sat Feb 15 2014 17:00:00 GMT+0100 (CET)",
      "name"        : "EVENT_NAME",
      "description" : "EVENT_DESCRIPTION",
      "place"       : "PLACE_ID",
      "assistents"  : ["USER_ID_1", "USER_ID_2"],
      "created_at"  : "Tue Feb 04 2014 15:25:07 GMT+0100 (CET)",
      "tags"        : ["app", "osakidetza"],
      "guest"       : [ "USER_ID_1"],
      "owner"       :
        {
          "id"       : "OWNER_ID",
          "username" : "OWNER_NICKNAME",
          "mail"     : "OWNER_MAIL",
          "avatar"   : "OWNER_AVATAR",
          "name"     : "OWNER_NAME"
        }
      }
```

#### GET calendar/event/search

Appnima te permite buscar eventos. Al utilizar este recurso se debe enviar como
parámetro una palabra, y se busca una coincidencia con dicha palabra en el
nombre y en la descripción de los eventos que tienes acceso. Es decir, aquellos
que estén en un calendario donde seas el dueño o te los hayan compartido y
aquellos eventos a los que te hayan invitado:

```json { "query": "futbol" } ```

La función devuelve una lista de eventos que cumplan dichas coincidencias:

```json
  events:
    [
      {
        "id"          : "EVENT_ID",
        "calendar"    : "CALENDAR_ID",
        "date_init"   : "Sat Feb 22 2014 11:00:00 GMT+0100 (CET)",
        "date_finish" : "Sat Feb 22 2014 12:00:00 GMT+0100 (CET)",
        "name"        : "EVENT_NAME",
        "description" : "EVENT_DESCRIPTION",
        "place"       : "PLACE_ID",
        "assistents"  : [],
        "created_at"  : "Tue Feb 04 2014 15:35:10 GMT+0100 (CET)",
        "tags"        : ["near"],
        "guest"       : [],
        "owner"       :
          {
            "id"       : "OWNER_ID",
            "username" : "OWNER_NICKNAME",
            "mail"     : "OWNER_MAIL",
            "avatar"   : "OWNER_AVATAR",
            "name"     : "OWNER_NAME"
          }
        }
      ]
```

#### DELETE calendar/event

Cabe la posibilidad de eliminar un evento, para ello basta con utilizar este
recurso enviando como parámeto la "id", del evento que se desee borrar.

```json { "id" : "EVENT_ID" } ```

En caso de que el calendario no exista, devuelve un error 404. En caso de haya
vaya bien, devuelve un mensaje indicando que todo ha ido satisfactoriamente.

```json {"message": "Successful"} ```

#### GET calendar/event/activity

Al igual que con un calendario, Appnima también nos ofrece información de qué ha
sucedido en un evento en concreto. Con este recurso enviando como parámetro la
"id" de un evento, nos ofrece una lista de actividades que han sucedido en él,
como son, modificar ese evento, invitar a alguien o quitarle de la lista de
invitados o asistencia o desasistencia de un usuario.

```json { "id" : "EVENT_ID" } ```

En caso de que el evento no exista, devuelve un error 404. En caso de que haya
ido bien, nos devuelve un listado de actividades con la estructura que se
muestra a continuación

```json
  activities:
    [
      {
        "id"         : "ACTIVITY_ID",
        "message"    : "Has invited the event to Cata",
        "created_at" : "Mon Feb 10 2014 16:56:25 GMT+0100 (CET)",
        "profile":
          {
            "username" : "USER_NICKNAME",
            "name"     : "USER_NAME",
            "mail"     : "USER_MAIL",
            "avatar"   : "USER_AVATAR",
            "id"       : "USER_ID"
          },
        "event":
          {
            "id"          : "EVENT_ID",
            "calendar"    : "CALENDAR_ID",
            "date_init"   : "Mon Apr 14 2014 09:00:00 GMT+0200 (CEST)",
            "date_finish" : "Mon Apr 14 2014 11:00:00 GMT+0200 (CEST)",
            "name"        : "EVENT_NAME",
            "description" : "EVENT_DESCRIPTION",
            "place"       : "PLACE_ID",
            "assistents"  : ["USER_ID"],
            "created_at"  : "Mon Feb 10 2014 16:56:24 GMT+0100 (CET)",
            "tags"        : [ "learn" ],
            "guest"       : [ "USER_ID_1", "USER_ID_2"]
          },
        "calendar":
          {
            "id"         : "CALENDAR_ID",
            "name"       : "CALENDAR_NAME",
            "color"      : "#FA58F4",
            "created_at" : "Mon Feb 10 2014 16:56:24 GMT+0100 (CET)",
            "owner"      : "OWNER_ID",
            "shared"     : []
          },
        "owner":
          {
            "id"       : "OWNER_ID",
            "username" : "OWNER_NICKNAME",
            "mail"     : "OWNER_MAIL",
            "avatar"   : "OWNER_AVATAR",
            "name"     : "OWNER_NAME"
          }
        }
      ]
```

El evento y el calendario, es dónde se ha realizado la actividad. El campo
"owner" es la persona que realiza la actividad y el campo "profile", es la
persona a la que va dirigida la actividad.

[Socket](id:sockets)
------

Appnima te permite trabajar con sockets donde puedes tener con salas de
conversaciones. Para ello ten en cuenta que todas las peticiones que hagas
tendrán que ir a:

``` http://socket.appnima.com/{RECURSO} ```

Recuerda que todas las peticiones que hagas a Appnima tienen que ir
identificadas con tu `Appnima.key` o bien con el par de datos `client` y
`secret`. Ahora veamos los recursos que puedes utilizar, para ello el primer
parametro indica el tipo de petición (GET, POST, UPDATE, DELETE …) y el segundo
parametro el nombre del recurso.

### Comenzando

Para la comunicación en tiempo real, appnima utiliza socket.io para conectarse a
las diferentes salas creadas. A continuación se exponen los siguientes métodos
de interacción con ellas. Aunque recomendamos el uso de nuestra librería
`Appnima.js` para esta labor.

#### open

Para crear una sala se llamará al método `open`. Esté recibirá los siguientes
parametros:

-   Token del usuario

-   Contexto (que se usará para identificar la sala para futuras conexiones)

-   Nombre de sala

-   Tipo de sala

-   Si queremos que la información de la sala sea persistente

-   Array de los ids de los usuarios permitidos

Si todo está correcto la sala se creará y automáticamente el autor estará
conectado a ella.

#### join

Un usuario puede conectarse a una sala siempre y cuando ya esté creada y tenga
permiso de conexión a la misma. Para ello se llamará al método `join` y se le
pasarán los siguientes parámetros:

-   Token del usuario

-   Contexto (identificador de la sala a la que se quiere conectar)

-   Tipo (tipo de la room a la que se quiere conectar)

#### leave

En caso de querer desconectarse de una sala se llamará al método `leave`.

#### sendMessage

Para enviar un mensaje a una sala basta con llamar al método `sendMessage` y
pasarle como parámetro un objeto (el mensaje puede ser cualquier tipo de dato).
Este mensaje llegará a todos los usuarios conectados a la sala, emisor incluido,
se recibirá a través del listener `onMessage` y tendrá el siguiente formato:

```json
{
  "user"       : "USER_ID",
  "message"    : "Mensaje enviado",
  "created_at" : "2013-05-23T12:01:02.736Z"
}
```

#### broadcastMessage

Este método funciona igual que el método `sendMessage` con la única diferencia
de que el emisor no recibirá el mensaje enviado.

#### allowUsers

Se pueden añadir usuarios a la lista de admitidos a través del método
`allowUsers`, para ello bastará con llamar a este método y pasarle un array con
los ids de los usuarios a admitir.

#### disallowUsers

Se pueden eliminar usuarios de la lista de admitidos a través del método
`disallowUsers`, para ello bastará con llamar a este método y pasarle un array
con los ids de los usuarios a eliminar de la lista de admitidos.

### Tipos y permisos

Existen distintos tipos de salas, cada una tiene unos permisos distintos
dependiendo del usuario.

-   Privadas: Solo se pueden conectar, leer y escribir usuarios que se
    encuentren en la lista de admitidos.

-   Públicas: Todo el mundo se puede conectar y leer, pero solo el dueño puede
    publicar en ella.

-   Inbox: Solo el dueño puede leer lo que se escribe, pero cualquier usuario
    puede ecribir.

-   Aplicación: Esta sala existe en todas las aplicaciones, por lo que nadie
    tiene que crearla, todos pueden conectarse, leer y escribir en ella.

### Peticion HTTP

#### GET /rooms

Un usuario puede obtener el listado de salas de sockets en las que participa,
para ello bastaría con llamar al recurso y se obtendría un mensaje `200 Ok`
junto con un listado como el siguiente:

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

WebRTC
------

Appnima dispone de un servidor que se encarga de gestionar conexiones entre
usuarios de appnima para así poder realizar comunicaciones webRTC.

``` http://webrtc.appnima.com/ ```

WebRTC (Web Real-Time Communication) es una API que está siendo elaborada por la
World Wide Web Consortium (W3C) para permitir a las aplicaciones del navegador
realizar llamadas de voz, chat de vídeo y uso compartido de archivos P2P sin
necesidad de instalar ningún plugin.

### Comenzando

Para crear una conexión webRTC Appnima dispone de un servidor socket.io que se
encarga de realizar el `signaling` entre 2 usuarios de appnima. Se recomienda
utilizar la libreria `Appnima.js` para este propósito, o en su defecto, utilizar
la libreria de cliente de socket.io.

Más información sobre [socket.io][9]

[9]: <http://socket.io/>

### Métodos de llamada al servidor WebRTC:

#### connect

Para conectar con el servidor de WebRTC se llamará al método `connect`. Este
recibirá un único parámetro que será el token proporcionado en el login de
usuario.

Una vez se haya conectado el usuario, podrá tanto realizar llamadas como recibir
llamadas de otros usuarios.

#### offer

Para solicitar una llamada a otro usuario se debe utilizar el método `offer` y
para ello se le pasarán los siguientes parámetros:

-   Token del usuario (obligatorio)

-   Id de usuario al que se quiere llamar (obligatorio)

-   SessionDescription: objeto devuelto mediante el método de la API de WebRTC
    PeerConnection.createOffer (obligatorio).

#### answer

Para aceptar o responder a una llamada de otro usuario se debe utilizar el
método `answer` y para ello se le pasarán los iguientes parámetros:

-   Token del usuario (obligatorio)

-   Id de usuario del que quiere responder la llamada (obligatorio)

-   SessionDescription: objeto devuelto mediante el método de la API de WebRTC
    PeerConnection.createAnswer (obligatorio). Debe enviarse el objeto completo
    y serializado: JSON.serialize(objetoDescription)

#### ice

Una vez creada la conexión entre dos usuarios, el usuario que recibe la llamada
deberá emitir al servidor los cambios de sus servidores "ice" asociados a su
conexión P2P.

La API de webRTC dispone de un evento `onicecandidate` al que el cliente deberá
suscribirse y emitir la información recibida a este método `ice` con los
siguientes parámteros:

-   Token del usuario (obligatorio)

-   Candidato: El parámetro candidate del objeto recibido en la suscripcion a
    `onicecandidate`. Este objeto debe enviarse serializado.

### Métodos de vuelta desde el servidor WebRTC:

#### offer

El usuario recibe una oferta de conversación de algún amigo suyo. Como parámetro
recibirá la información del usuario

#### answer

Cuando un amigo acepta una conversación, este evento será lanzado en el cliente
que ha realizado la llamada. Dispondrá como parámetro el amigo que ha contestado
serializado.

#### connected

Se ha conectado algún amigo. Como parámetro llegará la información de este amigo
serializada.

#### disconnected

Se ha desconectado algún amigo. Como parámetro llegará la información de este
amigo serializada.

#### ice

Cuando hay cambios de los servidores del que recibe la llamada, al que ha
realizado la llamada le llegará la información de los nuevos servidores ICE.

#### error

Método lanzado cuando ocurre algún error en el servidor.

[Push](id:push)
----

Con este módulo puedes enviar notificaciones a los terminales registrados de los
usuarios de tu aplicación.

``` http://api.appnima.com/push{RECURSO} ```

Recuerda que todas las peticiones que hagas a Appnima tienen que ir
identificadas con tu `Appnima.key` o bien con el par de datos `client` y
`secret`. Ahora veamos los recursos que puedes utilizar, para ello el primer
parametro indica el tipo de petición (GET, POST, UPDATE, DELETE …) y el segundo
parametro el nombre del recurso.

#### POST /push

Envía notificaciones push mediante este recurso. Junto con la petición, envía
los siguientes parámetros:

```json
{
  "user"    : "USER_ID",
  "title"   : "Texto a mostrar en la notificación",
  "message" : "Es mi message"
}
```

En caso de éxito se devolverá el código `200 Ok`.
