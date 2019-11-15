---
permalink : /tutorials/sending-and-receiving-files-with-xmpp-http-file-upload-explained
title:  "Sending and Receiving Files with XMPP : Http File Upload Explained."
search: false
categories: 
  - XMPP
classes: wide
sidebar:
  - text: "[![your logo](/images/xmpp-smack-post/SmackCourseAd2.png)](https://blikoon.teachable.com/p/android-xmpp-chat-app-video-tutorial)"
last_modified_at: 2018-03-17T08:06:00-05:00
---

In the previous tutorials, we covered the basics on XMPP and visited the Roster and saw how you can add your contacts in XMPP. In this one, we’re tackling an important problem of sending and receiving files.Everybody expects an instant messenger these days to be able to send and receive any kind of file. XMPP being one of the competing Instant Messaging Protocols, has attempted to come up with ways to allow the sending and receiving of files but most of the techniques highly depend on there being a single peer to peer connection( sender to receiver) over which the files are sent.

This becomes a problem in today’s expectations where you need to send the files in group chats, multiple connected clients on the same account. Simply put, a better tool for the job was needed and most people where already using a hack of uploading the file to some HTTP server and then sharing the link to that file. This worked all fine but the problem was that there was no standardized way to do this to have all our XMPP software inter-operate. Luckily, somebody took the initiative and we now have a standard to follow : Http File Upload.

Http File Upload was one of the fastest to be adopted among XMPP Extension Protocols. This high adoption rate is probably  due to the fact that HTTP is all over the place and there was no need to learn something new and most XMPP clients had some HTTP infrastructure laying around anyway ( Doing REST, Downloading stuff,…). On the server side, it was also not that difficult to delegate the handling of uploading and downloading of files to well know tools for the task like Apache or Nginx and crafting a minimal HTTP server for the task wasn’t that hard for those suffering from the NIH syndrome 🙂

Needless to say Http File Upload was a really good quick win ( Easy to implement : Solves a HUUUUGE problem). Now, how does Http File Upload work and most importantly how do I use it in my project ? Below is my attempt to put the process into a flow chart.


<img src="{{ site.url }}{{ site.baseurl }}/images/http-file-upload/Http-File-Upload-Flow-Chart.png" alt="Http-File-Upload-Flow-Chart">

## Check Server Support

We start with a client checking whether it’s server supports Http File Upload or not. The check is done by sending an IQ get like shown below

```xml
<iq from='romeo@montague.tld/garden'
    id='step_01'
    to='montague.tld'
    type='get'>
  <query xmlns='http://jabber.org/protocol/disco#items'/>
</iq>
```

The iq stanza is of type get, we’re trying to get some kind of information, and it has a child query element qualified ty the namespace xmlns=‘http://jabber.org/protocol/disco#items’. When the server gets this, it knows somebody is asking :” Could you please tell me the features  you support ?”. Any XMPP compliant server is supposed to respond in kind with a list of its supported features. It does so with an IQ result as shown below

```xml
<iq from='montague.tld'
    id='step_01'
    to='romeo@montague.tld/garden'
    type='result'>
  <query xmlns='http://jabber.org/protocol/disco#items'>
    <item jid='upload.montague.tld' name='HTTP File Upload' />
    <item jid='conference.montague.tld' name='Chatroom Service' />
  </query>
</iq>
```

In this case the server is saying that it supports two features, Http File Upload and MUC. We know that Http File Upload is supported we can move forward within our flowchart. But what if we don’t see Http File Upload in this list? In that case we stop trying to use it to send files and an XMPP client may choose to try other peer to peer sending mechanisms or stop and report that it can’t send the files all together. This is all open for the client developer to decide.

If it is supported then the client asks for more information about the Http File Upload feature advertised by the server.It does so by sending another IQ get stanza, but this time qualified by a different namespace

```xml
<iq from='romeo@montague.tld/garden'
    id='step_02'
    to='upload.montague.tld'
    type='get'>
  <query xmlns='http://jabber.org/protocol/disco#info'/>
</iq>
```

In English, this translates to please tell me more about you. The server should respond with whatever information it deems worthy sharing at this time, and it uses an IQ result stanza

```xml
<iq from='upload.montague.tld'
    id='step_02'
    to='romeo@montague.tld/garden'
    type='result'>
  <query xmlns='http://jabber.org/protocol/disco#info'>
    <identity category='store'
              type='file'
              name='HTTP File Upload' />
    <feature var='urn:xmpp:http:upload:0' />
    <x type='result' xmlns='jabber:x:data'>
      <field var='FORM_TYPE' type='hidden'>
        <value>urn:xmpp:http:upload:0</value>
      </field>
      <field var='max-file-size'>
        <value>5242880</value>
      </field>
    </x>
  </query>
</iq>
```

For example in the response above, we know that if we try to send files more than 5M, we will get an error, and a wise client might to compress the files before sending or even alerting the user, saving them from getting these cryptic errors to deal with.

## Requesting Upload Slot

At this point we can proceed with the next step, which is requesting a slot from the server.

