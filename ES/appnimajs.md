App/nima.JS
===========
Descubre como dar alma a tus aplicaciones con el primer LaaS en el mundo.

*Version Actual: [1.0.0]()*

Para tener appnima.js listo para trabajar solo tienes que fijar el valor de la variable `Appnima.key` con la key que te proporciona APP/NIMA al crear una aplicación:

    Appnima.key = "TU_CLAVE";

App/nima te proporciona recursos para mantener en memoria la información de sesión de tu usuario

Promesas
--------
Appnima.js usa el patrón promesa para gestionar cada petición, por lo que para vincular un callback (el cual tendrá error y result) a cada petición lo haremos usando `.then` como se muestra a continuación:

    Appnima.resource().then(function(error, result){
        if(error !== null){
            console.log("Error", error);
        }else{
            console.log("Success", result);
        }
    });


Manejando errores
-----------------
Appnima.js tiene la capacidad de poder centralizar la gestión de los errores en un solo método. Para ello basta con asignarle a Appnima.onError una funcion de callback que recibirá el error generado:

    Appnima.onError = function(error){
        console.log("CODE: ", error.code);
        console.log("TYPE: ", error.type);
        console.log("MESSAGE: ", error.message);
    };

Gestión de Token
----------------
App/nima usa un token para acceder a los recursos. Cuando el token expire el servidor devolverá un código de error `480`, para refrescar el token hay que llamar al método `Appnima.User.token()`. Este se encargará de hacer todas las transacciones necesarias para poder seguir trabajando.


User
====
Basic
-----
#### Signup
Para registrar un usuario en tu aplicación únicamente necesitas pasar como parámetros junto con la petición su e-mail, password y opcionalmente el username:

    Appnima.User.signup("javi@tapquo.com", "USER_PASSWORD");


#### Login
Realizar el login de un usuario es tan sencillo como llamar al recurso con los parámetros mail y password:

    Appnima.User.login("javi@tapquo.com", "USER_PASSWORD");


#### Logout
Si quieres desloguear a un usuario de tu aplicación tan sencillo como llamar al método *logout* sin ningún parametro:

    Appnima.User.logout();


#### Información
¿Necesitas todos los datos registrados de tu usuario? Simplemente llama a este recurso para obtener información:

    Appnima.User.info();


Por otro lado, puedes obtener los datos de cualquier usuario de App/nima enviando la id de dicho usuario al método anterior:

    Appnima.User.info(12345678943);


El objeto que obtienes en ambos casos, es como el siguiente:

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


#### Actualizar
Puedes actualizar uno o varios campos de datos de tu usuario con este recurso. Envía junto a la petición qué parámetros deseas cambiar:

    data = {
        mail: "inigo@tapquo.com",
        username: "haas85",
        name: "Inigo",
        avatar: "http://AVATAR_URL",
        picture: "http://PICTURE_URL",
        phone: "29306832906",
        site: "http://tapquo.com",
        bio: ""
    }

    Appnima.User.update(data);


#### Cambiar Password
Para cambiar el password del usuario, utiliza este recurso enviando la clave antigua y la nueva clave:

    Appnima.User.password(OLD_PASSWORD, NEW_PASSWORD);


#### Avatar
Tus usuarios pueden subir su propio fichero de avatar desde su equipo. Para subir una imagen utiliza este recurso pasando el fichero codificado en Base64:

    Appnima.User.avatar(USER_AVATAR);

Terminal
--------
#### Registrar/Actualizar
Con este recurso puedes registrar o actualizar el dispositivo con el que tu usuario accede a tu aplicación. La petición se realiza de la siguiente forma:

    Appnima.User.terminal("Android", "Phone", "MobilePhone", "4.1");

#### Información
Obten los terminales con los que el usuario ah accedido a la aplicación utilizando este recurso:

    Appnima.User.teminal();

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


Suscripción
-----------
Si lo necesitas, puedes solicitar a tus usuarios que registren sus direcciones de e-mail para obtener una invitación a la aplicación. El recurso únicamente necesita como parámetro el e-mail:

    Appnima.User.subscribe("javi@tapquo.com");


Tickets
-------
Utiliza este recurso como sistema de gestión de tickets para la resolución de las consultas e incidencias de tus usuarios. La petición únicamente necesita el texto de la consulta:

    Appnima.User.ticket("[SUGGESTION] Botones más grandes");


