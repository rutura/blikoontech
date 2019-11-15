---
permalink : /xmpp/xmpp-a-soft-friendly-introduction
title:  "A friendly introduction to XMPP"
search: false
categories: 
  - XMPP
classes: wide
sidebar:
  - text: "[![your logo](/images/xmpp-smack-post/SmackCourseAd2.png)](https://blikoon.teachable.com/p/android-xmpp-chat-app-video-tutorial)"
last_modified_at: 2016-01-12T08:06:00-05:00
---
<img src="{{ site.url }}{{ site.baseurl }}/images/header.jpg" alt="XMPP Foundations">

XMPP stands for eXtensible Messaging and Presence Protocol.It is an open standard protocol that is used to build real time applications.Example applications are Instant Messaging Apps ,White-boarding applications,real time gaming applications and many more.The protocol uses XML as a means to exchange information.In short XMPP allows you to send chunks of XML data from one node to another in real time .In this tutorial we lay down the conceptual concepts needed to make your XMPP journey as smooth as possible.

<a href="https://blikoon.teachable.com/p/android-xmpp-chat-app-video-tutorial">**XMPP Smack Android Comprehensive Video Course Available!**</a> By the very same author of this tutorial.
{: .notice--warning}
[![foo]({{ site.url }}{{ site.baseurl }}/images/xmpp-smack-post/teachablexmppcourses.png)](https://blikoon.teachable.com/p/android-xmpp-chat-app-video-tutorial)

<a href="https://blikoon.teachable.com/p/android-xmpp-smack-chat-app-sending-and-receiving-files-http-file-upload">**Follow up course on Sending and Receiving Files with XMPP and Smack also available!**</a>
{: .notice--info}


## Services Provided by XMPP

XMPP is usually broken into two parts : the XMPP Core Services and XMPP Extension Protocols (XEPs).The core part comprises of the features(services) deemed essential to most XMPP applications like

One to one Messaging : covered in RFC6121 ,enables you to send messages from one entity to another.This is the service that got me interested in XMPP in the first place.

Data Communication Security Mechanisms:covered in RFC6120 these services allow you to build secure applications using advanced encryption algorithms when you transmit data across the wire and flexible ways to authenticate the data source and integrity through technologies liek STARTTLS ,TLSand SASL

Presence and Contact Lists: covered in RFC6120 ,the presence service is in charge of notifying you of your contacts online status.The Contact List (Roster) Service deals with managing your contact lists.

Don’t worry if these don’t make any sense as of yet.We will explain in more details in the subsequent sections when the needed basic concepts have been explained.

The full list of features in the Core part of XMPP is specified in the RFC( Request For Comment) documents RFC6120 and RFC6121 published by the the IETF.

The extension part of the XMPP protocol contains services like:

Multi-User Chat :The service allows you to build Chat Rooms.

Service Discovery : The service allows for your client application to learn what services are available from the server it it connected to.

A more exhaustive list of services provided by the XMPP protocol can be at the XMPP protocol stack page .

With a portion of the services provided by XMPP slightly introduced one can combine them to build practical real time applications.For instance ,one can combine One to one chat + Multi User Chat + Presence + Contacts Lists to build a fully functional modern instant Messaging application like Whatsapp.

## How XMPP Pieces fit together

XMPP applications use the Client Server Architecture.One of the main ideas behind XMPP is to do the possible maximum to move the complex operations to the server side so it becomes easy to write client applications.The XMPP application architecture is as shown in the figure below :

<img src="{{ site.url }}{{ site.baseurl }}/images/friendly-intro-to-xmpp/client_server_xmpp.png" alt="XMPP Client Server">

We can see client nodes in the form of PCs and mobile phones and server nodes identified by server1.com ,server2.com and server3.com .Note that no client is directly connected to another client ,all client connections pass through the server.Clients can connect to the server through both wired links ( broadband connection) and wireless links ( Wifi wireless and 3G/4G Wireless).

A good point to note is that XMPP Servers can be federated.This means for example that when server2.com and server3.com are connected and know about each other and a client on server2.com sends a message to a client on server3.com ,sever2.com is smart enough to know that the message doesn’t belong to him and forwards the message to server3.com which in turn delivers the message to the connected client.This allows for XMPP systems that have been built independently to be able to communicate.

Another feature supported by most popular XMPP server is clustering where for example server1.com ,server2.com and server3.com can be configured to serve one xmpp domain like one_served_domain.com .Clustering is a topic beyond the scope of this tutorial so we won’t say more about it .

## XMPP Addressing

Being one of the network protocols ,XMPP entities should have a way of addressing nodes on the network.XMPP clients have addresses of the form “user@server.com” where “user”is the username and “server.com”is the domain.A node address in XMPP is called a Jaber ID abbreviated JID.The figure below shows four users addressed on the XMPP network. 

<img src="{{ site.url }}{{ site.baseurl }}/images/friendly-intro-to-xmpp/xmpp_addressing.png" alt="XMPP Addressing">

You may have noticed that user1 has apparently three addresses on the network.These are not different addresses ,we simply take into account that a user may connect on the same account from multiple devices.Lets say that user1 is connected to the server simultaneously from her PC ,her tablet and her phone.We distinguish between these devices using another portion added to the JID of the user called the resource as shown in the lower section of the figure above.user1 is then connected with devices whose resource are pc ,phone1 and phone2 respectively.A JID of the form “user@server.com” is called a bare JID ,and a JID of the form “user@sever.com/resource” is called a full JID.Servers on the XMPP network are addressed by the domain name only.In our XMPP network our server JIDs are server1.com ,server2.com and server3.com respectively.

## XMPP Client- Server Streams


Earlier in the tutorial ,we mentioned that XMPP allows real time transmission of XML data chunks.When you want to start a session with an XMPP server ,you open a long lived TCP connection and negotiate an XML stream to the server(from client to server) .When the server accepts ,it also opens an XML stream to the client( from server to client).


<img src="{{ site.url }}{{ site.baseurl }}/images/friendly-intro-to-xmpp/xmpp_client_server_stream.png" alt="xmpp_client_server_stream">


The details of XMPP streams are beyond the scope of this tutorial ,but for think of it this way,when your client succesfuly connects to a server ,two XML streams are opened,one from the client to the server (Green in the figure above) and one from server to client.(Black).Another way to understand this is that there are two XML stream files ,one client XML stream file and one server XML stream file,every time the clients sends something to the server ,the client writes in green to the client XML stream file and when the data gets to the server ,the data gets written in green on the server XML stream file.The same happens when the server responds.Its response is written in black on the server XML stream file as it moves out to the network link and as it gets to the client ,the data is written the client XML stream file.A more realistic portion of a client XML stream file is shown below:


<img src="{{ site.url }}{{ site.baseurl }}/images/friendly-intro-to-xmpp/xmpp_client_xml_stream_file.png" alt="xmpp_client_xml_stream_file">



In this client XML stream file ,the client is requesting the contact list for the “juliet@example.com/balcony” JID (Green contour) and the server responds with a list of contacts.Don’t worry to much if you don’t understand the details of this client XML stream file as the details are explained in sections to follow.

If you are a software developer writting XMPP client applications the client XML stream file is a priceless concept as it is there you see everything about the conversation between your client and your server ,XMPP libraries (client and server) usually provide a way to link the XML stream file to the debug output and you can easily see what is happening behind the hood.

## XMPP Communication Primitives

The most basic unit of communication in XMPP is called a stanza.A stanza is the smallest piece of XML data a client can send to a server ( server send to client) in one package.Stanzas have three possible names( XML tag names) in XMPP ,these can be a Message stanza , an IQ stanza and a Presence stanza.Each of these stanzas is handled differently by XMPP servers and clients.Stanzas have type attributes ,these can be used to further differentiate stanzas as we are about to see shortly.

### The Message Stanza

The <message/> stanza is meant to be used to send data between XMPP entities.It is a fire and forget thing.<message/> stanzas are not acknowledged by the receiving party be it the server or the client at the other end.Usually when you send a <message/> stanza from your client and no error of any kind is generated ,you can assume that it has been sent successfully.

<message/> stanzas have different types differentiated by the type attribute.The most used are the <message type=”chat”/> ( chat message stanza) , the < message type=”groupchat”/> ( groupchat message stanza) and the < message type=”error”/> (error message stanza) .The chat message stanza is used to send one to one messages across the xmpp network.The groupchat message stanza is used to send messages in Multi-User Chat rooms.And the error message stanza used to report when something has gone wrong.

Besides the type attribute ,the message stanza can have three other attributes ,namely the from attribute ( specifying the JID where the message comes from ) ,the to attribute( specifying the JID where the message is destined ) and the id attribute( used for tracking purposes for example when debugging ).A more complete message stanza example is shown below :


<img src="{{ site.url }}{{ site.baseurl }}/images/friendly-intro-to-xmpp/message_stanza_example.png" alt="message_stanza_example">


All the attributes we have mentioned can be seen in this message stanza.Note that the body of the message is encapsulated in <body>..</body> tags.This tells you that XMPP stanzas can sometimes contain child (nested) child XML tags to structure the information within.

### The Presence Stanza

The <presence/> stanza advertises the online status( network availability) of other entities.Presence works like subscription in XMPP.When you are interested in the presence of some JID ,you subscribe to their presence ,in other terms ,you tell the XMPP server “everytime this JID sends you a presence update ,I want to be notified”.Of course the server asks the JID holder if he accepts to disclose his presence information to you.When you accept ,the server remembers your decision and updates anyone subscribed to your presence whenever you change your online status.

Presence online status can be one of the following : chat : meaning that you are online and available for chat ; away : meaning that you are away from your device for a short time; xa :meaning that you are away from your device for an extended period of time and dnd: meaning do not disturb,stating that you are not to be disturbed obviously :-).A more complete presence update is shown below:


<img src="{{ site.url }}{{ site.baseurl }}/images/friendly-intro-to-xmpp/xmpp_away_presence.png" alt="xmpp_away_presence">


Having seen most of the attributes on the message stanza ,we can see that this presence update is coming from romeo@example.net/bar destined to juliet@example.com .Juliet must be subscribed to romeo’s presence updates to be able to receive these presences(And romeo must have accepted). The availability status is embedded within the <show/> XML tag.

It is important to note that just because A is subscribed to B’s presence updates doesn’t mean that B is automatically subscribed to A’s presence updates.If what we want is for A and B to be subscribed to each other’s presence updates.A has to explicitly subscribe to B and B has to explicitly subscribe to A.The details of how subscription is carried out are subject of another upcoming tutorial.

### The IQ stanza

The IQ( Info/Query) stanza is used to get some information from the server ( info about the server or its registered clients) or to apply some settings to the server.Just like the stanzas we have seen so far the IQ stanza also has the from/to/id and type attributes.Its type attribute can either be :get ,set ,result or error. < iq type=”get”/> stanzas are used to get(ask) some information ( from the server). <iq type=”set”/> stanzas are used to apply some settings to the server.When you send get/set IQ stanzas to the server ,it can reply either with an < iq type=”result”/> stanza when your request has been successfully processed by the server or and <iq type=”error”/> stanza when something has gone wrong with your request.The figure below shows an IQ stanza that we send to the server and the reply we get from the server.