<img src="{{ site.url }}{{ site.baseurl }}/images/http-file-upload/step_request_slot.png" alt="step_request_slot">


Now what is a slot in the first place ? Well, a slot is really a permission from the server to upload a file to a PUT url it gives you  and for anyone on the planet who has the GET url it gives you, to be able to download the file. In short a slot = Pemission + PUT url + GET url. Now, a client requests a slot using the mighty IQ get stanza as shown below

```xml
<iq from='romeo@montague.tld/garden'
    id='step_03'
    to='upload.montague.tld'
    type='get'>
  <request xmlns='urn:xmpp:http:upload:0'
    filename='très cool.jpg'
    size='23456'
    content-type='image/jpeg' />
</iq>
```

It’s really the same IQ get stanza you’ve used already and the important thing in here is the payload which is a request child element qualified by the namespace xmlns=‘urn:xmpp:http:upload:0’ . Inside we specify the name of the file we want to send, its size and the mime type for it. When this is sent down the network connection pipe, your server has a choice to make : Grant or not Grant the slot ? It should grant the slot and if it doesn’t it should give you an error message with a reason why. An example of a grant response from the server is shown below

```xml
<iq from='upload.montague.tld'
    id='step_03'
    to='romeo@montague.tld/garden'
    type='result'>
  <slot xmlns='urn:xmpp:http:upload:0'>
    <put url='https://upload.montague.tld/4a771ac1-f0b2-4a4a-9700-f2a26fa2bb67/tr%C3%A8s%20cool.jpg'>
      <header name='Authorization'>Basic Base64String==</header>
      <header name='Cookie'>foo=bar; user=romeo</header>
    </put>
    <get url='https://download.montague.tld/4a771ac1-f0b2-4a4a-9700-f2a26fa2bb67/tr%C3%A8s%20cool.jpg' />
  </slot>
</iq>
```

The child element of interest here is the slot child element. It contains two elements of importance, namely the PUT url : url=‘https://upload.montague.tld/4a771ac1-f0b2-4a4a-9700-f2a26fa2bb67/tr%C3%A8s%20cool.jpg’ and the GET url : url=‘https://download.montague.tld/4a771ac1-f0b2-4a4a-9700-f2a26fa2bb67/tr%C3%A8s%20cool.jpg’ . With these two urls in hand the XMPP client attempting to send the file has two things to do . One is using its HTTP powers to upload the file to the specified PUT url

<img src="{{ site.url }}{{ site.baseurl }}/images/http-file-upload/HttpFileUpload_worked.png" alt="HttpFileUpload_worked">


Once the file is uploaded the client shares the GET url with anyone who might be interested in the file.The sharing mechanism is just sending a normal Message Stanza with the link to the file as the payload. Extensions like Out of Band Data can also be used to make it easier for the receiving party to know that it is a link to a file to download rather than other links that should be opened in the browser. When the other side gets the GET url, they in turn use their HTTP powers to download the file and do whatever it is the want to do with it.

One security aspect about HTTP File Upload is that anyone who gets their hand on the GET url can download and see the file. But how do they get their hands on it in the first place. The Http File Upload specification stresses that the PUT and GET url in your slot should have long cryptic paths/to/the/file , making them impossible to guess.

Now that you know the working concepts of Http File Upload, a logical next step is to want to use it in your XMPP client. Depending on the platform you are targeting I recommend looking around to see if there is no project or code base that provides the XMPP infrastructure you can base your work upon. The official XMPP website provides a good list you can look at ,but as always, nothing beats a good old google search or a hit on some keywords on github.

If you happen to be targeting the Android platform, they you’re in luck, we have published a complete step by step video course on udemy to guide you into how to use Smack to build Http File Upload support into your Android XMPP client

In closing as always don’t just take my word for it, take the time to read the Http File Upload Spec document yourself and gather all the info you need to start using this technology in your apps. For now I hope this has been informative to you and I would like to thank you for reading.

<a href="https://blikoon.teachable.com/p/android-xmpp-chat-app-video-tutorial">**XMPP Smack Android Comprehensive Video Course Available!**</a> By the very same author of this tutorial.
{: .notice--warning}
[![foo]({{ site.url }}{{ site.baseurl }}/images/xmpp-smack-post/teachablexmppcourses.png)](https://blikoon.teachable.com/p/android-xmpp-chat-app-video-tutorial)

<a href="https://blikoon.teachable.com/p/android-xmpp-smack-chat-app-sending-and-receiving-files-http-file-upload">**Follow up course on Sending and Receiving Files with XMPP and Smack also available!**</a>
{: .notice--info}

 One Comment
 
**Commenting not available on this page at the moment! Please hit the author on various social media channels or share your thoughts on this github issue. Worthy conversations will be reproduced here. Thanks for understanding**
{: .notice--info}

    sweetxdbz
    January 18, 2019 at 6:37 pm Reply	

    I am developing a prototype real-time chat on mobile phone. I had to use ionic 1. I use openfire. i am stuck on the part of share photos like whatsapp.

    I can get the slot with both link (get and put) but

    How do you upload the photo on the server with the put link ?
