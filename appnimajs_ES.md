APPNIMA.JS
====
Utiliza appnima.js para realizar de forma sencilla las peticiones a los recursos de APP/NIMA.


Registrar un usuario
------
Para registrar un usuario en tu aplicación únicamente necesitas pasar como parámetros junto con la petición su e-mail, password y opcionalmente el username:

Appnima.User.signup `javi@tapquo.com`, `USER_PASSWORD`


Oauth
------
Recuerda que son necesarias las credenciales del usuario para todas las peticiones a App/nima. Obtén el token mediante la cabecera  "http authorization basic"(con el client_id:client_secret codificados en Base64):

```Authorization: basic client_id:client_secret```

```json
    {
        grant_type: "password",
        mail:       "javi@tapquo.com",
        password:   "USER_PASSWORD"
    }
```

Login 
------
Realizar el login de un usuario es tan sencillo como llamar al recurso con los parámetros mail y password:

`Appnima.User.login javi@tapquo.com, USER_PASSWORD`

Información sobre el usuario
------
¿Necesitas todos los datos registrados de tu usuario? Gracias al protocolo de autenticación OAuth2 no es necesario ningún parámetro junto a la petición. Espera la respuesta `200 Ok` y obtendrás un objeto como el que sigue:

`Appnima.User.info()`

El objeto que obtienes es como el siguiente:

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



Actualizar información del usuario
------
Puedes actualizar uno o varios campos de datos de tu usuario con este recurso. Envía junto a la petición qué parámetros deseas cambiar:

`Appnima.User.info avatar "http://NEW_USER_AVATAR_URL"`



Cambiar password
------
Para cambiar el password del usuario, utiliza este recurso enviando la nueva clave:

`Appnima.User.password USER_PASSWORD`


Subir fichero como avatar
------
Tus usuarios pueden configurar su avatar mediante URL o subir un fichero desde su equipo. Para que puedan utilizar una imagen de su galería utiliza este recurso pasando el fichero codificado en base 64:

`Appnima.User.avatar USER_AVATAR`


Registrar un dispositivo
------
Con este recurso puedes registrar el dispositivo con el que tu usuario accede a tu aplicación. La petición se realiza de la siguiente forma:

`Appnima.User.terminal "Android", "Phone", "MobilePhone", "4.1"`

Información sobre dispositivos
------
Conoce con qué dispositivos se ha accedido a tu aplicación llamando este recurso. De la misma forma que con **GET /user/info** no necesitas enviar ningún parámetro junto con la petición.

`Appnima.User.teminal()`

La información que recibirás será de la forma:

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

Actualizar un dispositivo
------
Cambia los datos respecto a un dispositivo registrado mediante este recurso pasando los parámetros a actualizar:



Servicio de suscripción
------
Si lo necesitas, ofrece a tus usuarios que registren sus direcciones de e-mail para suscripciones, invitaciones, demos… El recurso únicamente necesita como parámetro el e-mail:

`Appnima.User.subscribe "javi@tapquo.com"`


Tickets
------
Utiliza este recurso como sistema de gestión de tickets para la resolución de las consultas e incidencias de tus usuarios. La petición únicamente necesita el texto de la consulta:

`Appnima.User.ticket "[SUGGESTION] Bigger buttons"`


**MENSAJERÍA**


Enviar e-mails
------
Los usuarios de tu aplicación pueden enviar e-mail a otros usuarios del sistema mediante este recurso. Para ello, los parámetros que recibe son: el ID del usuario al que se le envía el e-mail, el asunto y el cuerpo del mensaje:

`Appnima.Messenger.mail "28319319832". "Meeting", "Tomorrow morning"`

Enviar SMS
------
App/nima también proporciona servicio de SMS. Así que para utilizar este recurso se necesita como parámetros el ID del usuario destinatario y el mensaje:

`Appnima.Messenger.sms "28319319832". "Remember that your appointment is tomorrow"`


Enviar mensajes privados
------
para utilizar el sistema de mensajería privado de App/nima en tu aplicación, basta con llamar al recurso pasando como parámetros los siguientes datos: ID del usuario al que se le envía el mensaje, el cuerpo del mensaje y opcionalmente al asunto del mensaje:

`Appnima.Messenger.message "28319319832". "Where are you?"`


Leer mensajes recibidos
------
Los mensajes intercambiados entre los usuarios de tu aplicación se pueden visualizar utilizando este recurso. Obtén la lista de mensajes recibidos llamando al recurso de la siguiente forma:

