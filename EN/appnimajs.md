App/nima.JS
===========
Find out how to add soul to your apps with first LaaS in the world.

*Current version: [1.0.0]()*

To have appnima.js ready to work with you only have to set the value of the variable `Appnima.key` with the key that APP/NIMA provides you on the application creation:

    Appnima.key = "YOUR_KEY";

App/nima store user's session data in memory, so the session will persist in time.


Promises
--------
Appnima.js uses the promise pattern to handle each request, so to attach a callback (with error and result) to each request we have to do it using `.then` as it is shown in the following example:

    Appnima.resource().then(function(error, result){
        if(error !== null){
            console.log("Error", error);
        }else{
            console.log("Success", result);
        }
    });


Error Handler
-------------
Appnima.js has the capability to centralize the error management in a method. To use this, just assign to Appnima.onError a callback function that will receive the generated error:

    Appnima.onError = function(error){
        console.log("CODE: ", error.code);
        console.log("TYPE: ", error.type);
        console.log("MESSAGE: ", error.message);
    };

Token Management
----------------
App/nima uses a token to access the resources. When a token is expired the server will return a `480` error code, to refresh the token the method `Appnima.User.token()` must be called. It will do all the necessary transactions to update your token so you can continue working.

User
====
Basic
-----
#### Signup
To register a user in your application you only need to call the following method using as parameters the email, password and optionally a username:

    Appnima.User.signup ("javi@tapquo.com", "USER_PASSWORD");


#### Login
Login a user is as easy as calling the resource with the email and password:

    Appnima.User.login("javi@tapquo.com", "USER_PASSWORD");


#### Logout
If you want to logout a user of your application as simple as calling the method *logout* without any parameter:

    Appnima.User.logout();


#### Information
Do you need all the data registered by a user? Just call this resource to get information:

    Appnima.User.info();

In addition, to obtain information of any user of App/nima, you only have to send the id of this user to the previous method:

    Appnima.User.info(1244545632);



The object you will receive in both cases is:

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


#### Update
The `update` resource also allows users to update their data. To do this just call it using as parameter a dictionary that contains all this optional fields:

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



#### Change Password
To change user's pasword use this resource:

    Appnima.User.password(OLD_PASSWORD, NEW_PASSWORD);


#### Avatar
Your users can upload their own avatar file. To upload an avatar use this resource using as parameter the avatar coded in Base64:

    Appnima.User.avatar(USER_AVATAR);

Terminal
--------

#### Register or Update
With this resource you can register or update the device the user is using to access the application. The request is done in the following way:

    Appnima.User.terminal("Android", "Phone", "MobilePhone", "4.1");

#### Info
Get the terminals that the user has used to access to the application using this resource:

    Appnima.User.teminal();

The data you will receive will be like:

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


Subscription
------------
If you need it, you can ask users to register their emails to get an invitation to the app. Just call the resource using the email as parameter:

    Appnima.User.subscribe("javi@tapquo.com");


Tickets
-------
Use this resource as ticket managing system to resolve incidences or attend consults from users. The resource only needs the text:

    Appnima.User.ticket("[SUGGESTION] Bigger buttons");



Messenger
=========

Mail
----
Your application's users can send emails to other users using this resource. To do this, the parameters that are needed are: receiver's ID, the subject and the email body:

    Appnima.Messenger.mail("28319319832". "Meeting", "Tomorrow morning");

SMS
---
APP/NIMA also provides a SMS service. To use this resource the parameters needed are the receivers ID and the message:

    Appnima.Messenger.sms("28319319832". "Remember that your appointment is tomorrow");

Messages
--------
#### Private messages
To use the APP/NIMA's private message service just call the resource using the following parameters: Receiver's ID, message body and optionally the subject:

    Appnima.Messenger.message("28319319832". "Where are you? I'm at home waiting for you.", "Where are you?");


#### Inbox
You can get the unread messages you have received by just calling this resource:

    Appnima.Messenger.messageInbox();

#### Outbox
As we do with received messages we can get the list of of sent messages, just use this resource:

    Appnima.Messenger.messageOutbox();

