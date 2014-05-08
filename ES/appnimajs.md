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


#### Avatar
Tus usuarios pueden subir su propio fichero de avatar desde su equipo. Para subir una imagen utiliza este recurso pasando el fichero codificado en Base64:

    Appnima.User.avatar(USER_AVATAR);

Password
--------
APP/NIMA ofrece a sus usuarios dos formas de tratar contraseñas, recordarla o cambiarla.

#### Recordar contraseña
Esta acción se realiza mediante dos funciones. Primero habría que llamar al siguiente método:

    Appnima.User.rememberPassword("jdkdksj421432k", "http://application_domain", "reset_password");

El primer parámetro se trata del ```token```del usuario de APP/NIMA, esto es, el ```ACCESS_TOKEN``` del usuario. El segundo parámetro se trata del dominio de la aplicación que llama a dicha funcionalidad y el último la url a la que se quiere llamar.

Esta función envia un mail al usuario propietario del token de parte de APP/NIMA con una URL de la siguiente forma:

    DOMINIO/URL/CODE -> http://application_domain/reset_password/25kj4fkwnfmndjkhgjk4h5nmf

El código lo genera APP/NIMA y sirve para identificar la petición de qué usuario ha pedido recordar la contraseña. Para esto, como se puede observar, es necesario generar un endpoint en el backend de la aplicación con dicha URL en la que haya un formulario donde rellenar la nueva contraseña deseada. Por lo tanto habría que llamar al siguiente método:

    Appnima.User.resetPassword("25kj4fkwnfmndjkhgjk4h5nmf", "12345");

El primer parámetro es el código de la URL generada en el paso anterior y el segundo parámetro es la nueva contraseña que el usuario quiere regenerar. Una vez hecho esto, APP/NIMA envía un email al usuario avisándole de que su contraseña ha sido modificada.

En el caso de no tener backend en la aplicación, APP/NIMA le ofrece una vista en la que regenerar la contraseña al usuario. Para ello se debería llamar al método:

    Appnima.User.rememberPassword("fdfadsfdsf");

Pasándole únicamente el token del usuario del que se quiere regenerar el password y APP/NIMA le enviará un mail con la URL propia donde regenerar el password.

#### Cambiar contraseña
Con esta función se puede cambiar la contraseña directamente. El único requisito es que el usuario esté logueado con APP/NIMA, y después habría que llamar al siguiente método:

    Appnima.User.changePassword("12345", "67890");

El primer parámetro se trata de la vieja contraseña y el segundo de la nueva por la que se desea cambiar.


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
Utiliza este recurso como sistema de gestión de tickets para la resolución de las consultas e incidencias de tus usuarios. La petición necesita un objeto como el siguiente:
```json
    parameters = {
        title       : "[QUESTION]: How can I do this?",
        description : "Lorem ipsum dolor sit amet, consectetur adipisicing elit",               reference   : "1356f43524fa4",
        type        : "2"
    }
```
El campo reference se utiliza por si se quiere añadir la ID de cualquier otro modelo, ya sea de APPNIMA o de otra base de datos.
El campo type puede ser 0, 1 o 2. Si no se manda este campo, por defecto es 0.

0 -> "question"
1 -> "bug"
3 -> "support"

    Appnima.User.ticket(parameters);

Por otro lado, si se quiere modificar un ticket hay dos opciones:

La primera sería poder modificar los datos del ticket. Esto es solo posible cuando el ticket aún no está respondido. Para ello simplemente hay que enviar los mismos datos que a la hora de crearlo, añadiendo la ID del ticket que se quiere modificar.

La otra opción es contestar a un ticket. Para ello habría que mandar el siguiente objeto:
```json
    parameters = {
        response : "Lorem ipsum"
    }
```
Una vez respondido al ticket, se envía un email al creador de dicho ticket.
Para ambos casos habría que enviar los datos a la siguiente llamada:

    Appnima.User.updateTicket(parameters);

Si se desea obtener un ticket en concreto, habría que mandar la ID del ticket a la siguiente llamada de AppnimaJS:

    ```json
        parameters = { id: 325425324563654654}
    ```

    Appnima.User.getTicket(parameters);

También existe la opción de buscar un conjunto de tickets. Para ello habría que enviar los siguientes parámetros:

    ```json
        parameters = {
            reference : 325425324563654654,
            type      : 0,
            solved    : true,
            user      : 43242465344789
        }
    ```