Messenger
=========
Mail
----
Los usuarios de tu aplicación pueden enviar e-mail a otros usuarios del sistema mediante este recurso. Para ello, los parámetros que recibe son: el ID del usuario al que se le envía el e-mail, el asunto y el cuerpo del mensaje:

    Appnima.Messenger.mail("28319319832". "Reunion", "Hoy a las 5");


SMS
---
APP/NIMA también proporciona servicio de SMS. Así que para utilizar este recurso se necesita como parámetros el ID del usuario destinatario y el mensaje:

    Appnima.Messenger.sms("28319319832". "Recuerda tu cita de la tarde");


Mensajes
--------

#### Enviar
Para utilizar el sistema de mensajería privada de APP/NIMA en tu aplicación, basta con llamar al recurso pasando como parámetros los siguientes datos: ID del usuario al que se le envía el mensaje, el cuerpo del mensaje y opcionalmente al asunto del mensaje:

    Appnima.Messenger.message("28319319832". Done estas? Estoy aquí esperando.", "Donde estas?");


#### Recibidos
Los mensajes intercambiados entre los usuarios de tu aplicación se pueden visualizar utilizando este recurso. Obtén la lista de mensajes recibidos llamando al recurso de la siguiente forma:

    Appnima.Messenger.messageInbox();

#### Enviados
De la misma forma que con los mensajes recibidos, para obtener la lista de mensajes que se ha enviado, el recurso se llama de la siguiente forma:

    Appnima.Messenger.messageOutbox();

#### Marcar como leído
Para marcar un mensaje como leído basta con llamar al recurso pasando el id del mensaje como parámetro:

    Appnima.Messenger.readMessage("28319319832");


#### Eliminar
Elimina un mensaje llamando al recurso pasando el id del mensaje como parámetro:

    Appnima.Messenger.deleteMessage("28319319832");

Posts
--------
#### Post
Los usuarios pueden crear mensajes (post) o modificar los que hayan creado dentro de una aplicación. Para ello tienen que mandar los siguientes parámetros junto con la petición:

    data = {
        id: POST_ID
        title: "Lorem Ipsum"
        content:  "Lorem ipsum dolor sit amet, consectetur adipisicing elit."
        image: "http://IMAGE_URL"
    }

    Appnima.Messenger.post(data);

Solo el campo ```content``` es obligatorio. Si no se manda el campo ```id```, se creará el post; en caso contrario, se actualizará dicho post.

#### Comentarios
Un post puede tener comentarios.

Añadir un comentario a un post, pasandole el id del post y el texto del comentario:

	Appnima.Messenger.addComment("324685348953", "Lorem impsum dolor sit...")

Obtener comentarios de un post en concreto, pasando el id de el post en cuestión:

	Appnima.Messenger.postComment("53485u452395")
	
Eliminar el comentario, pasando como párametro el id del comentario:

	Appnima.Messenger.deleteComment("837456459643")

Relaciones
==========
#### Seguir
Para seguir a un usuario hay que llamar a este recurso junto con la ID del usuario al que se quiere seguir:

    Appnima.Network.follow("28319319832");


#### Dejar de seguir
Tan sencillo como el recurso anterior, para dejar de seguir a un usuario basta con pasar el ID del usuario:

    Appnima.Network.unfollow("28319319832");


#### Siguiendo
Con este recurso puedes obtener la lista de persona a las que tu usuario sigue o puedes obtener la lista de otro usuario. Al llamar al recurso de la forma:

    Appnima.Network.following():

Obtienes la lista de tu usuario loqueado. Si llamas al recurso pasando como parámetro la ID de algún usuario de tu aplicación, obtendrás su lista:

    Appnima.Network.following("28319319832");

Por otro lado, también existe la opción de que te devuelva la lista de gente a la que sigues con paginación; esto es, que en cada llamada a la API te vaya devolviendo parte de la lista de usuarios. Para ello unicamente se debe enviar dos variables más junto con la id del usuario del que quieres obtener los datos:

    Appnima.Network.following("28319319832", 0, 4);

El primer valor se trata del número de página que deseas obtener; esto es, el trozo de la lista de usuarios que deseas. La segunda variable es el numero de resultados que quieres obtener. En la primera llamada, esa variable será multiplicada por 2, y en los demás casos, se devulverá dicha cifra de usuarios.

#### Seguidores
De la misma forma que lo anterior, puedes utilizar este recurso de dos maneras: si no pasas parámetro obtienes la lista de seguidores del usuario loqueado:

    Appnima.Network.followers();

Si pasas la ID de un usuario de tu plataforma obtienes su lista de seguidores:

    Appnima.Network.followers("28319319832");

Al igual que en lo explicado anteriormente, también existe la posibilidad de obtener los resultados con paginación. El modo de uso es igual que en la obtención de los usuarios a los que sigues.

Hay que mencionar, que en este caso, cada objeto del array devolverá un campo más indicando si el usuario logueado sigue a esa persona o no.

#### Información
Con este recurso puedes obtener una visión general del estado de relaciones de un usuario. Puedes conocer de forma ágil cuantos seguidores tiene y a cuantas personas sigue, a la vez que obtienes la lista de ambos. Esta información la puedes obtener de cualquier usuario de tu aplicación si pasas como parámetro su ID:

    Appnima.Network.info("28319319832");

Si no pasas ningún parámetro lo que obtendrás es la lista del usuario logueado:

    Appnima.Network.info();


#### Estado
Con este recurso puedes obtener información respecto a la relación entre dos usuarios, saber si alguno está en la lista de seguidores o de personas a las que sigue. Para ello llama al recurso de la siguiente forma:

    Appnima.Network.check("28319319833");


#### Buscar
Los usuarios de tu aplicación pueden buscar a otros usuarios mediante este recurso. Para ello puedes pasar como parámetro el e-mail o el nickname del usuario que se quiere encontrar:

    Appnima.Network.search("javi@tapquo.com");



Geolocalización
===============
Lugares
-------
#### Buscar
Mediante la latitud y longitud de tu usuario o de un punto cualquiera puedes obtener sitios cercanos como museos, restaurantes, edificios públicos,…. Además puedes proporcionar el radio para que la búsqueda sea más acotada. Para ello utiliza el recurso pasando como parámetros latitud, longitud y radio:

    Appnima.Location.places("43.6525842", "-79.3834173, 100");


#### Información
Obtén información detallada de un establecimiento o lugar utilizando este recurso. Junto a la petición envía los parámetros `ID` y `REFERENCE` del sitio:

    Appnima.Location.place("28319319833", "CqQBlwAAAEXdx350jL2InIRtksTkbZJ-m");


#### Añadir
En APP/NIMA puedes agregar un sitio y añadir información relevante como el nombre, la dirección, el teléfono,… Para ello tienes que pasar como mínimo la información de "name", "address", "locality", "postal_code", "country", "latitude" y "longitude":

    Appnima.Location.add("Talleres Juan", "C/ Laubide 10", "Mungia", "48100", "ES", "43.354", "-2.8467");

Opcionalmente puedes añadir información sobre e-mail, teléfono y Web del sitio.

#### Checkin
Con APP/NIMA tus usuarios pueden registrar que están en un establecimiento o lugar. Junto a la petición envía los parámetros `ID` y `REFERENCE` del sitio:

    Appnima.Location.checkin("28319319833", "CqQBlwAAAEXdx350jL2InIRtksTkbZJ-m");

Obtén la lista de check ins de un usuario. Para esta petición sólo es necesario el id del usuario:

    Appnima.Location.checkins("28319319833");


Personas
--------
#### Amigos
Si lo necesitas APP/NIMA puede ofrecer una lista de amigos que se encuentran cerca, para ello es necesaria la latitud, longitud y el redio de busqueda:

    Appnima.Location.friends("43.6525842", "-79.3834173, 100");


#### Desconocidos
APP/NIMA también te permite obtener un listado de personas cercanas al usuario que consulta. La petición es similar a la anterior:

    Appnima.Location.people("43.6525842", "-79.3834173, 100");



Push
====
Para enviar notificaciones push a los dispositivos registrados de tus usuarios únicamente necesitas enviar la ID del usuario, el texto de la notificación y el contenido:

    Appnima.Push.send("28319319833", "Mensaje", {"title": "JSON con los campos necesarios", "text": "Hola App/nima!"});



Socket
======
Grupos
------
Los grupos son salas persistentes de 1 a N usuarios con persistencia. En los grupos puede conectarse y enviar datos aquellos usuarios a los que se les haya dado permisos. Para trabajar con grupos se debe crear una instancia de grupo:

    group = new Appnima.Socket.Group();

Creación:

    group.create("name", ["2387569328yvc2","21y89ch3x8hg2032","2938tyh2g0ghh0i2bg8"]);

Listar mis grupos:

    group.list().then(function(error, groups){[code]});

Eliminar:

    group.remove("id");

Cambiar nombre de grupo:

    group.rename("id", "name");

Obtener los mensajes del grupo. El segundo parametro indica el numero de la página que se quiere obtener, siendo 0 la primera página, de esta forma los mensajes irán llegando de 50 en 50:

    group.messages("id", 0);

Eliminar el contador de mensajes no leídos de ese grupo, el primer parametro debe de ser la id del grupo:

    group.deleteUnreadCount("id", callback);


Chat
----
Los chats son salas de 1 a N personas sin persistencia. Al igual que en los grupos en los chats puede conectarse y enviar datos aquellos usuarios a los que se les haya dado permisos.Para trabajar con Chats primero se debe crear una instancia de Chat:

    chat = new Appnima.Socket.Chat();

Creación:

    chat.create("name", ["2387569328yvc2","21y89ch3x8hg2032","2938tyh2g0ghh0i2bg8"]);


Emisor
------
El emiter es una sala en la que el autor de la misma es el unico que puede mandar mensajes y los receptores se deben conectar usando `Appnima.Socket.Listener`. Para crearlo primero hay que crear una instancia de Emiter:

    room = new Appnima.Socket.Emiter();

Creación:

    room.create("context");


Oyente
------
El listener se conecta a las salas creadas por un emiter y recibe los mensajes que envía. Para crearlo primero hay que crear una instancia de Listener, dicha instancia se conectará automáticamente a la sala:

    listener = new Appnima.Socket.Listener("context");

Aplicación
----------
El canal de aplicación permite a los usuarios de una aplicación comunicarse entre ellos. Para conectarse al canal solo hace falta crear una instancia del socket de aplicación:

    application = new Appnima.Socket.Application();


Inbox
-----
El Inbox permite a un usuario recibir mensajes de otros usuarios. Los mensajes solo los recibirá el que crea la sala, pero cualquier usuario de la aplicación podrá escribirle. Para ello crearemos una instancia de Inbox:

    inbox = new Appnima.Socket.Inbox();

Obtener el número de mensajes no leídos por grupos:

    inbox.unreadCount(callback);
    
Amigos que están conectados:

	inbox.onlineUsers(callback);
	
Conexión de amigo:

	inbox.friendConnection(callback);
	
Desconexión de amigo:

	inbox.friendDisconnection(callback);
	
Enviar datos a seguidores:

	inbox.sendToFollowers(data);
	
Enviar datos a amigos:

	inbox.sendToFriends(data);
	
Enviar datos a un usuario en concreto:

	inbox.sendToUser(user_id, data);

Usuario
-------
Socket.User permite el envío de mensajes al inbox de un user. Para ello primero tenemos que crear una instancia:

    user = new Appnima.Socket.User("3907390629397");


Métodos
-------
Estos métodos son los necesarios para la gestión de los tipos de socket vistos anteriormente:

* `instance.connect("id")`: Permite conectarse a una sala de Group o Chat

* `instance.disconnect()`: Se desconecta de una sala

* `instance.allowUSers(["","",""…])`: Permite añadir usuarios permitidos en Groups y Chats

* `instance.disallowUsers(["","",""…])`: Elimina usuarios de la lista de admitidos

* `instance.send(message, optionalData)`: Envía un mensaje a todos los usuarios de la sala (emisor incluido). El segundo parametro es opcional y se utiliza para enviar un objeto con datos extra que nos podría interesar mandar.

* `instance.broadcast(message, optionalData)`: Envía un mensaje a todos los usuarios de la sala excepto al emisor. El segundo parametro es opcional y se utiliza para enviar un objeto con datos extra que nos podría interesar mandar.

* `instance.broadcast(message)`: Envía un mensaje a todos los usuarios de la sala excepto al emisor

* `instance.onConnect(callback)`: Llama al callback cuando se conecta a la sala

* `instance.onError(callback)`: Llama al callback cuando sucede un error

* `instance.onMessage(callback)`: Llama al callback cuando se recibe un mensaje, el mensaje será un objeto con los atributos:
    * content: "Contenido del mensaje"
    * user: {"Usuario que lo envía"}
    * data: {"Información extra enviada por el emisor"}
    * created_at: "2013-11-16T05:55:02.736Z"

* `instance.onDisallow(callback)`: Llama al callback cuando uno o varios usuarios han sido echados de un grupo
