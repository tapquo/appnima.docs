App/nima - *LaaS Open Platform*
===============================


## 1. Introduction
Just for reading this document you are telling that you are a developer who wants to improve his skills continously and wants to create more efficient projects. App/nima is the first platform which offers logic services for any type of project, no matter you want to create an application or a site, App/nima will help you in both situations.

A bit of history, 3 years ago in [**Tapquo**](http://tapquo.com) we found a common problem in the development area, it was sthat each new product we create we had to repeat every time the same basic functionalities. App/nima borns from the need of being more efficients and the desire to develop only the business features of our new product and no more the horizontal logic.

+ OAuth 2 services for authentication
+ User management
+ User social network
+ Messaging service to send emails, SMS, calls and private messages
+ Find/geolocate places
+ Find/geolocate other users
+ Real-time communications via sockets
+ Push notifications for ioS, Android and Blackberry
+ Get our users' behaviour

If sometime you have had to develop some of this functionalities in any project, you are welcome to our platform because App/nima is thougth to help you as a developer. We want to give you a platform made to ease your life, we love software as much as you and we seek being better. Use App/nima.


### 1.1 Arquitecture
Probably now you have doubts of how App/nima can help you in being more efficient, but calm down we are going to explain you how it works. App/nima is completely based in the authentication protocol [**Oauth 2**](http://oauth.net/2/) and in the serialization via REST-style with JSON objects. If you have never used this paradigm don't worry, we are explaining step by step how to connect to the platform and will offer you explicit tools such as [**AppnimaJS**](http://) that will ease you the connection process.

All the backend is deployed around the world using platforms such as Amazon or Google Cloud Engine, the ones which give us the ability to give allways the best response quality for our users. Don't worry, you won't have to be a *Jedy SysAdmin* because you will only have to communicate with our REST. The scalability is our duty, you will only have to worry about creating the best project and us in providing every day the best platform.

In summary, App/nima is a platform in a REST API way that provide you with the logical services of your projects, and depending the nature of your project maybe you won't need your own backend. Each logical service is hosted in different servers to obtain the biggest independent scalability, along the documentation we will watch the routes of each service.


### 1.2 Authentication
All App/nima services use the authentication protocol [**OAuth 2**](http://oauth.net/2/) looking for the largest compatibility with third party tools. If you are not an expert in Oauth 2 we recommend you to study it, because it is beeing the authentication protocol by default and companies like Google, Facebook or Twitter use it to connect with their APIs. Read the [documentation](http://) to start right now working with App/nima.


### 1.3 	No XML, just JSON
We only allow JSON type data serialization. Our format is not to have any root element and use *snake_case* to describe atributes' keys. This means that each petition to App/nimamust be send the sufix:

	Content-Type: application / json; charset = utf-8

Yo will receive a response 415 *Unsupported Media Type* if you try to use another URL sufix.


### 1.4 Use HTTP cache
You have to use the *freshness* HTTP headers to reduce our servers' load (and speed up your application!). Most of the petitions we return will include an *ETag* or a header *Last-Modified*. The first time you request a resource, store this value and send it to us in the next petitions as *If-None-Match* and *If-Modified-Since*. If the resource hasn't change you will obtain the code 304 *Not Modified* as response, saving time and broadband (because you already have the resource)


### 1.5 Handling errors
If App/nima has any problem, it is possible that you get an 5xx error. 500 means that the application is totally down, but you can also get a 502 *Bad Gateway*, 503 *Service Unavailable*, or 504 *Gateway Timeout*. In all these situations it is your responsibility to try again later.


### 1.6 API ready to work
* [User](http://)
* [Network](http://)
* [Messenger](http://)
* [Location](http://)
* [Socket](http://)
* [Push](http://)


### 1.7 API we are working on
* [Payments](http://)
* [X](http://)


### 1.8 Help us on being better
Please, don't have any doubt in contacting us if you think you can do a better API. If you think that we have to support a new functionalit or if you have found a bug, use GitHub issues. Make a fork of this documentation and send us your *pulls* with the improvements.

To talk with us or with other developers about the API, suscribe to our [**mailing list**](https://groups.google.com/forum/#!forum/appnima).