El ejemplo anterior sería la opción de enviar todos los parámetros posibles. El atributo `solved` puede ser `true` o `false`o puede no enviarse. Si se envía a `true`se está diciendo que se desean los tickets que ya están contestados. Si se envía a `false` sería los que están pendientes de responder, y si no se envía ese atributo es porque se desea obtener cualquier ticket, ya sea respondido o no. Cualquiera de los atributos del objeto podría no enviarse.

La llamada que hay que hacer para buscar los tickets sería la siguiente:

    Appnima.User.searchTickets(parameters);

Se puede utilizar paginación añadiendo dos atributos al objeto anterior:

    ```json
        parameters = {
            reference   : 325425324563654654,
            type        : 0,
            solved      : true,
            user        : 43242465344789,
            num_results : 10,
            page        : 2
        }
    ```

La forma de paginación es igual que la paginación de los post pero no hay que enviar el atributo "last_data".


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

#### Conversación
Para obtener los mensages intercambiados con otro usuario, tanto los recibidos como los enviados, se utiliza este método, que recibe como parámetro el username del otro usuario.

    Appnima.Messenger.conversation(username);

#### Marcar como leído
Para marcar un mensaje como leído basta con llamar al recurso pasando el id del mensaje como parámetro:

    Appnima.Messenger.readMessage("28319319832");

#### Resumen
Este método te permite obtener una lista que contenga el último mensaje que hayas mandado o recibido por cada usuario.

    Appnima.Messenger.summary();

#### Buscar
Es posible realizar una búsqueda entre tus mensajes enviados y recibidos a través de una palabra, tanto que la contengan en el asunto del mensaje como en el cuerpo. Para ello se enviará como atributo la palabra con la cual quieres realizar la búsqueda.

    Appnima.Messenger.search("node");


#### Eliminar
Elimina un mensaje llamando al recurso pasando el id del mensaje como parámetro:

    Appnima.Messenger.deleteMessage("28319319832");

Relaciones
==========
#### Buscar
Los usuarios de tu aplicación pueden buscar a otros usuarios mediante este recurso. Puedes enviar como parámetro el mail o parte del mail de un usuario o su nickname o parte de él.:

    Appnima.Network.search("javi");


En el caso de que la respuesta haya sido satisfactoria se devolverá un `200 Ok` junto con una lista de usuarios que coinciden con la búsqueda:
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

#### Seguir
Para seguir a un usuario hay que llamar a este recurso junto con la ID del usuario al que se quiere seguir:

    Appnima.Network.follow("23094392049024112b431d");

El servicio devuelve un `200 Ok` y el objeto `message: "Successful"`


#### Dejar de seguir
Tan sencillo como el recurso anterior, para dejar de seguir a un usuario basta con pasar el ID del usuario:

    Appnima.Network.unfollow("23094392049024112b431d");

El servicio devuelve un `200 Ok` y el objeto `message: "Successful"`


#### Siguiendo
Con este recurso puedes obtener la lista de persona a las que tu usuario sigue o puedes obtener la lista de otro usuario. Al llamar al recurso de la forma:

    Appnima.Network.following():

Obtienes la lista de tu usuario loqueado. Si llamas al recurso pasando como parámetro un objeto con la ID de algún usuario de tu aplicación, obtendrás su lista:

    Appnima.Network.following({user: "23094392049024112b431d"});

Si todo ha salido bien el servicio devolverá un `200 Ok` junto con el objeto:
```json
    count: 2
    [{
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
    }]
```

Por otro lado, también existe la opción de que te devuelva la lista de gente a la que sigues con paginación; esto es, que en cada llamada a la API te vaya devolviendo parte de la lista de usuarios. Para ello unicamente se debe enviar dos variables más junto con la id del usuario del que quieres obtener los datos:

    parameters =
        user: "23094392049024112b431d"
        page: 0
        num_results: 4

    Appnima.Network.following(parameteres);

El primer valor se trata del número de página que deseas obtener; esto es, el trozo de la lista de usuarios que deseas. La segunda variable es el numero de resultados que quieres obtener. En la primera llamada, esa variable será multiplicada por 2, y en los demás casos, se devulverá dicha cifra de usuarios.

#### Seguidores
De la misma forma que lo anterior, puedes utilizar este recurso de dos maneras: si no pasas parámetro obtienes la lista de seguidores del usuario loqueado:

    Appnima.Network.followers();

Si pasas un objeto con la ID de un usuario de tu plataforma obtienes su lista de seguidores:

    Appnima.Network.followers({user: "23094392049024112b431d"});

Al igual que en lo explicado anteriormente, también existe la posibilidad de obtener los resultados con paginación. El modo de uso es igual que en la obtención de los usuarios a los que sigues.