#### Mark as read
To mark a message as read call this resource using the message id as parameter:

    Appnima.Messenger.readMessage("28319319832");


#### Delete message
To delete a message cal this resource using the message id as parameter:

    Appnima.Messenger.deleteMessage("28319319832");


Posts
--------

#### Post
Users can create posts or modify those that have created in application. To do this, you must send this parameters with the request:

    data = {
        id: POST_ID
        title: "Lorem Ipsum"
        content:  "Lorem ipsum dolor sit amet, consectetur adipisicing elit."
        image: "http://IMAGE_URL"
    }

    Appnima.Messenger.post(data);

Only ```content``` field is required. If ```id``` field is not sent, it will create the post, otherwise, will update this post.

#### Comment
A post can contain a list of comments.

To add a comment into a post you have to pass the id of the post and the comment text:

    Appnima.Messenger.addComment("324685348953", "Lorem impsum dolor sit...")

To get all the comments of a post, you have to pas the id os the post:

    Appnima.Messenger.postComment("53485u452395")

To drop a comment, you have to pass the comment id:

    Appnima.Messenger.deleteComment("837456459643")

Relathionships
==============
#### Follow
You can follow a user using this resource, call this resource using the user to follow's id as parameter:

    Appnima.Network.follow("28319319832");


#### Unfollow
You can unfollow a user using this resource, call this resource using the user to unfollow's id as parameter:

    Appnima.Network.unfollow("28319319832");


#### Following
With this resource you can get the list of people that a user is following. It works as the previous parameter, accepting an optional user id parameter:

    Appnima.Network.following();

    Appnima.Network.following("28319319832");

On the other hand, there is also the option for you to return the list with pagination, that is, that in each API call returning part of the list of users. This should be sent only two variables along with the user id:

    Appnima.Network.following("28319319832", 0, 5);

The first variable is the page number you want to obtain, that is, the part of the list you want to get. Second number is the number of results you want to obtain. In the first call, this variable will be multiplied by 2, and in other cases, this variable is the same.

#### Followers
With this resource you can get the list of people that follow a user. It works as the previous parameter, accepting an optional user id parameter:

    Appnima.Network.followers();

    Appnima.Network.followers("28319319832");

Like as explained above, it is also possible to obtain results with pagination. The mode of this is the same as getting the users you follow.

In this case, the call returns one more variable in each object. This variable indicate that user is loggued is follow or not that user.

#### Information
This resource returns a user's relationships' stats and list of followers and followings. If we use a user's id as parameter it will return his stats, if not, logged user's stats:

    Appnima.Network.info();

    Appnima.Network.info("28319319832");


#### Check
With this resource yoa can get information about logged user's realtionship with other user, if I am following him and if he is my follower:

    Appnima.Network.check("28319319833");


#### Search
You can search for other users using this resource. For this use as parameter an email or username to search:

    Appnima.Network.search("javi@tapquo.com");



Location
========
Places
------
#### Search
Using the users latitude and longitude or any point you can get the nearest places such as museums, restaurants, public buildings… You also can provide a search radius to get a more accurate search. To do this use this respource  with the latitude, longitude and radius:

    Appnima.Location.places("43.6525842", "-79.3834173, 100");


#### Info
Get detailed information from a place using this resource. Send the `ID` and `REFERENCE` with the request:

    Appnima.Location.place("28319319833", "CqQBlwAAAEXdx350jL2InIRtksTkbZJ-m");


#### Add
In APP/NIMA you can add a place and add relevant information such as name, address, phone number… To do this yo have to use this resource with the following parameters: Name, address, locality, Postal Code, Country, latitude and longitude:

    Appnima.Location.add("Talleres Juan", "C/ Laubide 10", "Mungia", "48100", "ES", "43.354", "-2.8467");

Optionally you can add email, phone and web site data.


#### Checkins
You users can register the places where they are. Send the `ID` and `REFERENCE` with the request:

    Appnima.Location.checkin("28319319833", "CqQBlwAAAEXdx350jL2InIRtksTkbZJ-m");

You can get the list of checkins with this resource. The unique parameter needed is the user's id:

    Appnima.Location.checkins("28319319833");