`Appnima.Messenger.messageInbox()`

Leer mensajes enviados
------
De la misma forma que con los mensajes recibidos, para obtener la lista de mensajes que se ha enviado, el recurso se llama de la siguiente forma:

`Appnima.Messenger.messageOutbox()`

Marcar mensaje como leído
------
Para marcar un mensaje como leído basta con llamar este recurso de la siguiente forma:

`Appnima.Messenger.readMessage "28319319832"`


Eliminar un mensaje
------

Elimina un mensaje llamando al recurso de la siguiente forma:

`Appnima.Messenger.deleteMessage "28319319832"`


**RELACIONES**

Estado de relaciones de un usuario
------
Con este recurso puedes obtener una visión general del estado de relaciones de un usuario. Puedes conocer de forma ágil cuantos seguidores tiene y a cuantas personas sigue. Esta información la puedes obtener de cualquier usuario de tu aplicación si pasas como parámetro su ID:

`Appnima.Network.info "28319319832"`

Si no pasas ningún parámetro lo que obtendrás es la lista del usuario logueado:

`Appnima.Network.info()`


Lista de usuarios a los que se sigue
------
Con este recurso puedes obtener la lista de persona a las que tu usuario sigue o puedes obtener la lista de otro usuario. Al llamar al recurso de la forma:

`Appnima.Network.following()`

Obtienes la lista de tu usuario loqueado. Si llamas al recurso pasando como parámetro la ID de algún usuario de tu aplicación, obtendrás su lista:

`Appnima.Network.following "28319319832"`


Lista de usuarios seguidores
------
De la misma forma que lo anterior, puedes utilizar este recurso de dos maneras: si no pasas parámetro obtienes la lista de seguidores del usuario loqueado:

`Appnima.Network.followers()`


Si pasas la ID de un usuario de tu plataforma obtienes su lista de seguidores:

`Appnima.Network.followers "28319319832"`


Seguir a un usuario
------
Para seguir a un usuario hay que llamar a este recurso junto con la ID del usuario al que se quiere seguir:

`Appnima.Network.follow "28319319832"`


Dejar de seguir a un usuario
------
Tan sencillo como el recurso anterior, para dejar de seguir a un usuario basta con pasar el ID del usuario:

`Appnima.Network.unfollow "28319319832"`


Estado entre dos usuarios
------
Con este recurso puedes obtener información respecto a la relación entre dos usuarios, saber si alguno está en la lista de seguidores o de personas a las que sigue. Para ello llama al recurso de la siguiente forma:

`Appnima.Network.check "28319319833"`


Buscar usuarios
------
Los usuarios de tu aplicación pueden buscar a otros usuarios mediante este recurso. Para ello puedes pasar como parámetro el e-mail o el nickname del usuario que se quiere encontrar:

`Appnima.Network.search "javi@tapquo.com"`


**GEOLOCALIZACIÓN**

Buscar lugares o establecimientos
------
Mediante la latitud y longitud de tu usuario o de un punto cualquiera puedes obtener sitios cercanos como museos, restaurantes, edificios públicos,…. Además puedes proporcionar el radio para que la búsqueda sea más acotada. Para ello utiliza el recurso pasando como parámetros latitud, longitud y radio:

`Appnima.Location.places "43.6525842", "-79.3834173, 100"`


Información detallada de un sitio
------
Obtén información detallada de un establecimiento o lugar utilizando este recurso. Junto a la petición envía los parámetros `ID` y `REFERENCE` del sitio:

`Appnima.Location.place "28319319833", "CqQBlwAAAEXdx350jL2InIRtksTkbZJ-m"`


Añade un sitio
------
En App/nima puedes agregar un sitio y añadir información relevante como el nombre, la dirección, el teléfono,… Para ello tienes que pasar como mínimo la información de "name", "address", "locality", "postal_code", "country", "latitude" y "longitude":

`Appnima.Location.add "Talleres Juan", "C/ Laubide 10", "Mungia", "48100", "ES", "43.354", "-2.8467"`

Opcionalmente puedes añadir información sobre e-mail, teléfono y Web del sitio.

Registrar visita
------
Con App/nima tus usuarios pueden registrar que están en un establecimiento o lugar. Junto a la petición envía los parámetros `ID` y `REFERENCE` del sitio:

`Appnima.Location.checkin "28319319833", "CqQBlwAAAEXdx350jL2InIRtksTkbZJ-m"`


