Hay que mencionar, que en este caso, cada objeto del array devolverá un campo más indicando si el usuario logueado sigue a esa persona o no.

#### Amigos
También se pueden obtener los amigos del usuario de la sesión, es decir, aquellas personas que son seguidores tuyos y a la vez ellas te siguen a ti.

    Appnima.Network.friends();

Si todo ha salido bien el servicio devolverá un `200 Ok` junto con el objeto:
```json
    [{
        avatar  : "http://jany.jpg"
        id      : "52f34ac66a7e665b222b6617"
        mail    : "jany@jany.com"
        name    : "jany"
        username: "janixy91"
    }]
```

#### Estado
Con este recurso puedes obtener información respecto a la relación entre dos usuarios, saber si alguno está en la lista de seguidores o de personas a las que sigue. Para ello llama al recurso de la siguiente forma:

    Appnima.Network.check("52f34ac66a7e665b222b6617");


En el caso de que todo haya ido correctamente devolverá un `200 Ok` junto con el objeto que representa la relación que tiene el usuario consultado con el usuario de tu aplicación:

```json
    {
        following:  true,
        follower:   false
    }
```


Posts
--------
#### Crear
Los usuarios pueden crear mensajes (post) dentro de una aplicación. Para ello tienen que mandar el siguiente objeto con los parámetros junto con la petición:

    parameters =
        title   : "Lorem Ipsum"
        content : "Lorem ipsum dolor sit amet, consectetur adipisicing elit."
        image   : "http://IMAGE_URL"

    Appnima.Network.Post.create(parameters);

Solo el campo ```content``` es obligatorio. Un ejemplo de como crear un ```post```unicamente con el contenido es el siguiente:

    Appnima.Network.Post.create({content: "Lorem Ipsum"});

Esta llamada te devolverá un objeto con los datos del nuevo post como el siguiente:

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

#### Modificar
El usuario puede moficiar los post que haya creado. Para ello tiene que enviar los siguientes parámetros:

    parameters =
        id      : "124523132",
        content : "Lorem Ipsum update content",
        title   : "Lorem Ipsum update title",
        image   : "http://IMAGE_URL"

    Appnima.Network.Post.update(parameters);

No es obligatorio mandar los datos que no se vayan a modificar. Dos ejemplos de esto sería:

    parameters =
        id      : "123421434",
        content : "Lorem Ipsum",
        image   : "HTTP://IMAGE_URL"

    Appnima.Network.Post.update(parameters);

    parameters =
        id      : "34324234321423",
        content : "Lorem ipsum",
        title   : "Lorem title"

    Appnima.Network.Post.update(parameters);

Como se puede observar, en el primer caso, se quiere modificar el contenido y la imagen pero el título no, por lo que ese campo hay que mandarlo a nulo.

En el segundo caso, se quiere modificar el contenido y el título, y como la imagen es el último parámetro no hace falta pasarlo a nulo. Lo mismo pasaría en el siguiente caso:

    parameters =
        id      : "34234324",
        content : "Lorem ipsum"

    Appnima.Network.Post.update(parameters);

En este caso solo se quiere modificar el contenido y por lo tanto, las demás variables que van detrás no haría falta mandarlas en nulo.

Este método devolverá ```message: "Successful"``` si todo ha ido bien:

#### Borrar
El usuario que ha creado un post puede borrarlo. Para ello solamente tiene que enviar la id del post que desea borrar:

    Appnima.Network.Post.remove("432423423");

Como en el método anterior, si todo ha ido bien devolverá un mensaje de ```message: "Successful"```

#### Obtener un post
El usuario puede obtener la información de un post concreto enviando la ```id``` de dicho post.

    Appnima.Network.Post.get("423432432");

Esta llamada devolverá el siguiente objeto:

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

La lista de likes se trata de los usuarios que han hecho ```like``` a ese post.

#### Búsqueda
El usuario puede buscar un post en concreto mediante el texto de su contenido, o si le manda simplemente una palabra, le devolverá todos los post que contengan esa palabra en su contenido.

    parameters =
        query       : "lorem",
        page        : 1,
        num_results : 12,
        last_data   : last_data

    Appnima.Network.Post.search(parameteres);

Como se puede observar, en este caso se desean obtener los resultados mediante paginación, como se ha explicado anteriormente. Pero si no se desease usarla, simplemente habría que hacer la llamada de la siguiente manera:

    Appnima.Network.Post.search({query: "Lorem ipsum"});

En este caso, ambas formas devolverán un array con los post que ha encontrado, o un array vacío si no coincide ninguno.

