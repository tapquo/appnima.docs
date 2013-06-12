2 STETPS OAUTH
====
With this module App/nima get user token through the OAuth system called 2-step. The resource path is:

    http://appnima-dashboard.eu01.aws.af.cm/{RECURSO}
    
The following explains how to get token using this method.

## Step 1: GET /oauth/authorize
--------------------------------
User will be redirect to another URL. This resource is called with the following parameters:

`response_type`: type of request. Should be `code`.
`client_id`: your public ID application.
`scope`: token permissions
`redirect_uri`: URL to redirect. It will be the same as you provide when you registered your application.
`state`: state parameter with response to identify the operation.

The URL will be like this:

`http://appnima-dashboard.eu01.aws.af.cm/oauth2/authorize?scope=profile,push&response_type=code&client_id=519b84f0c1881dc1b3000002&redirect_uri=http://myapp.com&state=user48`

The user will see a site where will be shown data application and permissions required. If rejected or something it is wrong with query, user will be redirect to site specifying the error.

If the request was successful, user will be redirect to a site with a field `code` that allows to follow the process.


## Step 2: POST /oauth2/token
-----------------------------
After get `code`, token will be request. To do so, this resource will be called with header "http basic authorization" with application ID and secret Base64 encoded and the next parameters:

```json
    {
        grant_type: "code",
        code:       "h02h40g208ghop2envg2h9h2epne2eh092he0b2",
        client_id:  "532023h2308h2839h"  
    }
```

If the query was successful returns `201 Created` and the object:

```json
    {
        token_type:      "bearer",
        refresh_token:   "n72c03ty202ugx2gu2u",
        access_token:    "eh024hg02g2onvev29"
    }
```