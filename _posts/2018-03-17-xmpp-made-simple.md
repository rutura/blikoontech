---
permalink : /tutorials/xmpp-made-simple-roster-and-presence-explained
title:  "XMPP Made Simple : Roster and Presence Explained."
search: false
categories: 
  - XMPP
classes: wide
sidebar:
  - text: "[![your logo](/images/xmpp-smack-post/SmackCourseAd2.png)](https://blikoon.teachable.com/p/android-xmpp-chat-app-video-tutorial)"
last_modified_at: 2018-03-17T08:06:00-05:00
---
## Intro 
In a previous tutorial, we did our best to explain the basic concepts of XMPP in layman terms. We touched on the architectural and addressing aspects of the protocol and looked at its building blocks. In this tutorial we are moving one step up and looking at how XMPP manages contact lists and how it lets us advertise online status (online|offline) to our contacts. We will also explore how we can impose restrictions on who can see your online status. Lets get started shall we?

## Presence Demystified

In XMPP, the presence keyword can be misleading to new comers because it is interchangeably used to describe  two different things. First it is used in sentences like “Alice is sending presence information to Bob” or “When your XMPP Client connects successfully to the server, it is going to send presence information to the contacts that are subscribed to your presence”. In this case presence simply means whether you are online or not(offline). Now you know the first meaning of presence: Presence = Online status.

Secondly you’ll see it in sentences like ” Syntactically , it is a presence stanza of type…. the server sends to the client to achieve such and such functionality”. In this case, presence simply means a stanza of type Presence. You know what a stanza is right ? If not please have a look at this introductory tutorial. From now on , when you see the word presence in some XMPP documentation, it’s most probably going to be referring to either online status or simply presence as a stanza type. I wanted to get this out of the way because I get many questions on this.

Now we’ve covered the MEANING of presence.  From the perspective of USAGE, a presence stanza   is  usually used to do two different things.

* Advertising online/offline status information
* Controlling Contact subscriptions

We differentiate these two USAGEs by looking at the type of the presence we are dealing with. When we are dealing with Presence stanzas of type available- unavailable, we usually are sending-receiving online status information to-from our contacts. There are 4 other presence types that we haven’t seen yet and these are subscribe - subscribed - unsubscribe- unsubscribed .We use them when we want to control who can see our presence information or to ask to see other contacts’ presence information. It should be  understood that we can use these subscription related presence stanzas to request the server to stop sending us presence information for such and such contact, effectively unsubscribing to their online status.More on this in the sections below.