<img src="{{ site.url }}{{ site.baseurl }}/images/friendly-intro-to-xmpp/xmpp_iq_request_response.png" alt="xmpp_iq_request_response">


The client sends an IQ get stanza to the server to request its contact list.We know it is asking for the contact list because of the jabber:iq:roster XML namespace.An XML namespace is a way of giving more details about what the stanza is meant to do.For example the server upon receiving the iq from the client ( the one with a C: on the left),it first sees that it is a get IQ and it knows the stanza is asking for some information.Looking the XML name space ( xmlns) it knows exactly what is being asked for :”the contact list for the the JID where the stanza comes from “. The XMPP engine in the server is programmed to know that when a client sends jabber:iq:roster namespaced IQ ,it wants to retrieve its contact list.There are other namespaces in XMPP for other uses and you will surely come accross them in your XMPPing journey.

The server responds with a list of the JID’s contacts wraped within a jabber:iq:roster namespaced <query/>tag.

## Server and Client Implementations

You now have enough basics about XMPP under your belt to start messing around with the XMPP protocol.To be able to do that you probably don’t want to down to the metal and build your own XMPP infrastructure.At least for now.You will need to build on top of something somebody else has already built.

I wrote this tutorial form the perspective of someone who’s new to XMPP and who wants to get started writing XMPP client applications .The most common scenario to do that is to choose an XMPP server and install it on your platform.After that you find yourself an XMPP library( written in your favorite programming language). Most libraries have getting started guides that help you get a basic XMPP client up and running.If you are into Java/android ,there is a library called smack for which I have written a getting started guide.

The XMPP.ORG page gives a good an regularly updated page about what XMPP software is out there.If you want to try the ejabberd server on a linux machine then you are in luck I have a few tutorials about how to install it both from source code and from debian packages. Following up on this tutorial I have also published a tutorial on Roster and Presence that you might want to take a look at.

XMPP is a fun and really enabling protocol.We have covered the concepts I deem most basic and most crucial to get up and running .Something like what I wish I had when I started learning about XMPP.If you have any questions or suggestions don’t hesitate to drop me a line.I hope this has been informative to you and thanks for reading

