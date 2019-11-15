---
permalink : /android-xmpp-smack-client-tutorial-comments
title:  "Android Smack XMPP Introduction:Building a Simple Client | Comments"
search: false
classes: wide
sidebar:
  - text: "[![your logo](/images/xmpp-smack-post/SmackCourseAd2.png)](https://blikoon.teachable.com/p/android-xmpp-chat-app-video-tutorial)"
last_modified_at: 2016-04-29T08:06:00-05:00
---

**This Post is no longer accepting comments here. This is an archive of the comments we've collected over time just for anybody they can help. If you wantto get in touch, hit us on facebook**.
{: .notice--warning}


 161 Comments

    Avatar	
    Emmanuel
    May 16, 2016 at 5:13 am Reply	

    Hi man!
    Thanks very much.
    I’m just getting this error: “java.security.cert.CertPathValidatorException: Trust anchor for certification path not found.”
    Smack version 4.1.7

    Do i need some kind of ssl library?

    Greetings.
        gakwaya	
        gakwaya
        May 16, 2016 at 10:57 am Reply	

        Looks like it has something to do with your servers ssl certificate’s configuration.Are you sure it is properly configured?I would try with a public server I know to be working and start working out the problem from there.Hope this helps.
            Avatar	
            Android Devloper Bala (ADB)
            November 16, 2016 at 5:15 pm Reply	

            builder.setSecurityMode(ConnectionConfiguration.SecurityMode.disabled);

            use this line. your problem will solve
                gakwaya	
                gakwaya
                November 17, 2016 at 9:10 am Reply	

                While disabling security may work fine while testing,it is recommended to set up security.It really isn’t that hard to set up.I have a recent tutorial on how to set up a certificate for a web server :

                http://www.blikoon.com/tips-and-tricks/secure-your-nginx-site-with-letsencrypt-free-certificates

                May be should adapt that to xmpp servers 🙂
    Avatar	
    Dharmesh
    June 29, 2016 at 4:04 pm Reply	

    thanks
    its nice working when i login it displays online user on spark client but when i send message, no process is done. How can i check receiver side message? Give me a solution
        gakwaya	
        gakwaya
        August 26, 2016 at 4:10 pm Reply	

        Is it Spark or Rooster not receiving the message?If all else fails I would download the source code from github and work my way back from there.
        Avatar	
        akhilesh
        October 3, 2016 at 7:32 pm Reply	

        Hello Dharmesh use
        chatManager = ChatManager.getInstanceFor(mConnection);
        chatManager.addChatListener(this);
    Avatar	
    Dhaval Prajapati
    July 3, 2016 at 9:20 pm Reply	

    hi, can you please guide me on how to reconnect to xmpp server? the case is when i try login and connects its working fine, and then when all of a sudden internet is not there it closes the connection. when internet comes back i have added to reconnect. the reconnection connects to xmpp and then shows stream conflict saying that Client is already logged in. can you help me with this? what is the proper reconnection process?

    thanks.
        gakwaya	
        gakwaya
        August 26, 2016 at 4:14 pm Reply	

        The issue you are facing is because Rooster does not handle stream / connection management.Rooster is kind of a getting started guide on smack .If you want a proper client that takes care of all those details (that uses smack) please have a look at xaber . Conversations is another client worth a look at that takes care of these issues,(it doesn’t use smack but the underlying concepts are the same.)
    Avatar	
    Volker
    August 1, 2016 at 1:01 am Reply	

    Thanx for this nice tutorial!
    I can send Messages, but I can´t receive them :/
    I have implemented the ChatMessageListener but it seems that the
    @Override
    public void processMessage(Chat chat, Message message) {
    Methode is never called.
    Can u tell me what I can do?

    Thank u very much!
        gakwaya	
        gakwaya
        August 26, 2016 at 4:16 pm Reply	

        Do you experience the same problem if you compile the source code from github?
            Avatar	
            thierry
            September 12, 2016 at 2:35 pm Reply	

            First of all, thanks for this great tuto and nore generally for sharing your knowledge with others.
            ! i experience exactly the same… send yes but receive no 🙁
            and i also noticed processMessage is not called (from mobile to mobile) however it’s surprisingly working with Pidgin (processMessage is called) !!!
            how is that possible with exactly the same code ??
            thanks for your answer
        Avatar	
        akhilesh
        October 3, 2016 at 7:34 pm Reply	

        please do like this

        @Override
        public void authenticated(XMPPConnection connection, boolean resumed) {
        RoosterConnectionService.sConnectionState = ConnectionState.CONNECTED;
        Log.d(TAG, “Authenticated Successfully”);
        showContactListActivityWhenAuthenticated();
        chatManager = ChatManager.getInstanceFor(mConnection);
        chatManager.addChatListener(this);
        MultiUserChatManager.getInstanceFor(mConnection).addInvitationListener(this);

        }
        its working from my side
            Avatar	
            Azher
            January 2, 2017 at 8:12 pm Reply	

            in which class this code is being added..?
            Avatar	
            saurabh
            February 15, 2017 at 12:24 am Reply	

            can u please explain this a bit more akhilesh ?
            Avatar	
            saurabh
            February 15, 2017 at 2:14 am Reply	

            getting errors on these !!
            chatManager.addChatListener(this);
            MultiUserChatManager.getInstanceFor(mConnection).addInvitationListener(this);

            can you please explain the changes a bit more, it will be very helpful.
    Avatar	
    ahmad
    August 13, 2016 at 5:56 pm Reply	

    hi .
    thanks alot
    when i run the apk in device and want login when in click sigin in button the app React no action and nothig change . and stay in first activity .
    please help me
        gakwaya	
        gakwaya
        August 26, 2016 at 4:17 pm Reply	

        Hi Ahmad ,I need more details as to how you got to the problem you are facing to be able to help.
        Avatar	
        saurabh
        February 15, 2017 at 2:15 am Reply	

        your connection is not authenticating !! try to login with correct credentials on a public server like xmpp.jp
    Avatar	
    Devias
    August 22, 2016 at 2:21 pm Reply	

    Hello.
    I use jappix.com (server)
    When i run the apk in device and send message to some user (user@jappix.com) in another device not shoving my message, but when i use web wersion jappix.com work fine. Why ?
        gakwaya	
        gakwaya
        August 26, 2016 at 4:21 pm Reply	

        Hi!
        I don’t know about jappix web version so I can’t be of help here.(May be it is filtering messages from unkown resources?). But any standard xmpp client should be able to receive messages sent by Rooster.
            Avatar	
            Devias
            August 29, 2016 at 1:35 pm Reply	

            Thank you.
            But i use Rooster – Rooster dialog.
            This don’t work.
            With any client work fine.
    Avatar	
    Zine
    August 22, 2016 at 7:44 pm Reply	

    Hello,

    Thank you very much for your Tuto.

    I have a noob question, i have an OpenFire server and i would like to use it with your app, where to declare it and how ?

    Thank you
        gakwaya	
        gakwaya
        August 26, 2016 at 4:25 pm Reply	

        Hello Zine,
        You don’t need to declare the server anywhere in the source code.If your server is live and some ip address or domain ,you just need to login with the jid(user@server.com) and password. I happen to also have a tutorial on the basics of xmpp :

        http://www.blikoon.com/xmpp/xmpp-a-soft-friendly-introduction

        May be it can help.
    Avatar	
    Andrey
    August 26, 2016 at 2:10 am Reply	

    First of all, thanks for your generous work, your two recent tutorials (this one and about ReceiverView with MVC concept) are very detailed and well-written.

    During the execution of this exercise I’ve found an minor bug in your instructions.
    LoginActivity misses mContext variable assignment, this results in the error shown below:
    java.lang.RuntimeException: Error receiving broadcast Intent { act=rooster.uiauthenticated flg=0x10 pkg=rooster } in rooster.LoginActivity$3@c09717e
    Caused by: java.lang.NullPointerException: Attempt to invoke virtual method ‘java.lang.String android.content.Context.getPackageName()’ on a null object reference

    To fix the issue, replace it reference in onResume method of LoginActivity with context variable already passes as a parameter, like this:
    Intent i2 = new Intent(context,ContactListActivity.class);
        gakwaya	
        gakwaya
        August 26, 2016 at 4:26 pm Reply	

        Thanks Andrey , noted.
    Avatar	
    Rao
    August 30, 2016 at 3:23 pm Reply	

    Hey!
    Thanks very much for the tutorial. I have downloaded the source from github. I am using an open fire xmpp server. I have two test users created.

    I am able to login using the users from two separate devices. And I have changed a row in the contact model to the corresponding username. Eg: “username”@ipaddress. But when I send messages, the other device doesn’t receive any.

    Any help is appreciated.

    Thanks
        gakwaya	
        gakwaya
        September 4, 2016 at 11:05 pm Reply	

        Hello Rao,
        May be a problem with the server(Openfire) filtering out messages?I have tested this only with an ejabberd server and it worked.If possible please test with other publicly known servers like “xmpp.jp”.I wish I had time to investigate more on this and help but I don’t right now.
            Avatar	
            saurabh
            February 15, 2017 at 2:11 am Reply	

            Dear Daniel ! problem still persists, I have tested this on xmpp.jp. Its not working unfortunately. Kindly help us out, it will be much appreciated. Thanks.
            Avatar	
            saurabh
            February 15, 2017 at 2:20 am Reply	

            hi daniel !! tested with xmpp.jp too !! problem still persists, please try to help us out !! much appreciated if you can please take out some time and help us out. This issue is being asked many times in the comments.
            Avatar	
            saurabh
            February 16, 2017 at 11:14 pm Reply	

            problem still persists even with xmpp.jp !!! please i request to take some time and look into this issue.. its serious and will solve your purpose of this tutorial !! Please help !!!
        Avatar	
        shriya
        December 10, 2016 at 9:01 pm Reply	

        Hi,
        I am developing group chat using xmpp server. can you guide me for it or let me know github link for it.
        Avatar	
        shriya
        December 10, 2016 at 9:02 pm Reply	

        PLEASE SHARE GITHUB LINK
        Avatar	
        saurabh
        February 15, 2017 at 2:24 am Reply	

        Dear Daniel !! Very nice of you for such a detailed tutorial and keep up the work !!
        This tutorial really serves good knowledge for understanding XMPP chat.

        But I am stuck on a issue and it is also reported by other as I can see in the comments too, I think you are not able to reproduce it, please help us out. I followed the following steps:

        1. After downloading and successfully building your project from Github,

        2. Created two test users abc@xmpp.jp and xyz@xmpp.jp.

        3. Now in order to make them communicate with each other, I have to create two different logins i.e. one from abc@xmpp.jp and other from xyz@xmpp.jp and add vice versa to contact list of each other.

        4. So, to do this I modified the contactModel class and added contact abc@xmpp.jp and logged in from credentials of xyz@xmpp.jp.

        private void populateWithInitialContacts(Context context)
        {
        //Create the Foods and add them to the list;

        Contact contact1 = new Contact(“abc@xmpp.jp”);
        mContacts.add(contact1);
        }

        5. And to prepare other client I added contact xyz@xmpp.jp and logged in from credentials of abc@xmpp.jp.

        private void populateWithInitialContacts(Context context)
        {
        //Create the Foods and add them to the list;

        Contact contact1 = new Contact(“xyz@xmpp.jp”);
        mContacts.add(contact1);
        }

        6. Now,I have two different devices ready to communicate with each other such that I have abc@xmpp.jp in contact list of xyz@xmpp.jp and vice versa.

        7. But, when I select the contact to send the message there is no message received at the other end. However, In a case where sender and receiver is same message is received i.e. login from abc@xmpp.jp and adding abc@xmmpp.jp to contact list.

        I am helpless because I have invested much time in this tutorial and this tutorial is just what I need to start with. I would request some of your time to this issue, only then it will solve your purpose and benefit learners like us.

        Thanks in advance.
            Avatar	
            Neha
            August 14, 2018 at 12:22 pm Reply	

            How did you crested the two test users ?I’m not able to do it .Please help me .
                gakwaya	
                gakwaya
                August 16, 2018 at 1:24 pm Reply	

                You create the accounts on an XMPP server of your choice. I recommend first getting the XMPP basics nailed down before doing this tutorial. You can do that here : https://www.blikoontech.com/xmpp/xmpp-a-soft-friendly-introduction
    Avatar	
    aloy
    September 3, 2016 at 7:51 pm Reply	

    please help me sir, I’m making a family chat application(android) to keep us
    together, I’m good at java and android java to some level, i want on the app
    first installation for it to request that my uncle or brother or aunt sign Up
    with only their contact and then a short int is sent to them for verification
    then they type it and register them selves on the openfire server. Pls help me
    i dnt know how to do this…..i need to do this so that they will appreciate my
    work and finally see that I’m trying. Pls help me sir!
        gakwaya	
        gakwaya
        September 4, 2016 at 11:09 pm Reply	

        XMPP can do everything you mentioned exept sending the verification code.For there are plenty of paid services.A quick one is “nexmo.com”.For other basics please read this basics tute : http://www.blikoon.com/xmpp/xmpp-a-soft-friendly-introduction and follow up with this smack android tute.

        Good luck on your project.
            Avatar	
            Kopano
            November 17, 2017 at 4:13 pm Reply	

            Or aloy could use Firebase email authentication? its free to use
    Avatar	
    aloy
    September 3, 2016 at 8:02 pm Reply	

    Please Sir, don’t turn me down like other tutorials did.
    Avatar	
    abe
    September 4, 2016 at 1:41 am Reply	

    Nice Tutorial, so much love from me. I would like you to hint me on how to write plugin for openfire xmpp server.
    and i would like to ask if it’s possible to request an image from xmpp openfire server? I would so much appreciate any suggestion,
        gakwaya	
        gakwaya
        September 4, 2016 at 11:15 pm Reply	

        Hello abe,
        I am not much of user of openfire so I am afraid I can’t be of much help.If you join this muc “open_chat@conference.igniterealtime.org” you may get in touch with people that can help.

        Regarding requesting images from xmpp servers: XEP_0084 : http://xmpp.org/extensions/xep-0084.html might be what you need.
    Avatar	
    thierry
    September 13, 2016 at 12:58 pm Reply	

    First of all, thanks for this great tuto and nore generally for sharing your knowledge with others. Please forgive me for putting last message at a wrong place 🙁
    ! i experience exactly the same… send yes but receive no
    and i also noticed processMessage is not called (from mobile to mobile) however it’s surprisingly working with Pidgin (processMessage is called) !!!
    how is that possible with exactly the same code ??
    thanks for your answer
        Avatar	
        Erick Ribeiro
        October 2, 2016 at 1:00 pm Reply	

        o mesmo está acontecendo comigo ;(, alguém poderia ajudar?
    Avatar	
    Nirali
    September 15, 2016 at 6:10 pm Reply	

    Hey, I am not able to Login. So please help me to solve this problem and tell me, how to login from this app?
    Avatar	
    Nomi
    September 18, 2016 at 5:31 pm Reply	

    Hi gakwaya,
    Thanks for sharing such a nice tutorials.I am creating this one,and i have also tried your github source code.It login properly from conversation.im and xmpp.jp.But when i try to comunicate between two users,then messages will not share.
    Can you please help me in this regard.
    I shall be very thankful to you.
    Avatar	
    Adrian
    September 22, 2016 at 12:35 am Reply	

    Nice tutorial

    I think you forgot to mention calling the setupUiThreadBroadCastMessageReceiver
    from within the connect method in RoosterConnection.java its remarked out in the example above but enabled in the GIT

    best regards
        Avatar	
        Adrian
        September 23, 2016 at 9:15 pm Reply	

        I had the same problem with receiving messages – process message never fires
        I tried adding this after mConnection.login and it fires OK. Im using Openfire and the windows client Spark for testing

        StanzaTypeFilter message_filter = new StanzaTypeFilter(Message.class);
        mConnection.addSyncStanzaListener(new StanzaListener() {
        @Override
        public void processPacket(Stanza packet) throws SmackException.NotConnectedException {

        Log.i(TAG, “Processing Packet”);
        Message message = (Message)packet;
        if(message.getType() == Message.Type.chat) {
        String msg_xml = packet.toString();
        Log.i(TAG, “Message “+ msg_xml);
        if (msg_xml.contains(ChatState.gone.toString())) {
        //handle has closed chat
        Log.i(TAG, “Gone”);
        } else if (msg_xml.contains(ChatState.paused.toString())) {
        // handle “stopped typing”
        Log.i(TAG, “Paused”);
        } else {
        //single chat message
        Log.i(TAG, “Chat “+ message.getBody());
        }
        } else if(message.getType() == Message.Type.groupchat) {
        //group chat message
        Log.i(TAG, “GroupChat – “+ message.getBody());
        } else if(message.getType() == Message.Type.error) {
        //error message
        Log.i(TAG, “Error – “+ message.toXML());
        }

        }
        }, message_filter);
    Avatar	
    Aatif
    October 3, 2016 at 2:43 pm Reply	

    Hi man thanks for posting.
    But i am facing the following problems while adding the dependencies in build.gradle. (I have added the internet permission in manifest.xml file)

    F:\MyBook\Android_Workspace\MyApplication2\app\build.gradle
    Error:(27, 13) Failed to resolve: org.igniterealtime.smack:smack-tcp:4.1.0-alpha6
    disable.gradle.offline.mode” rel=”nofollow”Disable offline mode and Sync/MyBook/Android_Workspace/MyApplication2/app/build.gradle” rel=”nofollow”Show in Filehref=”open.dependency.in.project.structure” rel=”nofollow”Show in Project Structure dialog
    Error:(26, 13) Failed to resolve: org.igniterealtime.smack:smack-android-extensions:4.1.0-alpha6
    href=”disable.gradle.offline.mode” rel=”nofollow”>Disable offline mode and Sync”/MyBook/Android_Workspace/MyApplication2/app/build.gradle” rel=”nofollow”Show in Filehref=”open.dependency.in.project.structure” rel=”nofollow”Show in Project Structure dialog
        Avatar	
        Aatif
        October 3, 2016 at 4:30 pm Reply	

        ok I found a solution for this problem just needed to disable offline mode and sync it
    Avatar	
    akhilesh
    October 3, 2016 at 7:53 pm Reply	

    hello all i am work with multiuserchat but i am not able to send message on group please can any one describe briefly how to implement …
    Avatar	
    Muhammad Abubakar
    December 6, 2016 at 12:48 am Reply	

    hello thanks for this tutorial but i got this error
    i have downloaded openfire.
    12-05 21:43:22.980 2361-2361/com.blikoon.rooster D/RoosterService: Service Start() function called.
    12-05 21:43:24.810 2361-14139/com.blikoon.rooster I/System.out: Unparsed type SOA
    12-05 21:45:32.120 2361-14139/com.blikoon.rooster D/RoosterService: Something went wrong while connecting ,make sure the credentials are right and try again
    12-05 21:45:32.120 2361-14139/com.blikoon.rooster W/System.err: org.jivesoftware.smack.SmackException$ConnectionException: The following addresses failed: openfire.com:5222 Exception: failed to connect to openfire.com/209.15.13.134 (port 5222): connect failed: ETIMEDOUT (Connection timed out)
    12-05 21:45:32.120 2361-14139/com.blikoon.rooster W/System.err: at org.jivesoftware.smack.tcp.XMPPTCPConnection.connectUsingConfiguration(XMPPTCPConnection.java:564)
    12-05 21:45:32.120 2361-14139/com.blikoon.rooster W/System.err: at org.jivesoftware.smack.tcp.XMPPTCPConnection.connectInternal(XMPPTCPConnection.java:822)
    12-05 21:45:32.120 2361-14139/com.blikoon.rooster W/System.err: at org.jivesoftware.smack.AbstractXMPPConnection.connect(AbstractXMPPConnection.java:326)
    12-05 21:45:32.120 2361-14139/com.blikoon.rooster W/System.err: at com.blikoon.rooster.RoosterConnection.connect(RoosterConnection.java:86)
    12-05 21:45:32.120 2361-14139/com.blikoon.rooster W/System.err: at com.blikoon.rooster.RoosterConnectionService.initConnection(RoosterConnectionService.java:80)
    12-05 21:45:32.120 2361-14139/com.blikoon.rooster W/System.err: at com.blikoon.rooster.RoosterConnectionService.access$100(RoosterConnectionService.java:19)
    12-05 21:45:32.120 2361-14139/com.blikoon.rooster W/System.err: at com.blikoon.rooster.RoosterConnectionService$1.run(RoosterConnectionService.java:107)
    12-05 21:45:32.120 2361-14139/com.blikoon.rooster W/System.err: at java.lang.Thread.run(Thread.java:818)
    12-05 21:45:32.150 2361-2361/com.blikoon.rooster D/RoosterService: onDestroy()
    12-05 21:45:32.150 2361-2361/com.blikoon.rooster D/RoosterService: stop()
    12-05 21:45:32.150 2361-14139/com.blikoon.rooster D/RoosterConnection: Disconnecting from serser openfire.com
    12-05 21:45:32.150 2361-14139/com.blikoon.rooster W/System.err: org.jivesoftware.smack.SmackException$NotConnectedException: Client is not, or no longer, connected
    12-05 21:45:32.150 2361-14139/com.blikoon.rooster W/System.err: at org.jivesoftware.smack.AbstractXMPPConnection.disconnect(AbstractXMPPConnection.java:599)
    12-05 21:45:32.150 2361-14139/com.blikoon.rooster W/System.err: at org.jivesoftware.smack.AbstractXMPPConnection.disconnect(AbstractXMPPConnection.java:583)
    12-05 21:45:32.150 2361-14139/com.blikoon.rooster W/System.err: at com.blikoon.rooster.RoosterConnection.disconnect(RoosterConnection.java:170)
    12-05 21:45:32.150 2361-14139/com.blikoon.rooster W/System.err: at com.blikoon.rooster.RoosterConnectionService$2.run(RoosterConnectionService.java:130)
    12-05 21:45:32.150 2361-14139/com.blikoon.rooster W/System.err: at android.os.Handler.handleCallback(Handler.java:739)
    12-05 21:45:32.150 2361-14139/com.blikoon.rooster W/System.err: at android.os.Handler.dispatchMessage(Handler.java:95)
    12-05 21:45:32.150 2361-14139/com.blikoon.rooster W/System.err: at android.os.Looper.loop(Looper.java:135)
    12-05 21:45:32.150 2361-14139/com.blikoon.rooster W/System.err: at com.blikoon.rooster.RoosterConnectionService$1.run(RoosterConnectionService.java:109)
    12-05 21:45:32.150 2361-14139/com.blikoon.rooster W/System.err: at java.lang.Thread.run(Thread.java:818)

    tell me whats wrong with this?
    Avatar	
    yash
    December 9, 2016 at 3:52 am Reply	

    sir i have an instant messaging app running on tigase perfectly but how can i make this server open to public i.e upload it online
        gakwaya	
        gakwaya
        December 28, 2016 at 7:27 pm Reply	

        Hi.You probably need to install your server on a machine with a publicly accessible IP address or domain name.
    Avatar	
    shriya
    December 23, 2016 at 3:31 pm Reply	

    Hi
    Can you give github link for this tutorial..
    i have to implement.
        gakwaya	
        gakwaya
        December 28, 2016 at 7:23 pm Reply	

        https://github.com/gakwaya/Rooster
    Avatar	
    David
    December 26, 2016 at 3:58 pm Reply	

    Hello Daniel,

    Thanks for such an awesome Article…!
    I followed it and implemented small chatting module using Openfire.

    I need your help, on one more functionality, how to get one to one chat history on mobile from server to device?

    Currently I am using MamManager from Smack. But MamManager.isSupportedByServer() is returning false.
    I referred some links which tells that have to add Monitoring service on Openfire but still returning same I am using Openfire 4.1.0 & Smack 4.2.0-beta2.

    Thanks
        gakwaya	
        gakwaya
        December 28, 2016 at 7:22 pm Reply	

        Hello David,Thanks for the kind comment.
        Unfortunately I don’t use Openfire and I have no idea as to why retrieving Mam history is not working for you.If you join this muc “open_chat@conference.igniterealtime.org” you may get in touch with people that can help.The developpers of both smack and Openfire are
        pretty active in there and you might get a better response.Cheers!
    Avatar	
    Prem
    February 1, 2017 at 8:34 pm Reply	

    Hi Daniel,

    I’ve setup my own XMPP server in Ubuntu using .deb installer file. Now when I’m trying to connect to the XMPP server from my Android client using XMPPTCPConnection it is failing with SSLHandshakeException “Connection closed with error javax.net.ssl.SSLHandshakeException: java.security.cert.CertPathValidatorException: Trust anchor for certification path not found”.

    Could you throw some light on what’s going wrong here and how would I be able to fix the problem?

    Following is the error log,
    ****************************************
    02-01 15:55:22.626 32092-32177/com.aricent.chat.samplechatwithsmack D/SMACK: RECV (0):
    02-01 15:55:22.627 32092-32177/com.aricent.chat.samplechatwithsmack D/SMACK: RECV (0): PLAINDIGEST-MD5X-OAUTH2SCRAM-SHA-1
    02-01 15:55:22.628 32092-32176/com.aricent.chat.samplechatwithsmack D/SMACK: SENT (0):
    02-01 15:55:22.671 32092-32177/com.aricent.chat.samplechatwithsmack D/SMACK: RECV (0):
    02-01 15:55:22.762 32092-32177/com.aricent.chat.samplechatwithsmack W/AbstractXMPPConnection: Connection closed with error
    javax.net.ssl.SSLHandshakeException: java.security.cert.CertPathValidatorException: Trust anchor for certification path not found.
    at com.android.org.conscrypt.OpenSSLSocketImpl.startHandshake(OpenSSLSocketImpl.java:322)
    at com.android.org.conscrypt.OpenSSLSocketImpl.waitForHandshake(OpenSSLSocketImpl.java:623)
    at com.android.org.conscrypt.OpenSSLSocketImpl.getInputStream(OpenSSLSocketImpl.java:585)
    at org.jivesoftware.smack.tcp.XMPPTCPConnection.initReaderAndWriter(XMPPTCPConnection.java:658)
    at org.jivesoftware.smack.tcp.XMPPTCPConnection.proceedTLSReceived(XMPPTCPConnection.java:765)
    at org.jivesoftware.smack.tcp.XMPPTCPConnection.access$1000(XMPPTCPConnection.java:139)
    at org.jivesoftware.smack.tcp.XMPPTCPConnection$PacketReader.parsePackets(XMPPTCPConnection.java:1022)
    at org.jivesoftware.smack.tcp.XMPPTCPConnection$PacketReader.access$300(XMPPTCPConnection.java:956)
    at org.jivesoftware.smack.tcp.XMPPTCPConnection$PacketReader$1.run(XMPPTCPConnection.java:971)
    at java.lang.Thread.run(Thread.java:818)
    Caused by: java.security.cert.CertificateException: java.security.cert.CertPathValidatorException: Trust anchor for certification path not found.
    at com.android.org.conscrypt.TrustManagerImpl.checkTrusted(TrustManagerImpl.java:318)
    at com.android.org.conscrypt.TrustManagerImpl.checkServerTrusted(TrustManagerImpl.java:219)
    at com.android.org.conscrypt.Platform.checkServerTrusted(Platform.java:114)
    at com.android.org.conscrypt.OpenSSLSocketImpl.verifyCertificateChain(OpenSSLSocketImpl.java:550)
    at com.android.org.conscrypt.NativeCrypto.SSL_do_handshake(Native Method)
    at com.android.org.conscrypt.OpenSSLSocketImpl.startHandshake(OpenSSLSocketImpl.java:318)
    at com.android.org.conscrypt.OpenSSLSocketImpl.waitForHandshake(OpenSSLSocketImpl.java:623) 
    at com.android.org.conscrypt.OpenSSLSocketImpl.getInputStream(OpenSSLSocketImpl.java:585) 
    at org.jivesoftware.smack.tcp.XMPPTCPConnection.initReaderAndWriter(XMPPTCPConnection.java:658) 
    at org.jivesoftware.smack.tcp.XMPPTCPConnection.proceedTLSReceived(XMPPTCPConnection.java:765) 
    at org.jivesoftware.smack.tcp.XMPPTCPConnection.access$1000(XMPPTCPConnection.java:139) 
    at org.jivesoftware.smack.tcp.XMPPTCPConnection$PacketReader.parsePackets(XMPPTCPConnection.java:1022) 
    at org.jivesoftware.smack.tcp.XMPPTCPConnection$PacketReader.access$300(XMPPTCPConnection.java:956) 
    at org.jivesoftware.smack.tcp.XMPPTCPConnection$PacketReader$1.run(XMPPTCPConnection.java:971) 
    at java.lang.Thread.run(Thread.java:818) 
    Caused by: java.security.cert.CertPathValidatorException: Trust anchor for certification path not found.
    at com.android.org.conscrypt.TrustManagerImpl.checkTrusted(TrustManagerImpl.java:318) 
    at com.android.org.conscrypt.TrustManagerImpl.checkServerTrusted(TrustManagerImpl.java:219) 
    at com.android.org.conscrypt.Platform.checkServerTrusted(Platform.java:114) 
    at com.android.org.conscrypt.OpenSSLSocketImpl.verifyCertificateChain(OpenSSLSocketImpl.java:550) 
    at com.android.org.conscrypt.NativeCrypto.SSL_do_handshake(Native Method) 
    at com.android.org.conscrypt.OpenSSLSocketImpl.startHandshake(OpenSSLSocketImpl.java:318) 
    at com.android.org.conscrypt.OpenSSLSocketImpl.waitForHandshake(OpenSSLSocketImpl.java:623) 
    at com.android.org.conscrypt.OpenSSLSocketImpl.getInputStream(OpenSSLSocketImpl.java:585) 
    at org.jivesoftware.smack.tcp.XMPPTCPConnection.initReaderAndWriter(XMPPTCPConnection.java:658) 
    at org.jivesoftware.smack.tcp.XMPPTCPConnection.proceedTLSReceived(XMPPTCPConnection.java:765) 
    at org.jivesoftware.smack.tcp.XMPPTCPConnection.access$1000(XMPPTCPConnection.java:139) 
    at org.jivesoftware.smack.tcp.XMPPTCPConnection$PacketReader.parsePackets(XMPPTCPConnection.java:1022) 
    at org.jivesoftware.smack.tcp.XMPPTCPConnection$PacketReader.access$300(XMPPTCPConnection.java:956) 
    at org.jivesoftware.smack.tcp.XMPPTCPConnection$PacketReader$1.run(XMPPTCPConnection.java:971) 
    at java.lang.Thread.run(Thread.java:818) 
    02-01 15:55:22.763 32092-32177/com.aricent.chat.samplechatwithsmack D/xmpp: ConnectionClosedOn Error!
    02-01 15:55:22.764 32092-32166/com.aricent.chat.samplechatwithsmack E/(Service): SMACKException: javax.net.ssl.SSLHandshakeException: java.security.cert.CertPathValidatorException: Trust anchor for certification path not found.
    ****************************************

    Thanks,
    Prem
        gakwaya	
        gakwaya
        February 2, 2017 at 9:42 am Reply	

        Hi,
        You probably need to configure ssl certificates for your server to be able to connect with tls.Or you can disable encrypted connections in your client.But disabling encrypted connections is very dangerous you should do this ONLY in test environements.
        Good luck.
            Avatar	
            Nurlan
            February 2, 2017 at 9:09 pm Reply	

            Hi! How can i disable encrypted connections?
                gakwaya	
                gakwaya
                February 3, 2017 at 8:35 am Reply	

                Something like this:
                xmppConfig.setSecurityMode(ConnectionConfiguration.SecurityMode.disabled);
                in your connection routine.This is off the top my head and I haven’t used smack for a while.You should check the official API just to play it safe.
                Good luck.
    Avatar	
    emmanuel ogbonna
    February 10, 2017 at 6:17 am Reply	

    @Aloy make use of twitter digits for mobile number authentication is free.
    Avatar	
    emmanuel ogbonna
    February 10, 2017 at 6:18 am Reply	

    Hello please I can’t login into the app nothing is happening please help
        gakwaya	
        gakwaya
        February 18, 2017 at 4:31 pm Reply	

        Is Rooster saying something in the debug output?
    Avatar	
    saurabh
    February 15, 2017 at 2:02 am Reply	

    Dear Daniel !! Very nice of you for such a detailed tutorial and keep up the work !!
    This tutorial really serves good knowledge for understanding XMPP chat.

    But I am stuck on a issue and it is also reported by other as I can see in the comments too, I think you are not able to reproduce it, please help us out. I followed the following steps:

    1. After downloading and successfully building your project from Github,

    2. Created two test users abc@xmpp.jp and xyz@xmpp.jp.

    3. Now in order to make them communicate with each other, I have to create two different logins i.e. one from abc@xmpp.jp and other from xyz@xmpp.jp and add vice versa to contact list of each other.

    4. So, to do this I modified the contactModel class and added contact abc@xmpp.jp and logged in from credentials of xyz@xmpp.jp.

    private void populateWithInitialContacts(Context context)
    {
    //Create the Foods and add them to the list;

    Contact contact1 = new Contact(“abc@xmpp.jp”);
    mContacts.add(contact1);
    }

    5. And to prepare other client I added contact xyz@xmpp.jp and logged in from credentials of abc@xmpp.jp.

    private void populateWithInitialContacts(Context context)
    {
    //Create the Foods and add them to the list;

    Contact contact1 = new Contact(“xyz@xmpp.jp”);
    mContacts.add(contact1);
    }

    6. Now,I have two different devices ready to communicate with each other such that I have abc@xmpp.jp in contact list of xyz@xmpp.jp and vice versa.

    7. But, when I select the contact to send the message there is no message received at the other end. However, In a case where sender and receiver is same message is received i.e. login from abc@xmpp.jp and adding abc@xmmpp.jp to contact list.

    I am helpless because I have invested much time in this tutorial and this tutorial is just what I need to start with. I would request some of your time to this issue, only then it will solve your purpose and benefit learners like us.

    Thanks in advance.
        gakwaya	
        gakwaya
        February 18, 2017 at 4:30 pm Reply	

        Please have a look at the fix I applied at the end of the tutorial.
    Avatar	
    saurabh
    February 15, 2017 at 2:05 am Reply	

    helllo
    Avatar	
    saurabh sirohi
    February 15, 2017 at 2:27 am Reply	

    Dear Daniel !! Very nice of you for such a detailed tutorial and keep up the work !!
    This tutorial really serves good knowledge for understanding XMPP chat.

    But I am stuck on a issue and it is also reported by other as I can see in the comments too, I think you are not able to reproduce it, please help us out. I followed the following steps:

    1. After downloading and successfully building your project from Github,

    2. Created two test users abc@xmpp.jp and xyz@xmpp.jp.

    3. Now in order to make them communicate with each other, I have to create two different logins i.e. one from abc@xmpp.jp and other from xyz@xmpp.jp and add vice versa to contact list of each other.
        gakwaya	
        gakwaya
        February 18, 2017 at 4:30 pm Reply	

        Please have a look at the fix I applied at the end of the tutorial.
    Avatar	
    saurabh sirohi
    February 15, 2017 at 2:28 am Reply	

    why am i not able to see my comments !!!
        gakwaya	
        gakwaya
        February 18, 2017 at 4:29 pm Reply	

        When post a message it has to be approved by the moderator to bee seen here.Otherwise we get many garbage spamy messages.I do my best to check as often as possible.
    Avatar	
    heritier
    February 17, 2017 at 10:01 pm Reply	

    I am not able to get :
    compile ‘co.devcenter.square:android-ui-library:0.1’

    please help me
        gakwaya	
        gakwaya
        February 18, 2017 at 4:27 pm Reply	

        What exact problem are you facing?Is Android Studio giving you some error message?
            Avatar	
            heritier
            February 21, 2017 at 3:24 am Reply	

            Hello, thank you very much for your answer; But I have already solved the problem; I had to connect to the internet and everything was resolved
    gakwaya	
    gakwaya
    February 18, 2017 at 4:26 pm Reply	

    I finally had some free time to look into the problem of messages not being received by Rooster and applied a fix.The tests I have conducted have shown that I can send and receive messages to/from Rooster and when Rooster is talking to another xmpp client.I used Conversations for this tests.The server I have used is Prosody 0.10.Hope this fixes the problem for you guys.
    The details of the fix can be found in this git commit :
    https://github.com/blikoon/Rooster/commit/cd00718b3c1abf38483bfe65f5ad1a378035ca45#diff-ec8e92025fb2cd50d04c2fc3f23f49f7
        Avatar	
        Jason
        February 21, 2017 at 12:10 pm Reply	

        Thank you So much Daniel for your time !! lets hope it works all the way good this time.
    Avatar	
    heritier
    February 21, 2017 at 3:33 am Reply	

    It is not in this tuto; But I will ask you to help me; I would like that when sending a message while ChatActivity is not visible, it indicates a notification; Once you click on the notification it opens ChatActivity

    Please help me
    Thank you
    Avatar	
    heritier
    February 21, 2017 at 3:43 am Reply	

    It is not in this tuto; But I will ask you to help me; I would like that when receive a message while ChatActivity is not visible, it indicates a notification; Once you click on the notification it opens ChatActivity

    Please help me
    Thank you
        gakwaya	
        gakwaya
        February 21, 2017 at 5:26 pm Reply	

        Hi,
        Please have a look here : https://developer.android.com/guide/topics/ui/notifiers/notifications.html
    Avatar	
    Jason
    February 21, 2017 at 12:43 pm Reply	

    One question Daniel !!

    I am working on a chat app, yes a multi user chat having features alike whatsapp such as message notifications while app in background, user typing…, message sent-uploaded-seen by user, and many more as u know.. sending a voice/video recorded message and file transfer in a later phase..!!

    This tutorial seems very nice start for me.. does this project can be applied with these features ? Or the requirements needs some other way of implementation ? I know Rooster is using activities rather a tabbed activity and fragments but thats UI and its not as much concern.

    Thanks.
        gakwaya	
        gakwaya
        February 21, 2017 at 5:35 pm Reply	

        Hi Jason,
        Yes Rooster can be a basis for your project but it really isn’t doing anything fancy.Smack is doing all the heavy lifting.If you want to extend it, you should read smack docs an you should be able to do most of what you want.

        However,there are many open source projects that have used smack to implement most if not all the cool features you want. Kontalk and Xabber come to mind. If you don’t mind basing your work on top of them ,they really are worth a shot.
        Good luck.
            Avatar	
            Jason
            February 22, 2017 at 12:13 pm Reply	

            Yes, Thanks Daniel !! I am seeing Xabber and conversations for reference.. a lot of code there mostly complex and will definitely need certain a level of programming to understand it. Kontalk I will just check it out.

            Thanks again for your fixes, app works like a charm !!!
                gakwaya	
                gakwaya
                February 22, 2017 at 8:38 pm Reply	

                Glad it’s working for you Jason.
                    Avatar	
                    Jason
                    February 27, 2017 at 8:54 am

                    Hi again !! Daniel, can you suggest some open sources, something to take this a level up !! Open Projects like conversations and xabber, am facing a hard time with them..

                    thanks,
                    gakwaya	
                    gakwaya
                    February 28, 2017 at 10:52 am

                    Hey, most of these projects have years of development so they are understandably complex. I would suggest first laying down what you want to do in a list and take them down one by one. If you face any problem ask in the the smack/openfire MUC .If you are specific in what problem you are having ,somebody is usually happy to give you a helping hand.
    Avatar	
    emmanuel ogbonna
    February 22, 2017 at 4:26 am Reply	

    Hello please help the app is not working can you please send me your email I need your help
        gakwaya	
        gakwaya
        February 22, 2017 at 12:33 pm Reply	

        Hi,
        please ask your question here.That way others can also benefit.
    Avatar	
    Bittu Bushera
    February 22, 2017 at 6:21 pm Reply	

    Hi dear,
    it is very helpful for me as i am fresher in chat apps . But it just showing the messages on one device(on same device) which are sent by us. it is not showing recieved message . plz discuss how we can send and receive message on each other device. Even i tried with two devices with different different user with same contact as ‘user@testServer.com’
    pls help me and also provide your gmail id also for future help.
        gakwaya	
        gakwaya
        February 22, 2017 at 8:20 pm Reply	

        Hi,
        I am not sure I am getting your problem right, Rooster is installed on device A and B ,A can send to B and B can receive but B can send to A and A is not receiving?Also please post relevant debug output here so we stand a chance at seeing what is going wrong.Also the android versions your devices are running.
            Avatar	
            Bittu Bushera
            February 23, 2017 at 1:20 pm Reply	

            I have installed the Rooster application on two devices (device A and device B). On both devices i have tried with same email password as well as different email password also. On both devices, I have tried with same contact as ‘user@testServer.com’ as well as different contact also. But A just showing sent message but that message is not received by any device (not also device B). As it B just showing sent message but that message is not received by anyone. Means both devices just sending the message but not receiving the message. I trying with device A (Model :- M! 4W and version:- 6.0.1 MMB29M) and device B (Model :- vivo Y51L and version:- 5.0.2)
            Please suggest to me about this that how i can communicate with each other.
                Avatar	
                Jason
                February 23, 2017 at 2:15 pm Reply	

                Dont use vivo for testing !!! I had a bad time with it..!! vivo sucks with broadcast receivers, and also kills you app(main process and its service) as soon as you move out app from the recent app tray.

                Wasted my time and money on vivo.
    Avatar	
    Geekers Employee
    February 22, 2017 at 6:31 pm Reply	

    Hi
    I am trying this demo with two different-different device with different-different user ids.
    I am logging successfully but i can only send the message on both side ,not recieving the message or each other.
    Plz suggest me about it.
    Avatar	
    Jason
    February 23, 2017 at 2:36 pm Reply	

    Guys..!! Message not received on other device issue is -SOLVED.

    Regarding the issue about receiving the messages on other device..

    First read the code.. let yourself know the process.

    inside chatActivity

    @override
    onResume()
    onBroadcastRecieve

    1 if ( from.equals(contactJid))
    2 {
    3 mChatView.receiveMessage(body);
    4
    5 }else
    6 {
    7 Log.d(TAG,”Got a message from jid :”+from+”msg is: “+body);

    1. Line 1 : The code says it will only display a message in chatActivity if user and sender are same.

    2. Line 7: It does nothing just displays the message in log and You have to implement that part.

    Just try to send a message with sender and receiver as same.. check the debug logs !! U will see the message in chatActivity and in logs too.
    Avatar	
    emmanuel ogbonna
    February 23, 2017 at 8:23 pm Reply	

    Hello I installed the app but can’t sign in what do I do or what exactly am I suppose to input as the username and password. Help please
        gakwaya	
        gakwaya
        February 24, 2017 at 2:57 am Reply	

        You need an account to some xmpp server. May be some introduction on xmpp would help :
        http://www.blikoon.com/xmpp/xmpp-a-soft-friendly-introduction
    Avatar	
    Vivek Singh Shekhawat
    March 14, 2017 at 12:52 pm Reply	

    Hello @gakwaya …
    Thank you for the great tutorial for the xmpp beginners..i followed your tutorial and everything running smooth and now am trying to fetch the typing or composing status using your tutorial code…can you please share some knowledge about how to fetch typing or composing status using this tutorial…

    Thank You in Advance….
        gakwaya	
        gakwaya
        March 19, 2017 at 2:15 pm Reply	

        Hey,
        There is an Xmpp Extension Protocol ( XEP) for what you want to do.
        https://xmpp.org/extensions/xep-0085.html
        I haven’t used that with smack but a quick search came up with this link
        http://stackoverflow.com/questions/18436546/how-to-know-typing-status-in-xmpp-openfire-using-smack
        that may be a good start.
        Good luck.
    Avatar	
    Satya
    March 22, 2017 at 11:29 am Reply	

    hi .
    this is an awesome tutorial.
    I m using Android Studio 2.3 and installed latest ejabberd. refactored the code to match to latest dependencies.

    dependencies {
    compile fileTree(include: [‘*.jar’], dir: ‘libs’)
    compile ‘com.android.support:appcompat-v7:25.3.0’
    compile ‘com.android.support:design:25.3.0’
    compile ‘co.devcenter.square:android-ui-library:0.1’
    compile ‘org.igniterealtime.smack:smack-android-extensions:4.1.0-alpha6’
    compile ‘org.igniterealtime.smack:smack-tcp:4.1.0-alpha6’
    testCompile ‘junit:junit:4.12’
    }

    When the android emulator loads with login UI, I am unable to login.
    Log message:
    Connected to the target VM, address: ‘localhost:8630’, transport: ‘socket’, there is no exception
        Avatar	
        Satya
        March 22, 2017 at 11:34 am Reply	

        after some time I got the error org.jivesoftware.smack.SmackException$ConnectionException: The following addresses failed: localhost:5222 Exception: Connection refused

        I checked ejabberd, by executing following command

        sbin/ejabberdctl status
        The node ejabberd@localhost is started with status: started
        ejabberd 17.03.beta-63 is running in that node
            Avatar	
            Satya
            March 22, 2017 at 1:36 pm Reply	

            I resolved this, by adding

            builder.setHost(“10.0.2.2”);
            builder.setPort(5222);
            builder.setSecurityMode(ConnectionConfiguration.SecurityMode.disabled);
        gakwaya	
        gakwaya
        March 23, 2017 at 2:41 am Reply	

        Glad it’s working for you now 🙂
    Avatar	
    Jason
    March 24, 2017 at 8:46 am Reply	

    Hi daniel !! just to inform you..

    I am working on your project and has added some features..
    like autologin.. (was already there but a bit improved now)
    add and delete contact..
    receive and update contacts on contactsActivity.java..
    receive and update respective contact item on new message received..
    corresponding time the message received and number of messages received for a particular contact item view… !!
    Created a database for entering chat messages for current connection session..!

    In progress..
    display picture..
    message handling to be implemented in background..
    typing.. (state when other user is typing message)
    online/offline status..
    and further..!!!

    Still struggling with XMPPconnection stability and re-connection mechanism..!

    If any suggestion.. may kindly reply..!

    P.S: If you want we can update the current GitHub project and make rooster grow ! Thanks.
        gakwaya	
        gakwaya
        March 26, 2017 at 4:12 am Reply	

        Hello Jason,
        Sounds interesting what you’re doing there. But I am more interested in explaining the process of building that so it is easy to understand for readers. Simply merging in new code would confuse people coming from this tutorial.
        If possible, would you like to document what you have done into a tutorial and post it somewhere? I would happily link to your work for people who want to take this further. That would actually have a huge audience as I get lots of emails requesting to take this to the next step but I currently I am short on time to do so.
        Let me know what you think.
            Avatar	
            Jason
            March 28, 2017 at 12:33 pm Reply	

            That sounds great !! This is exactly what I want.

            That will a lot of learners as I am one and I remember when your tutorial gave me a good start for my project. I would definitely like take it further.

            The code is pretty simple as yours. Commented and Logged pretty well.

            Since then, I have implemented receiving messages in background and notifications for messages as well.

            I will get back to you soon when I am done with my basic IM chat application and will work as you desired.
            Avatar	
            soanm
            June 27, 2017 at 9:30 am Reply	

            Hello Jason ,

            I want to implement typing notification. can you plz help me how to implement this. there is no listener for this.
    Avatar	
    Vishal Kaushal
    March 28, 2017 at 7:16 pm Reply	

    Please tell me how can i customize chat view with images and custom backgrounds on chat view messages
        gakwaya	
        gakwaya
        March 29, 2017 at 3:08 am Reply	

        The chatView is powered by a third party library in Rooster. May be they provide an interface to change backgrounds.You should check that. But if you do your own chatView, you can easily apply a background to the the root layout containing the RecyclerView. Of course that is if you use the RecyclerView. Hope this helps.
            Avatar	
            Vishal Kaushal
            March 29, 2017 at 6:24 pm Reply	

            Thanks
    Avatar	
    Midhun
    April 12, 2017 at 9:12 pm Reply	

    Hii…. Its an awesome tuturial for xmpp android chat. I realy likes it.
    So i have few doubts, in my application am using smack as per the tutorial. It was fine for text messages. Bu now i need to transfer file through it but i cant customize the smack xml atributes. In my other end ios sending xml with a attach ment tag but my end smack skippes those tags.. So please suggest a solution for this.
    Thanks in adavance
    gakwaya	
    gakwaya
    April 23, 2017 at 10:55 am Reply	

    Hi,
    Xmpp has some XEPs that handle different scenarios of sending files two come to mind:
    Jingle file transfer (https://xmpp.org/extensions/xep-0234.html) and http file upload (https://xmpp.org/extensions/xep-0363.html) .You might want to check if Smack supports these (haven’t checked myself) and if it does do look at relevant docs.(There might be some other ways though,so my small list above isn’t exhaustive)
    Good luck.
    Avatar	
    Saurabh
    May 3, 2017 at 4:24 pm Reply	

    Hi Gakwaya,
    Thanks for the awesome tutorial!

    I am getting the following error when i send the message from chat activity.
    Please help!

    E/AndroidRuntime: FATAL EXCEPTION: main
    Process: com.example.networktasksdemo, PID: 6186
    java.lang.RuntimeException: Error receiving broadcast Intent { act=com.example.sendmessage flg=0x10 (has extras) } in com.example.networktasksdemo.MyXMPPConnection$4@584d035
    at android.app.LoadedApk$ReceiverDispatcher$Args.run(LoadedApk.java:893)
    at android.os.Handler.handleCallback(Handler.java:739)
    at android.os.Handler.dispatchMessage(Handler.java:95)
    at android.os.Looper.loop(Looper.java:148)
    at android.app.ActivityThread.main(ActivityThread.java:5441)
    at java.lang.reflect.Method.invoke(Native Method)
    at com.android.internal.os.ZygoteInit$MethodAndArgsCaller.run(ZygoteInit.java:738)
    at com.android.internal.os.ZygoteInit.main(ZygoteInit.java:628)
    Caused by: java.lang.NullPointerException
    at java.util.concurrent.ConcurrentHashMap.putVal(ConcurrentHashMap.java:847)
    at java.util.concurrent.ConcurrentHashMap.put(ConcurrentHashMap.java:842)
    at org.jivesoftware.smack.chat.ChatManager.createChat(ChatManager.java:251)
    at org.jivesoftware.smack.chat.ChatManager.createChat(ChatManager.java:243)
    at org.jivesoftware.smack.chat.ChatManager.createChat(ChatManager.java:224)
    at com.example.networktasksdemo.MyXMPPConnection.sendMessage(MyXMPPConnection.java:221)
    at com.example.networktasksdemo.MyXMPPConnection.access$200(MyXMPPConnection.java:32)
    at com.example.networktasksdemo.MyXMPPConnection$4.onReceive(MyXMPPConnection.java:201)
    at android.app.LoadedApk$ReceiverDispatcher$Args.run(LoadedApk.java:883)
    at android.os.Handler.handleCallback(Handler.java:739) 
    at android.os.Handler.dispatchMessage(Handler.java:95) 
    at android.os.Looper.loop(Looper.java:148) 
    at android.app.ActivityThread.main(ActivityThread.java:5441) 
    at java.lang.reflect.Method.invoke(Native Method) 
    at com.android.internal.os.ZygoteInit$MethodAndArgsCaller.run(ZygoteInit.java:738) 
    at com.android.internal.os.ZygoteInit.main(ZygoteInit.java:628) 
    I/Process: Sending signal. PID: 6186 SIG: 9
    Application terminated.
        gakwaya	
        gakwaya
        May 6, 2017 at 2:30 am Reply	

        Hi,
        are you seeing the same error when compile the code from github as is? Can’t really know what is going wrong for you without some code context.
    Avatar	
    sb
    June 6, 2017 at 3:54 pm Reply	

    Hi,
    This is Great tutorial. thank you.
    for developing it, i need your help.
    I want to receive pushnotification in device when the special jid send broadcast message .so i try it :

    in the RoosterConnection.java :

    messageListener = new ChatMessageListener() {
    @Override
    public void processMessage(Chat chat, Message message) {
    ///ADDED
    Log.d(TAG,”message.getBody() :”+message.getBody());
    Log.d(TAG,”message.getFrom() :”+message.getFrom());

    String from = message.getFrom();
    String contactJid=””;

    if ( from.contains(“/”))
    {
    contactJid = from.split(“/”)[0];
    Log.d(TAG,”The real jid is :” +contactJid);
    }else
    {
    contactJid=from;
    if ( contactJid.equals(“specialJid”)) // this is my code
    {
    Log.d(TAG,”deliver:”+contactJid);
    RecieveServerMsg(contactJid ,matn );

    } //ending
    }

    //Bundle up the intent and send the broadcast.
    Intent intent = new Intent(RoosterConnectionService.NEW_MESSAGE);
    intent.setPackage(mApplicationContext.getPackageName());
    intent.putExtra(RoosterConnectionService.BUNDLE_FROM_JID,contactJid);
    intent.putExtra(RoosterConnectionService.BUNDLE_MESSAGE_BODY,message.getBody());
    mApplicationContext.sendBroadcast(intent);
    Log.d(TAG,”Received message from :”+contactJid+” broadcast sent.”);
    ///ADDED

    }

    };

    —————
    and next add this function:
    public void RecieveServerMsg(String from , String msg) {
    Log.d(TAG,”roosterRecivam :1″);
    NotificationView inst = new NotificationView();
    inst.addNotification();
    }
    //
    ———–
    and next after create customnotif.xml , create NotificationView class:

    import android.app.Activity;
    import android.app.NotificationManager;
    import android.app.PendingIntent;
    import android.content.Context;
    import android.content.Intent;
    import android.os.Bundle;
    import android.support.v4.app.NotificationCompat;

    /**
    * Created by sabina on 6/5/2017.
    */
    public class NotificationView extends Activity {
    int id = 1;

    @Override
    public void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.custom_notif);
    }

    public void addNotification() {

    NotificationCompat.Builder builder =
    new NotificationCompat.Builder(this)
    .setSmallIcon(R.drawable.icon)
    .setContentTitle(“Notifications Example”)
    .setContentText(“This is a test notification”);

    Intent notificationIntent = new Intent(this, NotificationView.class);
    PendingIntent contentIntent = PendingIntent.getActivity(this, 0, notificationIntent,
    PendingIntent.FLAG_UPDATE_CURRENT);
    builder.setContentIntent(contentIntent);

    // Add as notification
    NotificationManager manager = (NotificationManager) getSystemService(Context.NOTIFICATION_SERVICE);
    manager.notify(id, builder.build());
    }
    }
    ————————————–
    and in manifestAndroid:

    ==========================================
    but after compile it , i get this error:
    06-06 13:28:03.204 2364-3752/com.blikoon.rooster E/AbstractXMPPConnection: Exception in packet listener
    java.lang.RuntimeException: Can’t create handler inside thread that has not called Looper.prepare()
    at android.os.Handler.(Handler.java:200)
    at android.os.Handler.(Handler.java:114)
    at android.app.Activity.(Activity.java:754)
    at com.blikoon.rooster.NotificationView.(NotificationView.java:0)
    at com.blikoon.rooster.RoosterConnection.RecieveServerMsg(RoosterConnection.java:164)
    at com.blikoon.rooster.RoosterConnection$1.processMessage(RoosterConnection.java:122)
    at org.jivesoftware.smack.Chat.deliver(Chat.java:177)
    at org.jivesoftware.smack.ChatManager.deliverMessage(ChatManager.java:336)
    at org.jivesoftware.smack.ChatManager.access$300(ChatManager.java:48)
    at org.jivesoftware.smack.ChatManager$2.processPacket(ChatManager.java:157)
    at org.jivesoftware.smack.AbstractXMPPConnection.invokePacketCollectorsAndNotifyRecvListeners(AbstractXMPPConnection.java:880)
    at org.jivesoftware.smack.AbstractXMPPConnection$ListenerNotification.run(AbstractXMPPConnection.java:851)
    at java.util.concurrent.Executors$RunnableAdapter.call(Executors.java:423)
    at java.util.concurrent.FutureTask.run(FutureTask.java:237)
    at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1113)
    at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:588)
    at java.lang.Thread.run(Thread.java:818)

    ==============================
    Avatar	
    Narendra K
    July 3, 2017 at 8:16 am Reply	

    I am getting below error…how can i resolve it ?

    07-03 11:07:39.942 31514-31514/com.blikoon.rooster D/RoosterService: onCreate()
    07-03 11:07:39.943 31514-31514/com.blikoon.rooster D/RoosterService: onStartCommand()
    07-03 11:07:39.943 31514-31514/com.blikoon.rooster D/RoosterService: Service Start() function called.
    07-03 11:07:39.945 31514-31830/com.blikoon.rooster D/RoosterService: initConnection()
    07-03 11:07:39.951 31514-31830/com.blikoon.rooster D/RoosterConnection: RoosterConnection Constructor called.
    07-03 11:07:39.951 31514-31830/com.blikoon.rooster D/RoosterConnection: Connecting to server xmpp.jp
    07-03 11:07:40.467 31514-31840/com.blikoon.rooster W/AbstractXMPPConnection: Connection closed with error
    java.net.SocketException: Connection reset
    at java.net.SocketInputStream.read(SocketInputStream.java:192)
    at java.net.SocketInputStream.read(SocketInputStream.java:120)
    at sun.nio.cs.StreamDecoder.readBytes(StreamDecoder.java:287)
    at sun.nio.cs.StreamDecoder.implRead(StreamDecoder.java:350)
    at sun.nio.cs.StreamDecoder.read(StreamDecoder.java:179)
    at java.io.InputStreamReader.read(InputStreamReader.java:184)
    at java.io.BufferedReader.read1(BufferedReader.java:221)
    at java.io.BufferedReader.read(BufferedReader.java:297)
    at org.kxml2.io.KXmlParser.fillBuffer(KXmlParser.java:1535)
    at org.kxml2.io.KXmlParser.peekType(KXmlParser.java:1007)
    at org.kxml2.io.KXmlParser.next(KXmlParser.java:357)
    at org.kxml2.io.KXmlParser.next(KXmlParser.java:321)
    at org.jivesoftware.smack.tcp.XMPPTCPConnection$PacketReader.parsePackets(XMPPTCPConnection.java:1170)
    at org.jivesoftware.smack.tcp.XMPPTCPConnection$PacketReader.access$200(XMPPTCPConnection.java:931)
    at org.jivesoftware.smack.tcp.XMPPTCPConnection$PacketReader$1.run(XMPPTCPConnection.java:950)
    07-03 11:07:40.468 31514-31840/com.blikoon.rooster D/RoosterConnection: ConnectionClosedOnError, error java.net.SocketException: Connection reset
    07-03 11:07:45.450 31514-31830/com.blikoon.rooster D/RoosterService: Something went wrong while connecting ,make sure the credentials are right and try again
    07-03 11:07:45.450 31514-31830/com.blikoon.rooster W/System.err: org.jivesoftware.smack.SmackException$NoResponseException: No response received within packet reply timeout. Timeout was 5000ms (~5s)
    07-03 11:07:45.450 31514-31830/com.blikoon.rooster W/System.err: at org.jivesoftware.smack.SynchronizationPoint.checkForResponse(SynchronizationPoint.java:176)
    07-03 11:07:45.450 31514-31830/com.blikoon.rooster W/System.err: at org.jivesoftware.smack.SynchronizationPoint.checkIfSuccessOrWait(SynchronizationPoint.java:110)
    07-03 11:07:45.450 31514-31830/com.blikoon.rooster W/System.err: at org.jivesoftware.smack.SynchronizationPoint.checkIfSuccessOrWaitOrThrow(SynchronizationPoint.java:93)
    07-03 11:07:45.450 31514-31830/com.blikoon.rooster W/System.err: at org.jivesoftware.smack.tcp.XMPPTCPConnection.connectInternal(XMPPTCPConnection.java:825)
    07-03 11:07:45.450 31514-31830/com.blikoon.rooster W/System.err: at org.jivesoftware.smack.AbstractXMPPConnection.connect(AbstractXMPPConnection.java:326)
    07-03 11:07:45.451 31514-31830/com.blikoon.rooster W/System.err: at com.blikoon.rooster.RoosterConnection.connect(RoosterConnection.java:88)
    07-03 11:07:45.451 31514-31830/com.blikoon.rooster W/System.err: at com.blikoon.rooster.RoosterConnectionService.initConnection(RoosterConnectionService.java:80)
    07-03 11:07:45.451 31514-31830/com.blikoon.rooster W/System.err: at com.blikoon.rooster.RoosterConnectionService.access$100(RoosterConnectionService.java:19)
    07-03 11:07:45.451 31514-31830/com.blikoon.rooster W/System.err: at com.blikoon.rooster.RoosterConnectionService$1.run(RoosterConnectionService.java:107)
    07-03 11:07:45.451 31514-31830/com.blikoon.rooster W/System.err: at java.lang.Thread.run(Thread.java:762)
    07-03 11:07:45.452 31514-31514/com.blikoon.rooster D/RoosterService: onDestroy()
    07-03 11:07:45.452 31514-31514/com.blikoon.rooster D/RoosterService: stop()
    07-03 11:07:45.453 31514-31830/com.blikoon.rooster D/RoosterConnection: Disconnecting from serser xmpp.jp
    07-03 11:07:45.455 31514-31830/com.blikoon.rooster W/System.err: org.jivesoftware.smack.SmackException$NotConnectedException: Client is not, or no longer, connected
    07-03 11:07:45.456 31514-31830/com.blikoon.rooster W/System.err: at org.jivesoftware.smack.AbstractXMPPConnection.disconnect(AbstractXMPPConnection.java:599)
    07-03 11:07:45.456 31514-31830/com.blikoon.rooster W/System.err: at org.jivesoftware.smack.AbstractXMPPConnection.disconnect(AbstractXMPPConnection.java:583)
    07-03 11:07:45.456 31514-31830/com.blikoon.rooster W/System.err: at com.blikoon.rooster.RoosterConnection.disconnect(RoosterConnection.java:184)
    07-03 11:07:45.456 31514-31830/com.blikoon.rooster W/System.err: at com.blikoon.rooster.RoosterConnectionService$2.run(RoosterConnectionService.java:130)
    07-03 11:07:45.456 31514-31830/com.blikoon.rooster W/System.err: at android.os.Handler.handleCallback(Handler.java:751)
    07-03 11:07:45.456 31514-31830/com.blikoon.rooster W/System.err: at android.os.Handler.dispatchMessage(Handler.java:95)
    07-03 11:07:45.456 31514-31830/com.blikoon.rooster W/System.err: at android.os.Looper.loop(Looper.java:154)
    07-03 11:07:45.456 31514-31830/com.blikoon.rooster W/System.err: at com.blikoon.rooster.RoosterConnectionService$1.run(RoosterConnectionService.java:109)
    07-03 11:07:45.456 31514-31830/com.blikoon.rooster W/System.err: at java.lang.Thread.run(Thread.java:762)
    07-03 11:09:39.842 31514-31514/com.blikoon.rooster V/InputMethodManager: Starting input: tba=android.view.inputmethod.EditorInfo@f2f0bcd nm : com.blikoon.rooster ic=com.android.internal.widget.EditableInputConnection@a2eac82
    07-03 11:09:39.843 31514-31514/com.blikoon.rooster I/InputMethodManager: [IMM] startInputInner – mService.startInputOrWindowGainedFocus
    07-03 11:09:39.848 31514-31514/com.blikoon.rooster D/InputTransport: Input channel constructed: fd=102
    07-03 11:09:39.848 31514-31514/com.blikoon.rooster D/InputTransport: Input channel destroyed: fd=84
    07-03 11:09:39.852 31514-31514/com.blikoon.rooster W/IInputConnectionWrapper: finishComposingText on inactive InputConnection
    07-03 11:09:39.959 31514-31514/com.blikoon.rooster W/IInputConnectionWrapper: getCursorCapsMode on inactive InputConnection
    07-03 11:09:39.997 31514-31514/com.blikoon.rooster W/IInputConnectionWrapper: getCursorCapsMode on inactive InputConnection
    07-03 11:09:40.071 31514-31514/com.blikoon.rooster W/IInputConnectionWrapper: getSelectedText on inactive InputConnection
    07-03 11:09:40.073 31514-31514/com.blikoon.rooster W/IInputConnectionWrapper: getTextBeforeCursor on inactive InputConnection
    07-03 11:09:40.075 31514-31514/com.blikoon.rooster W/IInputConnectionWrapper: getTextAfterCursor on inactive InputConnection
    07-03 11:09:40.100 31514-31514/com.blikoon.rooster W/IInputConnectionWrapper: getSelectedText on inactive InputConnection
    07-03 11:09:40.102 31514-31514/com.blikoon.rooster W/IInputConnectionWrapper: getTextBeforeCursor on inactive InputConnection
    07-03 11:09:40.104 31514-31514/com.blikoon.rooster W/IInputConnectionWrapper: getTextAfterCursor on inactive InputConnection
    07-03 11:09:45.292 31514-31514/com.blikoon.rooster D/ViewRootImpl@66c51fe[LoginActivity]: MSG_RESIZED_REPORT: frame=Rect(0, 0 – 1536, 2048) ci=Rect(0, 48 – 0, 0) vi=Rect(0, 48 – 0, 0) or=1
    07-03 11:09:45.322 31514-31514/com.blikoon.rooster D/ViewRootImpl@66c51fe[LoginActivity]: Relayout returned: oldFrame=[0,0][1536,2048] newFrame=[0,0][1536,2048] result=0x3 surface={isValid=true 547675805696} surfaceGenerationChanged=false
    07-03 11:09:45.326 31514-31514/com.blikoon.rooster D/ScrollView: onsize change changed
    07-03 11:09:45.339 31514-31514/com.blikoon.rooster D/ViewRootImpl@66c51fe[LoginActivity]: MSG_WINDOW_FOCUS_CHANGED 0
        gakwaya	
        gakwaya
        October 31, 2017 at 4:56 am Reply	

        From : 07-03 11:07:40.467 31514-31840/com.blikoon.rooster W/AbstractXMPPConnection: Connection closed with error
        java.net.SocketException: Connection reset, it looks like the server is closing the connection for some reason. If you are sure the credentials you are using are all right, I would try with another client and/or other server. From the results I would be able to pin out what is wrong. Also do you notice the same problem by compiling the code from github? I can’t diagnose the problem without looking at your code. Hope this helps.
    Avatar	
    Karl
    July 6, 2017 at 1:41 am Reply	

    Hi! I would like to ask if this is possible for a database (will be getting from http rest api) with a website that does not have xmpp? How would I know if it has xmpp? I am sorry, I am so confused. I’m an intern who was told to create an app based on the website another intern created.

    Was planning to use firebase but the website’s database is not designed for firebase.
        gakwaya	
        gakwaya
        October 31, 2017 at 4:59 am Reply	

        Sorry for the late reply Karl. I can’t really understand what it is you are trying to achieve. But if you want to communicate from android app to a web based xmpp client , that is very possible. You may know if the website uses xmpp by looking at its source code.
    Avatar	
    sonu
    July 11, 2017 at 8:08 am Reply	

    Well…thank you very much, it does exactly what it says.
    is there any way i can get notification when i receive a message?
    and also how can i make this app to login with another account instead of xmpp account?
        gakwaya	
        gakwaya
        October 31, 2017 at 5:03 am Reply	

            s there any way i can get notification when i receive a message? 

        Sure, you can implement the logic to pop up a notification when the a message comes in, have that in mind but haven’t had time to implement that yet.

            how can i make this app to login with another account instead of xmpp account? 

        What other account ? Can’t really be of much help without further details.
    Avatar	
    Himanshu Srivastava
    July 27, 2017 at 10:18 am Reply	

    Hey Man,

    Thanks for this post. It is amazing But how to handle certification of SSL and many more
        gakwaya	
        gakwaya
        October 31, 2017 at 5:07 am Reply	

        Smack handles ssl certification for you on the client side. You just have to make it required like this

        setSecurityMode(ConnectionConfiguration.SecurityMode.required)

        On the server side, https://letsencrypt.org/ is a good candidate for your xmpp server.

        Hope this is helpful
    Avatar	
    RameshJ
    August 10, 2017 at 12:37 pm Reply	

    Awesome Tutorial. I thoroughly enjoyed reading and practicing this project. I am very new android and I got lot of new information from this.
        gakwaya	
        gakwaya
        October 31, 2017 at 5:08 am Reply	

        Glad it was helpful RameshJ.
    Avatar	
    juliusham
    August 16, 2017 at 11:11 am Reply	

    Hi thanks for the tutorial. my problem is that when i click to sign up, the process takes long and the log simply says
    LoginActivity: saveCredentialsAndLogin() called.
    08-16 12:08:45.052 27287-27287/com.ash.mychat D/RoosterService: onStartCommand()
    08-16 12:08:45.052 27287-27287/com.ash.mychat D/RoosterService: Service Start() function called.

    and no other error.
    At times it signs in well. i cant tell why this happens. my internet connection is fast enough. please guide me
        gakwaya	
        gakwaya
        October 31, 2017 at 5:12 am Reply	

        The login process usually takes long because of the ssl handshake happening between your client and server. It usually takes 10~30 seconds on my environment. But this really depends on your environment. Your connection might be good but your server might be not that fast on responding. But again these are just guesses. You might have to diagnose more to find out what is the problem here.

        Hope this is helpful.
    Avatar	
    Rafael G Blanco
    October 22, 2017 at 9:13 pm Reply	

    Hi, can you help me.

    I’m using Firebase Google, but I can not connect, you can help me solve this problem.

    Exception.
    10-21 23:19:08.930 30047-30188/com.blikoon.rooster W/System.err: org.jivesoftware.smack.sasl.SASLErrorException: SASLError using X-OAUTH2: not-authorized
    10-21 23:19:08.930 30047-30188/com.blikoon.rooster W/System.err: at org.jivesoftware.smack.SASLAuthentication.authenticationFailed(SASLAuthentication.java:291)
    10-21 23:19:08.940 30047-30188/com.blikoon.rooster W/System.err: at org.jivesoftware.smack.tcp.XMPPTCPConnection$PacketReader.parsePackets(XMPPTCPConnection.java:1084)
    10-21 23:19:08.940 30047-30188/com.blikoon.rooster W/System.err: at org.jivesoftware.smack.tcp.XMPPTCPConnection$PacketReader.access$300(XMPPTCPConnection.java:982)
    10-21 23:19:08.940 30047-30206/com.blikoon.rooster I/XMPPTCPConnection: XMPPTCPConnection[not-authenticated] (0) received closing element. Server wants to terminate the connection, calling disconnect()
    10-21 23:19:08.940 30047-30188/com.blikoon.rooster W/System.err: at org.jivesoftware.smack.tcp.XMPPTCPConnection$PacketReader$1.run(XMPPTCPConnection.java:998)
    10-21 23:19:08.940 30047-30188/com.blikoon.rooster W/System.err: at java.lang.Thread.run(Thread.java:841)
    10-21 23:19:31.860 30047-30556/com.blikoon.rooster I/XMPPTCPConnection: XMPPTCPConnection[not-authenticated] (1) received closing element. Server wants to terminate the connection, calling disconnect()
    10-21 23:19:31.860 30047-30544/com.blikoon.rooster W/System.err: org.jivesoftware.smack.sasl.SASLErrorException: SASLError using X-OAUTH2: not-authorized
    10-21 23:19:31.860 30047-30544/com.blikoon.rooster W/System.err: at org.jivesoftware.smack.SASLAuthentication.authenticationFailed(SASLAuthentication.java:291)
    10-21 23:19:31.860 30047-30544/com.blikoon.rooster W/System.err: at org.jivesoftware.smack.tcp.XMPPTCPConnection$PacketReader.parsePackets(XMPPTCPConnection.java:1084)
    10-21 23:19:31.860 30047-30544/com.blikoon.rooster W/System.err: at org.jivesoftware.smack.tcp.XMPPTCPConnection$PacketReader.access$300(XMPPTCPConnection.java:982)
    10-21 23:19:31.870 30047-30544/com.blikoon.rooster W/System.err: at org.jivesoftware.smack.tcp.XMPPTCPConnection$PacketReader$1.run(XMPPTCPConnection.java:998)
    10-21 23:19:31.870 30047-30544/com.blikoon.rooster W/System.err: at java.lang.Thread.run(Thread.java:841)

    Config

    new AndroidSmackInitializer().initialize();

    XMPPTCPConnection.setUseStreamManagementResumptionDefault(true);
    XMPPTCPConnection.setUseStreamManagementDefault(true);

    SASLAuthentication.registerSASLMechanism(new SASLXOauth2Mechanism() );

    XMPPTCPConnectionConfiguration.Builder conf = XMPPTCPConnectionConfiguration.builder();
    conf.setXmppDomain(“FCM XMPP Client Connection Server”);
    conf.setHostAddress( InetAddress.getByName(GCM_SERVER ));
    conf.setPort(GCM_PORT);

    conf.setSecurityMode(ConnectionConfiguration.SecurityMode.disabled);
    conf.setSendPresence(false);
    conf.setSocketFactory(SSLSocketFactory.getDefault());
    // Launch a window with info about packets sent and received
    conf.setDebuggerEnabled(true);

    setupUiThreadBroadCastMessageReceiver();

    mConnection = new XMPPTCPConnection(conf.build());

    mConnection.addConnectionListener(this);
    try {
    Log.d(TAG, “Calling connect() “);
    mConnection.connect();

    mConnection.login(“#######@gcm.googleapis.com”, “APIIIIIIIIIIIIIIIIIIIIIII”);
    Log.d(TAG, ” login() Called “);
    } catch (InterruptedException e) {
    e.printStackTrace();
    }
        gakwaya	
        gakwaya
        October 31, 2017 at 5:16 am Reply	

        I haven’t worked with firebase, so I can’t be of much help here. I guess you might get more helpful feedback from firebase specific forums or tutorial.

        Good luck.
    Avatar	
    Ashu Kumar
    November 3, 2017 at 8:24 am Reply	

    Your tutorial is good and i m learning it, thanks for it but now in updated version of smack 4.2.1 connection is not working correctly in localhost server installed on local device.
    please update ur connection with https://stackoverflow.com/a/43163972/4140878. it is working and i tested it.

    thanks again 🙂
        gakwaya	
        gakwaya
        November 4, 2017 at 4:01 am Reply	

        Thanks Ashu for the input,
        Were you building the code from github ?, I am in the process of updating it and I will consider your suggestion. I still have to find the time to update the tutorial. Happy coding!
    Avatar	
    Glados
    January 18, 2018 at 3:03 pm Reply	

    Hi ! Firt, i will say thank you for your job, everything work fine, but i got a graphic bug when i receive the message. I got a black line with the content of the message.

    Can you help there ?
        gakwaya	
        gakwaya
        January 20, 2018 at 7:27 am Reply	

        Looks like tthis?. Seems to be a bug in the ui-library
            Avatar	
            Glados
            January 22, 2018 at 10:33 am Reply	

            Yes exaclty like this, i guess yes
    Avatar	
    Ranchhod
    January 29, 2018 at 9:54 am Reply	

    Hi gakwaya!! How can i send multimedia and documents?
        gakwaya	
        gakwaya
        January 29, 2018 at 5:31 pm Reply	

        The best technique to send documents for now is Http File Upload. Ofcourse your server needs to support it.
    Avatar	
    Hashim
    January 31, 2018 at 11:43 am Reply	

    Hello,
    Im running the project from the github link.
    and even though I’ve created a user on the web and I changed the host link in the RoosterConnection.java to the host url im working on., i cant seem to log in on my mobile with the error “Something went wrong while connecting ,make sure the credentials are right and try again”.

    What could be the problem here?
        Avatar	
        Hashim
        January 31, 2018 at 2:25 pm Reply	

        Okay I can log in now, but the messages arent being shown to the other user.

        D/SMACK: SENT (0): How are you

        D/SMACK: RECV (0):

        D/SMACK: RECV (0):
            Avatar	
            Hashim
            January 31, 2018 at 2:27 pm Reply	

            to=”uuu@localhost/HashimAndroid” from=”uuu@192.168.41.177″
            it is giving error ‘remote server not found’
            error code 404
                gakwaya	
                gakwaya
                February 7, 2018 at 4:01 am Reply	

                I would have to know a lot about your setup to be able to help. But I would try with public XMPP Server accounts, confirm it works and from there diagnose the problem with my local setup.
    Avatar	
    Ravi
    February 6, 2018 at 7:04 pm Reply	

    Very good post and learned a lot. Currently trying to send Multimedia (specially images) but always getting 503 service unavailable error with Openfire and smack. Posted the same in stackoverflow as well (https://stackoverflow.com/questions/48604866/file-transfer-with-smack-android-xmpp). Would be really great if you can help me. Thank you.
        gakwaya	
        gakwaya
        February 7, 2018 at 3:58 am Reply	

        If I remember correctly File upload is provided by a plugin that was lately developed by the Openfire guys. You have to enable it somehow. I don’t use Openfire personally but you can ask in the “open_chat@conference.igniterealtime.org” MUC and you’ll probably get better advice.

        Good luck.
            Avatar	
            Ravi
            February 7, 2018 at 6:51 pm Reply	

            Thanks a lot.. let me drop an email to them,
    Avatar	
    shiv
    March 14, 2018 at 3:30 pm Reply	

    Hi thanks for the tutorial.
    I ma facing this problem on Oreo

    android.app.RemoteServiceException: Context.startForegroundService() did not then call Service.startForeground() at android.app.ActivityThread$H.handleMessage(ActivityThread.java:1775)at android.os.Handler.dispatchMessage(Handler.java:105)

    I have added startForegroundService(intent) in my service onStartCommand method but still showing same error
    Thank you again for the tutorial
        Avatar	
        shiv
        March 14, 2018 at 3:34 pm Reply	

        Thanks again
        gakwaya	
        gakwaya
        March 14, 2018 at 4:20 pm Reply	

        Hi Shiv,
        Unfortunately I can’t know what is the problem without looking at your code. I would advise compiling the code from github and let me know if you are having the same problem.
    Avatar	
    Taek
    March 18, 2018 at 4:23 pm Reply	

    Thank you!!! but I have a question !! Where is Server code??

    I think separate server and client code.
        gakwaya	
        gakwaya
        March 19, 2018 at 4:23 am Reply	

        You need to use a separately configured XMPP server to use your XMPP client against. Please have a look at the introductory tutorial
    Avatar	
    Eduardo
    March 26, 2018 at 1:12 am Reply	

    Hi,

    I have compiled on Android Studio 3.01 without any problems.

    I registered at xmpp.jp website also without problems .

    What is the error below?
    Trying to connect using your very first login activity I got:

    03-25 20:09:39.011 2708-2708/com.blikoon.rooster D/RoosterService: onCreate()
    03-25 20:09:39.011 2708-2708/com.blikoon.rooster D/RoosterService: onStartCommand()
    03-25 20:09:39.011 2708-2708/com.blikoon.rooster D/RoosterService: Service Start() function called.
    03-25 20:09:39.021 2708-5890/com.blikoon.rooster D/RoosterService: initConnection()
    03-25 20:09:39.021 2708-5890/com.blikoon.rooster D/RoosterConnection: RoosterConnection Constructor called.
    03-25 20:09:39.021 2708-5890/com.blikoon.rooster D/RoosterConnection: Connecting to server xmpp.jp
    03-25 20:09:39.021 2708-5890/com.blikoon.rooster D/RoosterConnection: Username : username
    03-25 20:09:39.021 2708-5890/com.blikoon.rooster D/RoosterConnection: Password : alteredforsecurity
    03-25 20:09:39.021 2708-5890/com.blikoon.rooster D/RoosterConnection: Server : xmpp.jp
    03-25 20:09:39.031 2708-5890/com.blikoon.rooster D/RoosterConnection: Calling connect()
    03-25 20:10:10.661 2708-5890/com.blikoon.rooster D/RoosterService: Something went wrong while connecting ,make sure the credentials are right and try again
    03-25 20:10:10.661 2708-5890/com.blikoon.rooster W/System.err: org.jivesoftware.smack.XMPPException$StreamErrorException: host-unknown You can read more about the meaning of this stream error at http://xmpp.org/rfcs/rfc6120.html#streams-error-conditions
    03-25 20:10:10.661 2708-5890/com.blikoon.rooster W/System.err:
    03-25 20:10:10.661 2708-5890/com.blikoon.rooster W/System.err: at org.jivesoftware.smack.tcp.XMPPTCPConnection$PacketReader.parsePackets(XMPPTCPConnection.java:1053)org.jivesoftware.smack.XMPPException$StreamErrorException: host-unknown You can read more about the meaning of this stream error at http://xmpp.org/rfcs/rfc6120.html#streams-error-conditions
    03-25 20:05:15.751 2708-5451/com.blikoon.rooster W/System.err:
    03-25 20:05:15.751 2708-5451/com.blikoon.rooster W/System.err: at org.jivesoftware.smack.tcp.XMPPTCPConnection$PacketReader.parsePackets(XMPPTCPConnection.java:1053)
    Avatar	
    Eduardo
    March 26, 2018 at 4:46 pm Reply	

    Hi,

    How to change your app to provide chat to three or more persons simultaneously?
    Thank you!
    Avatar	
    Yash
    October 16, 2018 at 4:00 pm Reply	

    Hey thank you for this tutorial. Just one question

    How to install openfire server on cPanel (my domain)?
    Avatar	
    Pallav
    December 2, 2018 at 12:15 pm Reply	

    Hi Akhilesh,
    Are you logging in from two different devices, with two different userid’s
    because it is not working for me. And i am using smack 4.3,please help me.

    Thanks in advance
    Avatar	
    Pallav
    December 2, 2018 at 2:05 pm Reply	

    HI Daniel,
    Thanks for the tutorial, they are working nice but the incomingchatmessagelistener is not working for received messages ,while message are sent succesfully. I am using smack 4.3 can you please suggest where am i going wrong. I am using your last committed code on github.
    Avatar	
    Pallav
    December 3, 2018 at 8:16 am Reply	

    Hi Daniel,
    Your tutorials are good, but the incomingmessagelistener is not working in the latest committed code.Can you please check it, it will be very helpful for me.
    Thank you in advance
    Avatar	
    Kato
    January 8, 2019 at 7:59 am Reply	

    Hi,

    I have a question, does this client app connects with any xxmp server or specific server.

    Reading through this owesome tutorial, I am unable to see the specific parameters that the app use to connect to a specific server, parameters such as servername, port, etc.

    Once again thanks for the tutorial.
        gakwaya	
        gakwaya
        January 8, 2019 at 8:51 am Reply	

        Hi,
        Any XMPP server would work fine. You can read on the basics of xmpp here : https://www.blikoontech.com/xmpp/xmpp-a-soft-friendly-introduction
    Avatar	
    Amann
    January 20, 2019 at 7:34 pm Reply	

    Hi!
    I’m developing my own chat application which will work offline in my local server.
    And everything except chatting is done. I store every user’s information in my database.
    And now I have a trouble with chatting.
    After reading your tutorial I understand that you do not use any specific servers or smth like that.
    Could you possibly say me if I follow this tutorial will I have acces to send messages locally(offfline)?
    Avatar	
    Priya
    February 8, 2019 at 1:02 pm Reply	

    I found error on this section. please help me
    public void connect() throws IOException,XMPPException,SmackException
    {
    Log.d(TAG, “Connecting to server ” + mServiceName);
    XMPPTCPConnectionConfiguration.Builder builder=
    XMPPTCPConnectionConfiguration.builder();
    builder.setServiceName(mServiceName);//error
    builder.setUsernameAndPassword(mUsername, mPassword);
    builder.setRosterLoadedAtLogin(true);// canot resolve the method
    builder.setResource(“Rooster”);

    //Set up the ui thread broadcast message receiver.
    //setupUiThreadBroadCastMessageReceiver();

    mConnection = new XMPPTCPConnection(builder.build());
    mConnection.addConnectionListener(this);
    mConnection.connect();
    mConnection.login();

    ReconnectionManager reconnectionManager = ReconnectionManager.getInstanceFor(mConnection);
    reconnectionManager.setEnabledPerDefault(true);
    reconnectionManager.enableAutomaticReconnection();
    Avatar	
    Sara Javed
    March 4, 2019 at 7:16 am Reply	

    Hello everyone !
    I am new in XMPP . Need your help Daniel and anyone else who has encountered it before . When I run the app nothing happens . Like @Priya my app is also causing issues and here
    Log.d(TAG, “Connecting to server ” + mServiceName);
    XMPPTCPConnectionConfiguration.Builder builder=
    XMPPTCPConnectionConfiguration.builder();
    builder.setServiceName(mServiceName);//error
    builder.setUsernameAndPassword(mUsername, mPassword);
    builder.setRosterLoadedAtLogin(true);// canot resolve the method
    builder.setResource(“Rooster”);

    //Set up the ui thread broadcast message receiver.
    //setupUiThreadBroadCastMessageReceiver();

    mConnection = new XMPPTCPConnection(builder.build());
    mConnection.addConnectionListener(this);
    mConnection.connect();
    mConnection.login();

    I got 2 errors :
    1 : Must have XMPP Domain at the mService . Can anyone let why it is so?
    2 : And setRosterLoadedAtLogin(true) is marked .

    Any help will be great !
    Avatar	
    Sara Javed
    March 4, 2019 at 7:22 am Reply	

    Hello Everyone mService is giving an exception shows as Must have a XMPP Domain . So whats the issue and how will we get the server name or domain . I had openfire server installed as me at admin and a user as well but still when I can app nothing is happening it crashes and ask for domain name
    Avatar	
    Virendra Kachhi
    October 12, 2019 at 4:03 pm Reply	

    I not getting recent message for below code.
    private void getArchivedMessage(Date date) {
    Log.e(“inArchive”, “getArchivedMessages: “);
    if (connection != null) {
    String userId = SharedPreferenceUtility.getInstance(activity).getString(Constants.USER_ID);
    MamManager manager = MamManager.getInstanceFor(XmppConnection.getInstance().getConnection());
    DBManager dbManager = new DBManager(activity);
    try {
    manager.enableMamForAllMessages();
    } catch (Exception e) {
    e.printStackTrace();
    }
    MamManager.MamQueryResult r = null;
    try {
    MamManager.MamQueryArgs mamQueryArgs = MamManager.MamQueryArgs.builder()
    .limitResultsBefore(date)
    .setResultPageSizeTo(1000000000)
    .queryLastPage()
    .build();
    MamManager.MamQuery mamQuery = manager.queryArchive(mamQueryArgs);
    List messageList = mamQuery.getMessages();
    for (Message message : messageList) {
    try {
    String body = message.getBody();
    SenderMessage sm = new Gson().fromJson(body, SenderMessage.class);
    if (sm.getFromUserId().equalsIgnoreCase(userId)) {
    sm.setSender(true);
    } else {
    sm.setSender(false);
    }
    dbManager.addArchiveMessage(sm, sm.isSender());
    } catch (Exception e) {
    e.printStackTrace();
    }
    }
    } catch (Exception e) {
    e.printStackTrace();
    }
    }
    }
    Avatar	
    griffin
    November 2, 2019 at 1:30 pm Reply	

    yoo thanks bro worked fine



<a href="https://blikoon.teachable.com/p/android-xmpp-chat-app-video-tutorial">**XMPP Smack Android Comprehensive Video Course Available!**</a> By the very same author of this tutorial.
{: .notice--warning}
[![foo]({{ site.url }}{{ site.baseurl }}/images/xmpp-smack-post/teachablexmppcourses.png)](https://blikoon.teachable.com/p/android-xmpp-chat-app-video-tutorial)

<a href="https://blikoon.teachable.com/p/android-xmpp-smack-chat-app-sending-and-receiving-files-http-file-uploadl">**Follow up course on Sending and Receiving Files with XMPP and Smack also available!**</a>
{: .notice--info}




