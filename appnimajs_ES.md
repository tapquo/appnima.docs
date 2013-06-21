APPNIMA.JS
====
Utiliza appnima.js para realizar de forma sencilla las peticiones a los recursos de APP/NIMA.

Configuración
-------------
Para tener appnima.js listo para trabajar solo tienes que fijar el valor de la variable `Appnima.key` la key que te proporciona appnima al crear una aplicación:
`Appnima.key = "fIiyFiBiufbifiBiu4iuiuGBIGbiUIUiIbnobyOlhPNXB5R3FoRmhIYFKkhfUYVKVhfIGUu"`


Registrar un usuario
------
Para registrar un usuario en tu aplicación únicamente necesitas pasar como parámetros junto con la petición su e-mail, password y opcionalmente el username:

Appnima.User.signup `javi@tapquo.com`, `USER_PASSWORD`


Login 
------
Realizar el login de un usuario es tan sencillo como llamar al recurso con los parámetros mail y password:

`Appnima.User.login javi@tapquo.com, USER_PASSWORD`


Storage
------
App/nima te proporciona recursos para mantener en memoria la información de sesión de tu usuario



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


Listar sitios visitados
------
Obtén la lista de lugares registrados por un usuario. Para esta petición no es necesario añadir ningún parámetro, utiliza este recurso de la siguiente forma:

`Appnima.Location.checkins "28319319833"`


Amigos cercanos
------
Si lo necesitas App/nima puede ofrecer una lista de amigos cercanos a un punto determinado, personas que están registradas en la lista de followers o followings del usuario que consulta. Para ello, realiza la petición junto con la latitud y la longitud y opcionalmente el radio (así acotarás la búsqueda):

`Appnima.Location.friends "43.6525842", "-79.3834173, 100"`


Personas cercanas
------
App/nima también te permite obtener un listado de personas fuera de las listar de followings y followers cercanas al usuario que consulta. La petición es similar a la anterior:

`Appnima.Location.people "43.6525842", "-79.3834173, 100"`


**PUSH**

Notificaciones PUSH
------
Para enviar notificaciones push a los dispositivos registrados de tus usuarios únicamente necesitas enviar la ID del usuario, el texto que quieres trasmitir y el content:

`Appnima.Push.send "28319319833", "Mensaje", {"title": "JSON con los campos necesarios", "text": "Hola App/nima!"}`


**SOCKET**


Salas que participa un usuario
------
Obtén las salas donde un usuario está participando cuando utiliza el recurso de sockets. El recurso no necesita parámetros al ser llamado:

`Appnima.Socket.`

El objeto que se obtienes es como sigue:

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

Grupos
------
Los grupos son salas persistentes de 1 a N usuarios con persistencia. Para trabajar con grupos se debe crear una instancia de grupo:

`group = new Appnima.Socket.Group();`

* Creación:

	`group.create("name", ["2387569328yvc2","21y89ch3x8hg2032","2938tyh2g0ghh0i2bg8"]);`

* Listar mis grupos:

	`group.list().then(function(error, groups){[code]});`

* Eliminar:

	`group.remove("id")`
	
Chat
----
Los chats son salas de 1 a N personas sin persistencia. Para trabajar con Chtas primero se debe crear una instancia de Chat:

`chat = new Appnima.Socket.Chat()`

* Creación:

`chat.create("name", ["2387569328yvc2","21y89ch3x8hg2032","2938tyh2g0ghh0i2bg8"])`

Emiter
------
El emiter es una sala en la que el autor de la misma es el unico que puede mandar mensajes y los receptores se deben conectar usando `Appnima.Socket.Listener`. Para crearlo primero hay que crear una instancia de Emiter:

`room = new Appnima.Socket.Emiter()`

* Creación:

`room.create("context")`


Listener
--------
El listener se conecta a las salas creadas por un emiter y recibe los mensajes que envía. Para crearlo primero hay que crear una instancia de Listener, dicha instancia se conectará automáticamente a la sala:

`listener = new Appnima.Socket.Listener("context")`

Application
-----------
El canal de aplicación permite a los usuarios de una aplicación comunicarse entre ellos. Para conectarse al canal solo hace falta crear una instancia del socket de aplicación:

`application = new Appnima.Socket.Application()`

Inbox
-----
El Inbox permite a un usuario recibir mensajes de otros usuarios. Para ello crearemos una instancia de Inbox:

`inbox = new Appnima.Socket.Inbox()`

User
----
Socket.User permite el envío de mensajes al inbox de un user. Para ello primero tenemos que crear una instancia:

`user = new Appnima.Socket.User("user id")`

Métodos de Socket
-----------------
Estos métodos son los necesarios para la gestión de los tipos de socket vistos anteriormente:

* `instance.connect("id")`: Permite conectarse a una sala de Group o Chat
* `instance.disconnect()`: Se desconecta de una sala
* `instance.allowUSers(["","",""…])`: Permite añadir usuarios permitidos en Groups y Chats
* `instance.disallowUsers(["","",""…])`: Elimina usuarios de la lista de admitidos
* `instance.send(message)`: Envía un mensaje a todos los usuarios de la sala (emisor incluido)
* `instance.broadcast(message)`: Envía un mensaje a todos los usuarios de la sala excepto al emisor
* `instance.onConnect(callback)`: Llama al callback cuando se conecta a la sala
* `instance.onError(callback)`: Llama al callback cuando sucede un error
* `instance.onMessage(callback)`: Llama ala callback cuando se recibe un mensaje