<a href="https://blikoon.teachable.com/p/android-xmpp-chat-app-video-tutorial">**XMPP Smack Android Comprehensive Video Course Available!**</a> By the very same author of this tutorial.
{: .notice--warning}
[![foo]({{ site.url }}{{ site.baseurl }}/images/xmpp-smack-post/teachablexmppcourses.png)](https://blikoon.teachable.com/p/android-xmpp-chat-app-video-tutorial)

<a href="https://blikoon.teachable.com/p/android-xmpp-smack-chat-app-sending-and-receiving-files-http-file-upload">**Follow up course on Sending and Receiving Files with XMPP and Smack also available!**</a>
{: .notice--info}


## Comments

 22 Comments
 
**Commenting not available on this page at the moment! Please hit the author on various social media channels or share your thoughts on this github issue. Worthy conversations will be reproduced here. Thanks for understanding**
{: .notice--info}

    Avatar	
    Ashok
    January 24, 2016 at 2:09 am Reply	

    Hey Dude,
    Thanks alot. simple, clear, useful. Keep writing this kind of stuff dude.
    Cheers…..
        gakwaya	
        gakwaya
        March 19, 2016 at 7:28 pm Reply	

        Thanks for the kind comment Ashok.
            Avatar	
            sivanthi
            May 12, 2016 at 2:55 pm Reply	

            I need some clearification.

            how to create jabberid and password in ejabberd
            how to handle
                gakwaya	
                gakwaya
                May 12, 2016 at 3:22 pm Reply	

                Depends on how you want to create your users.If you are creating admin users you can use the utility that comes with ejabberd like this:

                ./ejabberdctl register admin1 localhost ejabberdPassword

                Once you have your admin account you can alswo add users from the admin interface.Another option is to use the in band registration feature from an xmpp client that supports it.I have another tutorial for more details on this.Hope this helps.
    Avatar	
    Uyi
    September 10, 2016 at 9:38 am Reply	

    Thanks so much and i mean like so so so much. you just broke xmpp down to me now i have a good knowledge about it. moving on to your other tutorial on Smack API on Android.
        gakwaya	
        gakwaya
        February 24, 2017 at 2:59 am Reply	

        Glad we could help 🙂
    Avatar	
    Ragunath
    September 13, 2016 at 4:51 pm Reply	

    Hey gakwaya,
    Thanks alot. simple explanation with clear examples. Very informative for beginner. Keep writing this kind of stuff dude.
    Cheers…..
        gakwaya	
        gakwaya
        February 24, 2017 at 3:00 am Reply	

        Glad it was helpful 🙂
    Avatar	
    levi
    March 20, 2017 at 10:24 pm Reply	

    dude this is like the xmpp beginner’s holy grail!
        gakwaya	
        gakwaya
        April 23, 2017 at 10:56 am Reply	

        Glad you like it.
    Avatar	
    Hima
    April 9, 2017 at 9:36 am Reply	

    Nice explanation for a begnner..thank you!
        gakwaya	
        gakwaya
        April 23, 2017 at 10:56 am Reply	

        You are welcome.
    Avatar	
    Arun PM
    April 14, 2017 at 2:40 pm Reply	

    Hi Gakwaya,

    Such a nice article. Very useful, simple and easy to get started with XMPP this information. Keep crating same kind of articles.

    Thanks
    gakwaya	
    gakwaya
    April 23, 2017 at 10:57 am Reply	

    Welcome.
    Avatar	
    crengana
    August 15, 2017 at 6:30 pm Reply	

    Do I need to create these user accounts in the server before the users/clients can communicate ?
        gakwaya	
        gakwaya
        November 4, 2017 at 4:10 am Reply	

        Yes Crengana,
        You need to create these accounts.
    Avatar	
    shashidhar
    February 28, 2018 at 3:23 pm Reply	

    how to do it in node-xmpp
        gakwaya	
        gakwaya
        February 28, 2018 at 5:36 pm Reply	

        I don’t use NodeJs so I can’t help with that specific library, but just from searching , I found that node-xmpp hasn’t had any love for many years. If that’s the library you are using you should be using xmppJs which is having some recent updates. On their site they have some getting started code you can use. Hope this is helpful.
    Avatar	
    Larry Kiniu
    April 5, 2018 at 11:05 pm Reply	

    Thanks man for this awesome tutorial.
    Avatar	
    Victor
    June 6, 2019 at 2:53 am Reply	

    Hola,
    Excelente explicacion
    Tengo una consulta
    Deseo realizar aplicacion que funcione con datos en tiempo real (no es un chat).
    Estos datos habra una comunicacion uno a uno y en grupo.
    He visto que ejabberd esta orientado a un chat y creo que ya tiene una estructura de base de datos. pero yo tengo otra estructura.
    Mi pregunta es ¿Me recomiendas ejabberd?
    ¿Que me recomiendas para realizar algo mas custom? ¿Por donde puedo empeza?
    Nota: He estado aprendiendo Erlang viendo que ejabber esta basado en dicho lenguaje,
    Gracias.
    Avatar	
    Andrei Podznoev
    August 1, 2019 at 12:06 pm Reply	

    Thank you very much for a concise and understandable explanation of XMPP basics!
    Avatar	
    Ali Raza
    October 14, 2019 at 7:36 am Reply	

    Its a really good information Kindly keep spreading your knowledge Thanks