#### Contador
Este método se usa para obtener el contador de los post que el usuario tiene. La respuesta sería ```posts_count: 8```. En este ejemplo, el usuario de la sesión habría creado 8 post.

    Appnima.Network.Post.counter();
    Appnima.Network.Post.counter("424234234324");

Si al método se le envía una id de un usuario, devolverá el contador de dicho usuario.

#### Timeline
Este método sirve para obtener un listado de post. Habría dos formas diferentes de obtenerlos con dos resultados diferentes. Un caso sería el siguiente:

    Appnima.Network.Post.timeline();

    parameters =
        page        : 1,
        num_results : 12,
        last_data   : last_data

    Appnima.Network.Post.timeline(parameters);

En este caso, en la primera llamada se obtendría el ```timeline```del usuario de la sesión. Esto es, una lista de post creados por el y por los usuarios a los que sigue. (Un ejemplo fácil de entender sería el timeline de ```Twitter```).

En la seguna llamada es lo mismo, solo que se desea obtener los posts mediante paginación.
El otro caso sería el siguiente:

    Appnima.Network.Post.timeline({username: "username"});

    parameters =
        username    : "username",
        page        : 1,
        num_results : 12,
        last_data   : last_data

    Appnima.Network.Post.timeline(parameters);

En este caso se está obteniendo el timeline de un usuario en concreto, esto es, los post que él ha escrito. (Siguiente con el ejemplo de Twitter, este caso sería cuando entras en el perfil de un usuario en concreto). Como se puede observar, también se puede hacer mediante paginación.

#### Crear comentarios
Un post puede tener comentarios, y con esta llamada se puede realizar.

    parameters =
        id      : "424231423423"
        content : Este es mi comentario

    Appnima.Network.Post.createComment(parameters);

En este caso es obligatorio mandarle ambos campos y la llamada devolverá ```message: "Successful"```.

#### Borrar comentario
El usuario que ha creado un comentario también tiene la posibilidad de borrarlo. Para ello deberá llamar a la siguiente función:

    Appnima.Network.Post.deleteComment("4234324234234");

Hay que pasarle la id del comentario que se desea borrar y la API devolverá ```message: "Successful"``` si todo ha ido bien.


#### Obtener comentario
Para obtener los comentarios de un post en concreto el usuario debe usar el siguiente recurso pasándole la id del post:

    Appnima.Network.Post.comments("424231423423");

Esta llamada devolverá el siguiente array de objetos:

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
                },
                {
                    id: 43265453745,
                    content: "Comment 2",
                    created_at: comment_created_data,
                    owner: {
                        avatar: http://AVATAR_URL,
                        id: 3425425425,
                        name: user,
                        username: username
                    }
                }
            ]

#### Hacer favorito un post
El usuario puede hacer favorito un post, o quitar dicho "like". Para ello el usuario debe enviar la id del post junto con la llamada:

    Appnima.Network.Post.like("424231423423");

Si es la primera vez que hace favorito un post, este se creará, pero por el contrario, si este post ya tiene favorito, ese favorito desaparecerá y la próxima vez que se llame a este método volverá a ser para hacer favorito.
Si todo ha ido bien devolverá ```message: "Successful"```.

#### Obtener los usuarios que han hecho favorito un post
Si el usuario desea obtener todos los usuarios que han hecho favorito a un post en concreto, simplemente debe enviar la id de dicho post junto con la siguiente llamada:

    Appnima.Network.Post.likeUsers("424231423423");

Este método devolverá un array de objetos con todos los usuarios.

    users   : [
                {
                    id: 54364758,
                    name: user1,
                    username: usernam1,
                    avatar: http://AVATAR_URL,
                    bio: biouser1
                }
            ]

#### Obtener todos los post favoritos de un usuario
Si se desea obtener los post favoritos de un usuario, hay que llamar al siguiente recurso pasándole el username del usuario de quien se desean los datos:

    Appnima.Network.Post.userLike({username:"username"});

La API devolverá una lista de post que son favoritos para el usuario con dicho username.
También se pueden obtener mediante paginación. La llamada seria:

    parameteres =
        username    : "username",
        page        : 1,
        num_results : 12,
        last_data   : last_data

    Appnima.Network.Post.userLike(parameters);

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
Si lo necesitas APP/NIMA puede ofrecer una lista de amigos que se encuentran cerca, para ello es necesaria la latitud, longitud y el radio de busqueda:

    Appnima.Location.friends("43.6525842", "-79.3834173, 100");


