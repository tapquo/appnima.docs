2 STETPS OAUTH
====

Este módulo recoge toda la funcionalidad para obtener el token para un usuario a través del sistema denominado Oauth de 2 pasos, la ruta del recurso es:

    http://appnima-dashboard.eu01.aws.af.cm/{RECURSO}

A continuación se explica el proceso de obtención de token a través de este sistema.

## Paso 1: GET /oauth/authorize
--------------------------------
Se redirigirá al usuario a una url previamente mencionada y a este recurso, pasando como parámetros:

`response_type`: El tipo de solicitud, deberá ser "CODE"
`client_id`: El identificador público de tu aplicación
`scope`: Los permisos que tendrá el token
`redirect_uri`: La url de redirección. Será la misma que diste de alta en tu aplicación.
`state`: Variable de estado que acompañará a la respuesta para que se pueda identificar la operación.

La url sería algo similar a esto:

`http://appnima-dashboard.eu01.aws.af.cm/oauth2/authorize?scope=profile,push&response_type=code&client_id=519b84f0c1881dc1b3000002&redirect_uri=http://myapp.com&state=user48`

El usuario verá una página en la que se le presentan los datos de la aplicación y los permisos que requiere, si lo rechaza o algo va mal, se redirigirá al usuario a la página con un campo `error` que especifica el error.

Si ha ido todo bien se redirigirá a la página con un campo `code` que especifica un código con el que continuar el proceso.

## Paso 2: POST /oauth2/token
-----------------------------
Una vez tengamos el `code` se solicitará el token. Para ello se llamará al recurso /oauth2/token a través de una petición POST con la cabecera basic con el id y el secret de la applicación codificados en base64 y los siguientes parámetros:

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