App/nima - *LaaS Open Platform*
===============================


## 1. Introducción
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


### 1.1 Arquitectura
--------------------
Seguramente en este momento tendrás dudas de como App/nima puede ayudarte a ser más eficiente pero tranquilo vamos a explicarte cómo funciona. App/nima esta basada por completo en el protocolo de autentificación [**Oauth 2**](http://oauth.net/2/) y en la serialización mediante REST-style con objetos JSON. Si nunca has utilizado este paradigma no te preocupes te explicarémos paso a paso como conectarte a la plataforma y te ofreceremos herramientas específicas como [**AppnimaJS**](http://) que te facilitarán el proceso de conexión.

Todo el backend está montado a lo largo del mundo utilizando plataformas como Amazon o Google Cloud Engine las cuales nos dan la capacidad de ofrecer siempre la mejor calidad de respuesta para nuestros usuarios. No te preocupes no tendrás que ser un *Jedy SysAdmin* ya que tu unicamente te tienes que comunicar con nuestro REST la tarea de escalabilidad corre de nuestra cuenta, tu preocupate en crear el mejor proyecto y nosotros de ofrecerte cada día una mejor plataforma.

Resumiendo App/nima es una plataforma en forma de API REST que te provee de los servicios logicos de tu proyecto, y dependiendo de la naturaleza de tu proyecto puede que no necesites ni tu propio Backend. Cada servicio lógico esta alojado en servidores diferentes para obtener la mayor escalabilidad independiente, a lo largo de la documentación conoceremos las rutas de cada servicio.


### 1.2 Autentificación
-----------------------
Todos los servicios de App/nima utilizan el protocolo  de autentificación [**OAuth 2**](http://oauth.net/2/) buscando la mayor compatibilidad con herramientas de terceros. Si no eres un experto en OAuth 2 te recomendamos que lo estudies ya que esta siendo a llamar el protocolo de autentificación por excelencia y empresas como Google, Facebook o Twitter lo utilizan para conectarse a sus APIs. Lee la [documentación](http://) para comenzar hoy mismo con App/nima.


### 1.3 	No XML, solo JSON
--------------------------
Sólo permitimos la serialización de datos tipo JSON. Nuestro formato es no tener ningún elemento raíz y utilizar *snake_case* para describir las claves de atributos. Esto significa que en cada petición a App/nima tienes que enviar el sufijo:

	Content-Type: application / json; charset = utf-8

Recibirás una respuesta 415 *Unsupported Media Type* si intentas utilizar otro tipo de sufijo URL.


### 1.4 Usa el cacheo HTTP
--------------------------
Tienes que hacer uso de las cabeceras HTTP *freshness* para disminuir la carga de nuestros servidores (¡y aumentar la velocidad de tu aplicación!). La mayoría de las peticiones que devolvemos incluirán un *ETag* o una cabecera *Last-Modified*. Al solicitar por primera vez un recurso, almacena este valor y envianoslo en las posteriores peticiones como *If-None-Match* y *If-Modified-Since*. Si el recurso no ha cambiado, obtendrás un código 304 *Not Modified* como respuesta, que te ahorra tiempo y ancho de banda (porque ya tienes ese recurso).


### 1.5 Manejando los errores
-----------------------------
Si App/nima tiene algun problema, es posible que veas un error 5xx. 500 significa que la aplicación esta totalmente caída, pero tambien puedes ver un 502 *Bad Gateway* (entrada erronea), 503 *Service Unavailable* (Servicio no disponible), o un 504 *Gateway Timeout* (límite de tiempo de espera). En todos estos casos es tu responsabilidad reintentar la petición más tarde.


### 1.6 API lista para trabajar
-------------------------------
* [User](http://)
* [Network](http://)
* [Messenger](http://)
* [Location](http://)
* [Socket](http://)
* [Push](http://)


### 1.7 API en la que estamos trabajando
----------------------------------------
* [Payments](http://)
* [X](http://)


### 1.8 Ayudanos a ser cada vez mejores
---------------------------------------
Por favor no dudes en ponerte en contacto con nosotros si crees que puedes hacer una mejor API. Si crees que deberíamos soportar una nueva funcionalidad o si has encontrado un bug, usa GitHub issues. Haz un fork de esta documentación y mandanos tus *pulls* con las mejoras.

Para hablar con nosotros o con otros desarrolladores sobre la API, suscribete a nuestra [**lista de correo**](https://groups.google.com/forum/#!forum/appnima).