People
------
#### Friends around
You can ask APP/NIMA for the friends that are near to you. The parameters needed are the latitude, longitude and radius:

    Appnima.Location.friends("43.6525842", "-79.3834173, 100");


#### People around
You can ask APP/NIMA for the people that is near to you. The parameters needed are the latitude, longitude and radius:

    Appnima.Location.people("43.6525842", "-79.3834173, 100");


Push
====
To send push notifications to the devices that the users have registered the data needed is: User's ID, the notification text, and the content of the notification.

    Appnima.Push.send("28319319833", "Mensaje", {"title": "JSON con los campos necesarios", "text": "Hola App/nima!"});


Socket
======
Groups
------
The groups are persistent rooms from 1 to N users. In the groups only allowed users can connect and send data. To work with groups an instance of group must be created:

    group = new Appnima.Socket.Group();

Create a group:

    group.create("name", ["2387569328yvc2","21y89ch3x8hg2032","2938tyh2g0ghh0i2bg8"]);

Get the groups I take part:

    group.list().then(function(error, groups){[code]});

Remove group:

    group.remove("id");

Change the name of the group:

    group.rename("id", "name");

Get the messages of the group. The second parameter is the page number, the first page is 0 and each page have 50 messages:

    group.messages("id", 0);

Delete unread messages count, the first parameter is the group id:

    group.deleteUnreadCount("id", callback);

Chat
----
Chats are rooms from 1 to N users without persistence. In chats, as happens in the groups, only allowed users can connect and send data.To work with Chats first of all a instance of Chat must be created:

    chat = new Appnima.Socket.Chat();

Create a room:

    chat.create("name", ["2387569328yvc2","21y89ch3x8hg2032","2938tyh2g0ghh0i2bg8"]);


Emiter & Listener
-----------------
#### Emiter
The emiter is a room in which the autor of it is the only person allowed to send messages and the receivers must connect to it using `Appnima.Socket.Listener`. To create it the first thing is creating an instance of Emiter:

    room = new Appnima.Socket.Emiter();

Create a room:

    room.create("context");


#### Listener
The listener connects to rooms created with emiter and receives it messages. To create it the first thing is creating en instance and it will connect automatically to the room:

    listener = new Appnima.Socket.Listener("context");


Application
-----------
The application channel allows the users of an application to communicate with others. To connect to it just create an instance of the application:

    application = new Appnima.Socket.Application();


Inbox
-----
The inbox allows a user to receive messages from other users. The messages will only be received by the room author, but any user can send data.To do this create an instance of Inbox:

    inbox = new Appnima.Socket.Inbox();

Get the unread messages count by group:

    inbox.unreadCount(callback);

Get online friends:

    inbox.onOnlineFriends(callback);

Friend connection:

    inbox.onFriendConnected(callback);

Friend disconnection:

    inbox.onFriendDisconnected(callback);

Send data to followers:

    inbox.sendToFollowers(data);

Send data to friends:

    inbox.sendToFriends(data);

Send data to an user:

    inbox.sendToUser(user_id, data);


User
----
Socket.User allows user to send messages to a user's inbox. Just create an instance:

    user = new Appnima.Socket.User("user id");


Methods
-------
This methods are the same for all the socket types seen previously:

* `instance.connect("id")`: Connect to a  Group or Chat

* `instance.disconnect()`: Disconnect from a room

* `instance.allowUSers(["","",""…])`: Add allowed users to Groups or Chats

* `instance.disallowUsers(["","",""…])`: Remove users from allewed list

* `instance.send(message)`: Send a message to all the users of a room, also the sender

* `instance.broadcast(message)`: Send a message to all the users of a room, except the sender

* `instance.onConnect(callback)`: Calls to the callback on room connection

* `instance.onError(callback)`: Calls to the callback on error

* `instance.onMessage(callback)`: Calls to the callback on message received, the recived message will be an object like this:
    * content:  "message content"
    * user: {"Information about the sender"}
    * data: {"Extra information sendend"}
    * created_at: "2013-11-16T05:55:02.736Z"

* `instance.onDisallow(callback)`: Calls to the callback when a user is disallowed from the group