<a href="https://blikoon.teachable.com/p/android-xmpp-chat-app-video-tutorial">**XMPP Smack Android Comprehensive Video Course Available!**</a> By the very same author of this tutorial.
{: .notice--warning}
[![foo]({{ site.url }}{{ site.baseurl }}/images/xmpp-smack-post/teachablexmppcourses.png)](https://blikoon.teachable.com/p/android-xmpp-chat-app-video-tutorial)

<a href="https://blikoon.teachable.com/p/android-xmpp-smack-chat-app-sending-and-receiving-files-http-file-upload">**Follow up course on Sending and Receiving Files with XMPP and Smack also available!**</a>
{: .notice--info}

## Roster Demystified

In XMPP, the term roster is used to refer to the contact list. A user’s contact list is mostly meant to be stored on the user’s server. In layman terms, if your jid is user@server.com, your contact list is stored on the XMPP server with the domain name server.com. I have had students ask me why store the contact list on the server and not on the client, in case you are one of those wondering why, let me list a couple of reasons I can think of

* Reliability : You have your XMPP client that you’ve been using for a few months and you have built a shiny contact lists of your friends so you can talk to them anytime. On one unfortunate day , BOOOM! you loose your phone. What do you do ? Good luck building it again as fast as you would have wished 😉 . Storing your roster on the server avoids these kinds of problems. Should you loose access to your XMPP client, as long as you remember your jid and password, all you have to do is log in again and your faithful XMPP server will give it back to you
* Multiple accounts : You may not know it, but XMPP allows you to use ONE SINGLE account and login from multiple XMPP clients. Lets say Alice has a jid of alice@server.com and she has XMPP clients installed on her Android phone , her laptop and her Desktop computer. This is possible because XMPP adds a resource section to your JID (See  the Friendly Introduction to XMPP). Now Bob ( bob@seerver.com) sends a friendship (subscription) request to Alice and Alice accepts the subscription from her Android phone, effectively adding Bob to her contact list. Now Alice’s Android phone knows of the new contact but the laptop and the Desktop computer are completely ignorant of the new contact. If Alice happens to be using her laptop talking to Bob after a while , the laptop will confuse her by treating Bob as a stranger because the laptop doesn’t recognize Bob as a contact. From the perspective of the laptop, Bob is not in Alice’s roster.Storing the roster on the server solves this problem because as soon as the server adds a contact to your roster, it notifies all the user’s connected resources(XMPP clients using the same JID)

<img src="{{ site.url }}{{ site.baseurl }}/images/xmpp-made-simple-roster-and-presence-explained/Xmpp_multiple_accounts.png" alt="Xmpp_multiple_accounts">


Now that you know what a roster is and where it is stored, time to look at how you add/remove contacts to your roster, or more specifically , what possible things your XMPP client does when you instruct it to  add/remove a contact to/from your roster.

## Adding an Entry to the Roster

There are two different ways you can add a new contact to the roster, one is by sending a roster set IQ stanza to the server, with the new roster entry inside as described in the RFC 6121 document and shown in the snippet below :

```xml
<iq from='juliet@example.com/balcony'
       id='ph1xaz53'
       type='set'>
     <query xmlns='jabber:iq:roster'>
       <item jid='nurse@example.com'
             name='Nurse'>
         <group>Servants</group>
       </item>
     </query>
   </iq>
```

When your XMPP client sends this stanza to your server, any XMPP compliant server ( any server that mostly respects the rules specified in XMPP RFC documents and numerous XEPs) responds by adding the new contact and any other additional piece of information, in this case the nick  name “Nurse” and the group you wish to add this new contact to. XMPP allows you to group the people in your roster in different groups but more on this later. This is one way to add a new contact to the server : by sending a roster set to the server with your new contact JID inside.

The second way is through the subscription mechanism. The details of subscription are covered in the section below, but right now I would like you to know that when Alice(alice@server.com)  sends a subscription request to Bob(bob@server.com) and Bob accepts,  server.com automatically adds bob@server.com to Alice’s roster. The flow is similar if it is Bob who is initiating the subscription request. Recap : two ways to add a contact to your roster,  through the roster set IQ or through the subscription mechanism.

## Removing a contact from the roster

If you guessed that we also have two ways to remove a contact from the user’s roster,  you might be surprised to beWRONG 😉 . We can only delete a contact from the user’s roster by sending a roster set IQ to the user’s server and by stuffing a value of “remove” to the subscription attribute as described in the RFC 6121 document and shown in the snippet below :

```xml
<iq from='juliet@example.com/balcony'
       id='hm4hs97y'
       type='set'>
     <query xmlns='jabber:iq:roster'>
       <item jid='nurse@example.com'
             subscription='remove'/>
     </query>
</iq>
```

When the server receives this IQ stanza, it removes the roster entry with the JID “nurse@example.com” and notifies all the connected user’s clients so they can sync their local contact lists.

In the section above we saw that Alice sent a subscription request to Bob, Bob accepted and the server added Bob to Alice’s roster. Now what happens if later if Alice decides to stop receiving presence information (you’ll see this also referred to as presence updates) from Bob ? We’ll see how she does that in a section below. The intuitive thing to think is that the server removes bob@server.com from Alice’s roster but I am not aware of the place where the RFC document says that. And the RFCs are XMPP developer’s constitution, every XMPP client and server should strive to comply with what the RFC says to the possible maximum. Now back to our problem, what what happens is that Bob remains in Alice’s roster as a zombie contact, in that there is absolutely no sharing of presence updates even though Bob sits in Alice’s contact list.  If Alice wants to remove Bob completely from her roster, she has to use a roster set IQ as described above.

## Presence Subscriptions

Because of the way we have been used to mainstream messengers like WhatsApp, Facebook Messenger and many others, the process of adding friends in XMPP is usually judged to  be cumbersome by normal people ( non geeks). In WhatApp for example when Alice adds Bob as a contact and sends him a message, Bob receives the message with a snackbar reminding him that Alice is a stranger. Bob can choose to keep talking to her as a stranger, or to add her as a contact at which point they become contacts. Bob and Alice can see each others presence (online status) and many other pieces of information ( last seen,…).

XMPP makes the process of adding friends more granular in that for Alice and Bob to be contacts ( where Bob can see Alice’s online status and Alice can see Bob’s online status) , Alice has to explicitly send a subscription request to Bob and Bob in turn has to send a subscription request to Alice. This may seem counter intuitive  but remember that XMPP is a protocol. It is a blueprint that provides the building blocks you use to build XMPP software. If you wanted to build the user experience like WhatsApp, you would do it using these building blocks provided by XMPP.

Now how do we send the subscription requests? A subscription request is nothing but a Presence stanza of type subscribe. When Alice wants to subscribe to Bob’s presence, she sends a presence of type subscribe

```xml
    <presence id='xk3h1v69'
    to='bob@server.com'
    type='subscribe'/>
```

When Bob receives the request, he can either accept, in which case his client responds with a Presence of type subscribed

```xml
<presence from='bob@server.com'
              id='xk3h1v69'
              to='alice@server.com'
              type='subscribed'/>
```

or she can deny the subscription by sending back a Presence of type unsubscribed

```xml
<presence id='tb2m1b59'
              to='alice@server.net'
              type='unsubscribed'/>
```

The id’s you see in these stanzas are used for tracking purposes. When Bob accepts the subscription request from Bob, we say that Alice is subscribed TO Bob’s presence.  The “TO” is a subscription status. We have four types of subscription status in XMPP . These statuses are considered from the perspective of the user or the contact. If we take Alice as our user and Bob as our contact, we have 4 possible subscription status as shown below:

* NONE : in which Alice and Bob are not subscribed to each other’s presence
* TO  :  Alice is subscribed to Bob’s presence
* FROM : Bob is subscribed to Alice’s presence
* BOTH : Alice is subscribed to Bob’s presence and Bob is subscribed to Alice’s presence

The process by which each subscription status is attained is described in the image below :

<img src="{{ site.url }}{{ site.baseurl }}/images/xmpp-made-simple-roster-and-presence-explained/XMPP_Subscription_Flow.png" alt="XMPP_Subscription_Flow">


Initially, user and contact don’t have presence subscription with each other and user is not in contact’s roster. Similarly, contact is not in user’s roster. The subscription mechanism is started when user sends a Presence stanza of type subscribe to contact. When contact gets the stanza, they have two choices ,

* They can accept the subscription request, in which case their XMPP client responds with a presence stanza of type subscribed.  After this , from the perspective of user, the subscription status they have with contact is “TO”. From the perspective of contact, the subscription status they have with user is “FROM”. These subscription status can be confusing, so take some time to sort them through, take a look at the image above where these states are depicted. After this process, user’s server adds contact in user’s roster with a subscription of TO, and contact’s server adds user in contact’s roster with a subscription of FROM.
* They can reject the subscription request in which case their XMPP client responds with a presence stanza of type unsubscribed.  When user gets the stanza, the process stops and nothing more happens.

We have seen the process of user subscribing to contact. The process is the same when contact subscribes to user. When user and contact’s relationship reaches a point where they are subscribed to each other, we say that they have a subscription of BOTH. When user wants to stop receiving presence updates from contact, they just send a presence of type unsubscribe. When the server gets this stanza, it changes the subscription status accordingly and informs  all user’s connected resources of the new  subscription status .

Supposing that user is initially subscribed to contact, lets say that contact wants to cancel the previously granted presence subscriptions. They do this by sending a presence of type unsubscribed to their server which updates the subscription status accordingly, notifies contact’s all connected resources of the new status, and informs user’s server of the new status. User’s server in turn updates the subscription status accordingly in the roster and notifies user’s connected resources of the new subscription status as well.

One should note that there are intermediary subscription states to take into account pending subscription requests as described in the RFC document.

## Roster Retrieval

We have talked about all aspects of the roster, how it is built, how you add entries to it and how you delete entries but we didn’t talk about how XMPP clients retrieve the roster to sync their local contact lists. XMPP clients can retrieve their rosters at any time by sending an IQ stanza of type get to the server. The stanza should have a child query item qualified by the jabber:iq:roster namespace.

```xml
<iq from='juliet@example.com/balcony'
       id='bv1bs71f'
       type='get'>
    <query xmlns='jabber:iq:roster'/>
  </iq>
 ```

When the server gets this stanza, and you happen to have a roster, it responds with your roster as follows

```xml
<iq id='bv1bs71f'
       to='juliet@example.com/chamber'
       type='result'>
    <query xmlns='jabber:iq:roster' ver='ver7'>
      <item jid='nurse@example.com'/>
      <item jid='romeo@example.net'/>
    </query>
  </iq>
```

Notice that in this case, we have two entries in our roster. Another thing you should notice is the IDs that are attached to these stanzas. You can see that our IQ-get and IQ-result stanzas both have the same id of bv1bs71f. This is XMPP’s way of allowing us to track stanzas and know which response corresponds to which request.

We have covered all the information I deem necessary to start building Presence and Roster features into your next XMPP client software project. Once you have this knowledge , you can use your favorite XMPP library to get all this done. These are considered basic features of any XMPP software so there is a very good chance whatever client or server software you choose will  have all these features built in ready for you to use. If you are into Android, there is a library called Smack for which I have made a video course and it shows how to build these features into your android App and many more features.

I also have a getting started written tutorial on using Smack to build your XMPP client on Android in case video courses are not your thing. Either way if you have any question, comment or suggestion, don’t hesitate to ping me in the comments below. I hope this has been informative to you and thanks for reading.

<a href="https://blikoon.teachable.com/p/android-xmpp-chat-app-video-tutorial">**XMPP Smack Android Comprehensive Video Course Available!**</a> By the very same author of this tutorial.
{: .notice--warning}
[![foo]({{ site.url }}{{ site.baseurl }}/images/xmpp-smack-post/teachablexmppcourses.png)](https://blikoon.teachable.com/p/android-xmpp-chat-app-video-tutorial)

<a href="https://blikoon.teachable.com/p/android-xmpp-smack-chat-app-sending-and-receiving-files-http-file-upload">**Follow up course on Sending and Receiving Files with XMPP and Smack also available!**</a>
{: .notice--info}

# Comments 

  **Commenting not available on this page at the moment! Please hit the author on various social media channels or share your thoughts on this github issue. Worthy conversations will be reproduced here. Thanks for understanding**
{: .notice--info}