#### Desconocidos
APP/NIMA también te permite obtener un listado de personas cercanas al usuario que consulta. La petición es similar a la anterior:

    Appnima.Location.people("43.6525842", "-79.3834173, 100");


Calendario
===============

#### Crear
APP/NIMA permite a los usuarios tener un sistema de calendarios donde gestionar sus eventos.

Para crear un nuevo calendario, se utiliza el siguiente comando que envía como parámetro el nombre y el color del nuevo calendario.

    Appnima.Calendar.create("mi calendario", "#FF66CC")

Esta función nos devuelve el nuevo calendario:

    calendar : {
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

#### Modificar
Tambien tenemos la opción de modificar los atributos de un calendario ya creado, tanto el nombre, como el color. Para ello, se utiliza la siguiente función que envía como parámetro la "id" del calendario a modificar, el nuevo nombre y el nuevo color.

    Appnima.Calendar.update("28319319833", "mi nuevo calendario", "#FF66CC")

En caso de que el calendario no exista, devuelve un error 404. Si por el contrario existe, devuelve el calendario con los campos modificados:

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

#### Compartir
Cabe la posibilidad de compartir un calendario con otros usuarios, para que así, ellos también puedan ver los eventos que hay en dicho calendario. Para ello, sólo hay que realizar la llamada a la función que se muestra a continuación, enviando como parámetro la "id" del calendario, la "id" del usuario a invitar, y "add".

    Appnima.Calendar.shared("28319319833", "28319364941", "add")

O por el contrario, también se puede eliminar a un usuario de la lista de usuarios compartidos, para que ese usuario deje de ver dichos eventos. Para ello, la llamada a la función es la misma que la de compartir, sólo que como tercer paramentro se envía "remove"

    Appnima.Calendar.shared("28319319833", "28319364941", "remove")


En caso de que el calendario no exista, devuelve un error 404. En caso de que haya ido bién devolverá el calendario actualizado. El atributo "shared" corresponde con la lista de usuarios a los que se les ha compartido el calendario.

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

#### Borrar
Tambien se nos permite eliminar un calendario, eliminando al mismo tiempo, todos sus eventos. Para ello, se utiliza la siguiente función, enviando como parámetro la "id" del calendario que se desea borrar

    Appnima.Calendar.remove("28319319833")

En caso de que el calendario no exista, devuelve un error 404. En caso de haya vaya bien, devuelve un mensaje indicando que todo ha ido satisfactoriamente.

    message: Successful

#### Actividad
APP/NIMA también nos ofrece información de qué ha sucedido en un calendario. Si usamos la función que se muestra a continuación enviando como parámetro la "id" de un calendario, nos ofrece una lista de actividades que han sucedido en él, como son, modificar el calendario, crear, modificar o borrar un evento perteneciente al calendario, compartir el calendario o borrar a alguien de la lista de usuarios compartidos, invitar a alguien o quitarle de la lista de invitados de un evento de dicho calendario ó la asistencia o desasistencia de un usuario a un evento.

    Appnima.Calendar.activityCalendar("28319319833")

En caso de que el calendario no exista, devuelve un error 404. En caso de que haya ido bien, nos devuelve un listado de actividades con la estructura que se muestra a continuación

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
                },
                {
                  id: 52f8ef8282652a0000000007,
                  message: 'u1net has update the calendar',
                  created_at: Mon Feb 10 2014 16:25:54 GMT+0100 (CET),
                  profile: {
                             username: 'u1net',
                             name: 'name',
                             mail: 'a1@appnima.com',
                             avatar: 'http://appnima.com/img/avatar.jpg',
                             id: 52eb667ab71cd7e4be00000c
                            },
                  event: undefined,
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


El evento y el calendario, es dónde se ha realizado la actividad. En caso de que el evento es null, es por que la actividad únicamente afecta al calendario. El campo "owner" es la persona que realiza la actividad y el campo "profile", es la persona a la que va dirigida la actividad.

#### Crear un evento
A través de la siguiente función se puede crear un evento para un calendario. Se le debe envíar como parametros la "id" del calendario al que se desea que pertenezca el nuevo evento, el nombre del evento, la descripción, la fecha inicial y final en formarto mm-dd-yyyy hh:mm, una string con una lista de "id" de usuarios separados por "," que corresponde con los usuarios con los que quieres compartir dicho evento, una string con una lista de tags separados por "," para poder taguear el evento, la dirección de donde se va a realizar el evento, la localidad, el país, la latitud y la longitud

    Appnima.Calendar.event(52f0d497f4a9b16f47000002, "partido de futbol", "quedada para jugar un partido de fútbol", "04-14-2014 09:00", "04-14-2014 11:00", null, "futbol,deporte", "c/ San Mames", "Bilbao", "España", "23.23", "-2.29")

Esta función devuelve el nuevo evento:

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

#### Modificar un evento
También se nos permite modificar un evento. Se le debe envíar como parametros la "id" del evento que se desea modificar, el nombre del evento, la descripción, la fecha inicial y final en formarto mm-dd-yyyy hh:mm, una string con una lista de "id" de usuarios separados por "," que corresponde con los usuarios con los que quieres compartir dicho evento, una string con una lista de tags separados por ",",la dirección de donde se va a realizar el evento, la localidad, el país, la latitud y la longitud.

    Appnima.Calendar.event(52f0e1e6d028ec6b6f000011, "partido de balonceso", "quedada para jugar un partido de baloncesto", "04-14-2014 09:00", "04-14-2014 11:00", null, "futbol,deporte", "c/ San Mames", "Bilbao", "España", "23.23", "-2.29")

En caso de que el evento no exista, devuelve un error 404. Si por el contrario existe, devuelve el evento con los campos modificados con la estructura del objeto que se devuelve en la función de crear evento.

#### Listar eventos
A través de la siguiente función se pueden obtener los eventos de los calendarios en los que el usuario logueado es dueño, los eventos de los calendarios que le han compartido, y los eventos a los que se le han invitado. Se deben filtrar los eventos por tiempo. Si se quieren listar los eventos de un mes, como primer parámetro se le envía "month", como segundo parámetro se envía el año, y como segundo el número del mes del que se quieren listar los eventos.

    Appnima.Calendar.listEvents("month", "2014", "02")

Si lo que se quiere es listar los eventos de una semana, se le envía como primer parámetro "week", como segundo parámetro el año de la semana, como tercer parámetro el número del mes de la semana, y como cuarto parámetro el día. Esta fecha corresponde con un día de la semana de la que se quiere obtener los eventos

    Appnima.Calendar.listEvents("week", "2014", "02", "14")

Si lo que se quiere es listar los eventos de un día, se le envía como primer parámetro "day", como segundo parámetro el año del día, como tercer parámetro el número del mes, y como cuarto parámetro el día.

    Appnima.Calendar.listEvents("day", "2014", "02", "14")

Como resultado se obtiene una lista de eventos:

    events : [
                {
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

                },
                {
                    id: 52f0ed7893888c029200000d,
                    calendar: 52f0ed7893888c0292000002,
                    date_init: Mon Apr 14 2014 09:00:00 GMT+0200 (CEST),
                    date_finish: Mon Apr 14 2014 11:00:00 GMT+0200 (CEST),
                    name: 'bilboStack',
                    description: 'This event is bilboStack',
                    place: 52f0ed7893888c029200000c,
                    assistents: [ ],
                    created_at: Tue Feb 04 2014 14:39:04 GMT+0100 (CET),
                    tags: [ learn ],
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

#### Invitar a un evento.
Otra funcionalidad que es posible, es la de invitar a un usuario a un evento, para que así, él también pueda ver dicho evento. O por el contrario, eliminar una invitación para que ese usuario deje de ver dicho evento. Para ello, sólo hay que ejecutar la siguiente función que se muestra a continuación, enviando como parámetros, la "id" del evento, la "id" del usuario a invitar, y "add" o "remove", si se quiere añadir invitación, se envía "add" si por el contrario se quiere eliminar, se envía "remove".

    Appnima.Calendar.guestEvent("52f0f4f313255536a8000005", "52eb667ab71cd7e4be000004", "add")


En caso de que el evento no exista, devuelve un error 404. En caso de que haya ido bién devuelve el evento actualizado. El atributo "guest" corresponde con la lista de usuarios a los que se les ha invitado al evento.

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
#### Asistir a un evento.
También los eventos permiten saber qué usuarios van a asistir. Para confirmar la asistencia a un evento o para eliminarla se utiliza dicha función, en la cual, se envía como parámetro la "id" del evento, la "id" del usuario, y "add" o "remove". Si se quiere añadir invitación, se envía "add" si por el contrario se quiere eliminar, se envía "remove".

    Appnima.Calendar.assistentEvent("52f0f84333e9d53db2000005", "52eb667ab71cd7e4be000004", "add")

En caso de que el evento no exista, devuelve un error 404. En caso de que haya ido bién devolverá el evento actualizado. El atributo "assistents" corresponde con la lista de usuarios que van a asistir al evento.

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
#### Búsqueda de eventos.
APP/NIMA te permite buscar eventos. La función envía como parámetro una palabra, y se busca una coincidencia con dicha palabra en el nombre y en la descripción de los eventos que tienes acceso. Es decir, aquellos que estén en un calendario donde seas el dueño o te los hayan compartido y aquellos eventos a los que te hayan invitado.

    Appnima.Calendar.search("meeting")

La función devuelve una lista de eventos que cumplan dichas coincidencias:

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
                },
                {
                    id: 52f0fa9eb70ed01fb9000016,
                    calendar: 52f0fa9eb70ed01fb9000013,
                    date_init: Sat Feb 15 2014 16:00:00 GMT+0100 (CET),
                    date_finish: Sat Feb 15 2014 17:00:00 GMT+0100 (CET),
                    name: 'meeting osakidetza updated',
                    description: 'meeting to discuss changes in the implementation',
                    place: 52f0fa9eb70ed01fb9000015,
                    assistents: [ 52eb667ab71cd7e4be000004 ],
                    created_at: Tue Feb 04 2014 15:35:10 GMT+0100 (CET),
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
            ]

#### Borrar un evento
Cabe la posibilidad de eliminar un evento, para ello basta con ejecutar la siguiente función, enviando como parámeto la "id", del evento que se desee borrar

    Appnima.Calendar.deleteEvent(52f0e1e6d028ec6b6f000011)

En caso de que el calendario no exista, devuelve un error 404. En caso de haya vaya bien, devuelve un mensaje indicando que todo ha ido satisfactoriamente.

    message: Successful

#### Actividad
Al igual que con un calendario, APP/NIMA también nos ofrece información de qué ha sucedido en un evento en concreto. Con la función que se muestra a continuación enviando como parámetro la "id" de un evento, nos ofrece una lista de actividades que han sucedido en él, como son, modificar ese evento, invitar a alguien o quitarle de la lista de invitados o asistencia o desasistencia de un usuario.

    Appnima.Calendar.activityEvent("28319319833")

En caso de que el evento no exista, devuelve un error 404. En caso de que haya ido bien, nos devuelve un listado de actividades con la estructura que se muestra a continuación

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
              },
              {
                id: 52f8f6a9694687000000002c,
                message: 'u1net has update the event',
                created_at: Mon Feb 10 2014 16:56:25 GMT+0100 (CET),
                profile: {
                           username: 'u1net',
                           name: 'name',
                           mail: 'a1@appnima.com',
                           avatar: 'http://appnima.com/img/avatar.jpg',
                           id: 52eb667ab71cd7e4be00000c
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
                }]


El evento y el calendario, es dónde se ha realizado la actividad. El campo "owner" es la persona que realiza la actividad y el campo "profile", es la persona a la que va dirigida la actividad.


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

Eliminar el contador de mensajes no leídos de ese grupo, el parámetro requerido es el id de la room:

    group.deleteUnreadCount("id");


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

    inbox.onOnlineFriends(callback);

Conexión/Desconexión de amigo:

    inbox.onFriendStatusChange(callback);

Enviar datos a un usuario en concreto:

    inbox.sendToUser(user_id, data);


Métodos
-------
Estos métodos son los necesarios para la gestión de los tipos de socket vistos anteriormente:

* `instance.connect("id")`: Permite conectarse a una sala de Group o Chat

* `instance.disconnect()`: Se desconecta de una sala

* `instance.allowUsers(["","",""…])`: Permite añadir usuarios permitidos en Groups y Chats

* `instance.disallowUsers(["","",""…])`: Elimina usuarios de la lista de admitidos

* `instance.send(message)`: Envía un mensaje a todos los usuarios de la sala (emisor incluido).

* `instance.broadcast(message)`: Envía un mensaje a todos los usuarios de la sala excepto al emisor.

* `instance.onConnect(callback)`: Llama al callback cuando se conecta a la sala

* `instance.onError(callback)`: Llama al callback cuando sucede un error

* `instance.onDisconnect(callback)`: Llama al callback cuando el socket se desconecta

* `instance.onMessage(callback)`: Llama al callback cuando se recibe un mensaje, el mensaje será un objeto con los atributos:
    * content: "Contenido del mensaje"
    * user: {"Usuario que lo envía"}
    * created_at: "2013-11-16T05:55:02.736Z"

* `instance.onDisallow(callback)`: Llama al callback cuando uno o varios usuarios han sido echados de un grupo








Payments
========
Para realizar compras y efectuar pagos de manera sencilla Appnima proporciona la funcionalidad payments. Con Appnima payments puedes realizar compras dentro de la aplicación, gestionar información de pago los usuarios y hacer efectivos los cobros.

Importante recordar que debemos tener una sesión válida para poder hacer usor del módulos de pagos.

Compras
-------

### Generar una compra
Para efectuar compras que no necesiten de un intercambio monetario se utiliza el método purchase. Las compras se realizan en una secuencia de 2 pasos, generar una compra y confirmarla cuando se valide su veracidad. Primero se debe crear la compra y validarla posteriormente.

	Appnima.Payments.purchase();
	
Si todo ha salido correctamente Appnima nos generará una compra a nuestro usuario pendiente de confirmación por lo que su estado es 0.

	{
		token: "purchase_secret_token",
		amount: 0
	
	}

### Confirmar una compra

Cuando confirmemos la compra debemos enviar el token secreto que se nos proporcionó cuando generamos la compra. Como la compra carece de importe el importe a cobrar debe ser cero.

	Appnima.Payments.purchase({token: "purchase_secret_token", amount: 0});

Para indicar que el pago ha sido confirmado solo Appnima nos devolverá información de la compra con el estado 3 que significa que está correctamente procesada.

	{
		id: "purchase_ID",
		payed_at: purchase_confirmation_date,
		state: purchase_state "
	}

Tarjetas de crédito
-------------------
El principal medio de pago online hoy en dia son las tarjetas de crédito pro ello con Appnima podemos asociar tarjetas de crédito al perfil de un usuario de manera sencilla. 

### Crear tarjeta
Para crear una tarjeta de crédito tan solo tenemos que realizar la siguiente petición pasandole los datos de una tarjeta en cuestión. El cvc es opcional.

	Appnima.Payments.createCreditCard({number: "4242424242424242", cvc: 123, expiration_date: "11/2015"})

Si el proceso se ha realizado con éxito Appnima te devolverá el siguiente mensaje:

	{
		id: "credit_card_ID",
		number: "xxxxxxxxxxxx4242"
	}
	
### Tarjetas de un usuario
Para consultar todas las tarjetas de las que dispone un usuario tan solo hay que llamar a getCreditCards sin ningún argumento.

	Appnima.Payments.getCreditCards()
	
Lo que nos devolverá un array con la información ofuscada de las tarjetas de crédito del usuario.

### Borrar tarjeta
Para borrar una tarjeta de crédito tan solo tenemos que realizar la siguiente petición pasandole el id de la tarjeta que queremos borrar.

	Appnima.Payments.deleteCreditCard({id: "credit_card_ID"})

Si el proceso se ha realizado con éxito Appnima te devolverá un mensaje con código 200.

### Modificar una tarjeta
Para modificar la los datos de una tarjeta de crédito tan solo tenemos que pasarle el id de la tarjeta destino que queremos modificar con los campos que deseamos cambiar.

	Appnima.Payments.updateCreditCard({id: "credit_card_ID", number: "4343434343434343"})

Appnima nos devolverá un mensaje de confirmación para notificarnos que el cambio se ha realizado con éxito.

Payment Providers
-----------------
Cuando necesitemos realizar una compra que requiera un cobro sobre una tarjeta de crédito Appnima dispone de pasarelas de pago adaptadas a las necesidades de los usuarios.

### Stripe
Stripe es una de las principales pasarelas de pago hoy en día para realizar pagos mediante stripe tan solo tenemos que realizar una compra pero indicandole el medio de pago y el importe. El sistema es similar al de las compras que no requerían intercambio monetario. Generas la compra y la confirmas para realizar el cobro.

#### Compra con Stripe
Para realizar una compra con stripe tan solo tenemos que indicarle el id de la tarjeta sobre al que queremos realizar el cargo, el código de seguridad de esta y la cantidad.

      Appnima.Payments.stripePurchase({credit_card : "credit_card_ID",cvc: 123, amount      : 10000})

Esto nos devolverá una confirmación con un token secreto de un solo uso y la cantidad:

	 {
	 	token: "purchase_secret_token",
	 	amount: 10000 
	 }


#### Confimación con Stripe
Para hacer efectivo el cargo sobre la tarjeta es necesario confirmar para que la transacción se haga efectiva. Para ello debemos enviar el mensaje de confirmación con el token de seguridad y la cantidad para verificar la validez de la transacción.

      Appnima.Payments.stripeConfirm({token: "purchase_secret_token", amount: 10000})

Si la confirmación es correcta recibiremos el siguiente mensaje, con el estado de la compra a 3 que nos indica que el pago se ha realizado con éxito.

	{
		id: "purchase_ID",
		payed_at: purchase_confirmation_date, 
		state: purchase_state 
	}










