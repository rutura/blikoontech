---
permalink : /networking/how-to-install-ejabberd-on-linux-ubuntu
title:  "How to Install ejabberd from source code on linux ubuntu"
search: false
categories: 
  - XMPP
  - Server
classes: wide
sidebar:
  - text: "[![your logo](/images/xmpp-smack-post/SmackCourseAd2.png)](https://blikoon.teachable.com/p/android-xmpp-chat-app-video-tutorial)"
last_modified_at: 2016-01-04T08:06:00-05:00
---

ejabberd is an XMPP application server powering a good number of instant messaging applications out there.In this post we are going to learn how to install it on a unix based operating system.I will cover ubuntu14.04,but the steps should work on all systems with little or no modifications.

While installing any piece of software ,it is always a good idea to check what the official documentation has to say ,processOne has a good page covering how to install ejabberd.In this tutorial we are going to install from the source code as I find this option most flexible(it is also the most complicated).

Grab yourself a copy of the source code from either github or ejabberd download page.Note that the github version is a development version and it might contain bugs.Use the github version if you want to try the latest introduced features and in production environments use the released standard version from the processOne download page.

```xml
git clone git://github.com/processone/ejabberd.git ejabberd
```

To compile ejabberd you need:

  * GNU Make.
  * GCC.
  * Libexpat 1.95 or higher.
  * Libyaml 0.1.4 or higher.
  * Erlang/OTP 17.1 or higher.
  * OpenSSL 1.0.0 or higher, for STARTTLS, SASL and SSL encryption.
  * Zlib 1.2.3 or higher, for Stream Compression support (XEP-0138). Optional.
  * PAM library. Optional. For Pluggable Authentication Modules (PAM).
  * GNU Iconv 1.8 or higher, for the IRC Transport (mod_irc). Optional. Not needed on systems with GNU Libc.
  * ImageMagick’s Convert program. Optional. For CAPTCHA challenges.

Install the dependencies :

```xml 
sudo apt-get install libexpat1 libexpat1-dev libyaml-0-2 libyaml-dev erlang openssl zlib1g zlib1g-dev libssl-dev libpam0g
```

The erlang version provided by my ubuntu repo was 16.x.x but 17.1 or higher is required by this ejabberd version we are installing .I had to go to the eerlang download page and downloaded the latest version.

Install the erlang package.Mine is a debian package so I type :

```xml
dpkg-i xxxxxxx.deb
```

After I download the source code (github or processOne release ) my souce code is stored in

```xml
/home/gakwaya/ejabberd/ejabberd
```

as shown below:


<img src="{{ site.url }}{{ site.baseurl }}/images/how-to-install-ejabberd-on-linux-ubuntu/ejabberd_install_dir.png" alt="Ejabberd Install Directory">


After you have the source code you need to know the install options available to you.Type

```xml
./autogen
```

This generates a configure script whose options you can show by typing:

```xml
./configure --help
```

From the help output we see a lot of options for our ejabberd installation ,for instance we can specify the location where ejabberd is installed.By default ejabberd installed files are laid out as follows :

    /etc/ejabberd/: Configuration directory:
        ejabberd.yml: ejabberd configuration file
        ejabberdctl.cfg: Configuration file of the administration script
        inetrc: Network DNS configuration file for Erlang
    /lib/ejabberd/:
        ebin/: Erlang binary files (*.beam)
        include/: Erlang header files (*.hrl)
        priv/: Additional files required at runtime
        bin/: Executable programs
        lib/: Binary system libraries (*.so)
        msgs/: Translation files (*.msgs)
    /sbin/ejabberdctl: Administration script (see section ejabberdctl)
    /share/doc/ejabberd/: Documentation of ejabberd
    /var/lib/ejabberd/: Spool directory:
        .erlang.cookie: Erlang cookie file (see section cookie)
        acl.DCD, ...: Mnesia database spool files (*.DCD, *.DCL, *.DAT)
    /var/log/ejabberd/: Log directory (see section [logfiles]):
        ejabberd.log: ejabberd service log
        erlang.log: Erlang/OTP system log

You can specify a different location for your installation by specifying a prefix on your configure command :

```xml
./configure --prefix=/opt/ejabberd-server
```

Another option I find useful is enabling mysql support ,you can do this by typing :

```xml
./configure --prefix=/opt/ejabberd-server --enable-mysql
```

Type

```xml
make
```

and this causes for the needed modules to be downloaded:

<img src="{{ site.url }}{{ site.baseurl }}/images/how-to-install-ejabberd-on-linux-ubuntu/ejabberd_make.png" alt="ejabberd_make">


and ejabberd to be compiled,you should see something like this when your make command is done:


<img src="{{ site.url }}{{ site.baseurl }}/images/how-to-install-ejabberd-on-linux-ubuntu/erlang_make_finished.png" alt="erlang_make_finished">


Type

```xml
make install
```

to install ejabberd files .Those files are copied to the directory we specified with our configure command in the prefix option .cd to that directory and you see your ejabberd installation files :


<img src="{{ site.url }}{{ site.baseurl }}/images/how-to-install-ejabberd-on-linux-ubuntu/ejabberd_installed_in_dir.png" alt="ejabberd_installed_in_dir">


Our ejabberdctl program is stored in the sbin directory.To test that our installation works ,we can run it and check the status of our server :

ejabberdctl_test

We can see that our node is down.ejabberd is installed but not started yet.Start it by typing

```xml
./ejabberdctl start
```

<img src="{{ site.url }}{{ site.baseurl }}/images/how-to-install-ejabberd-on-linux-ubuntu/ejabberd_running.png" alt="ejabberd_running">


and we can see that it started and running.Here it says that it is ejabberd 0.0 running while we have installed ejabberd 15.11 .This is subject to further investigation ,we ignore it for now.

With our installation working we need to add the admin user and add a few user accounts so that xmpp clients can user them to connect .The processone guide page gives enough info on this but we will reproduce it here just to be complete .

If you go to the /etc/ejabberd directory of your installation you will see a ejabberd.yml file.This is your configuration file.Open the file with your favorite text editor and add another served host.This can be any ip address accessible from the outside or any ping-able domain name.Mine is configured to use a local IP address as follows:


<img src="{{ site.url }}{{ site.baseurl }}/images/how-to-install-ejabberd-on-linux-ubuntu/ejabberd_add_served_host.png" alt="ejabberd_add_served_host">


Register an admin user with the command:

```xml
./ejabberdctl register admin1 localhost ejabberdPassword
./ejabberdctl register admin1 192.168.1.106 ejabberdPassword
```

This registers the normal xmpp account user.We register twice because we have two served hosts on the server The 192.168.1.106 account was added because we want access to the web admin from other machines.I left the localhost host here simply because it was there and it doesn’t hurt to keep it there but feel free to comment it out and see what happens.It s a good way to learn.

To give our users admin privileges we modify the ejabberd.yml config file as follows:


<img src="{{ site.url }}{{ site.baseurl }}/images/how-to-install-ejabberd-on-linux-ubuntu/ejabberd_give_admin_privileges.png" alt="ejabberd_give_admin_privileges">


Be careful with how you indent your changes in the ejabberd.yml file as errors are hard to debug especially when you are not familiar with yaml.A good example of such an error is this github issue .

We have created two xmpp accounts admin1@localhost and admin1@192.168.1.106 on our server.These accounts have bee assigned admin privileges and they can do all kinds of things on the server such as adding and deleting users and much much more such as changing the configurations on the server.

Start the server :


<img src="{{ site.url }}{{ site.baseurl }}/images/how-to-install-ejabberd-on-linux-ubuntu/ejabberd_start_server.png" alt="ejabberd_start_server">


and from your browser ,open the address( it is ofcouse your IP address that you use) :

```xml
http://192.168.1.106:5280/admin/
```

A window shows up asking you login credentials.Type in your username in the form user@domain (or user@ipAddress) and your password:


<img src="{{ site.url }}{{ site.baseurl }}/images/how-to-install-ejabberd-on-linux-ubuntu/ejabberd_admin_login.png" alt="ejabberd_admin_login">


Click ok and you are presented the admin interface:


<img src="{{ site.url }}{{ site.baseurl }}/images/how-to-install-ejabberd-on-linux-ubuntu/ejabberd_admin_interface.png" alt="ejabberd_admin_interface">

From here you can do all kinds of things ,for example click on Virtual hosts and see the hosts your server is currently serving:


<img src="{{ site.url }}{{ site.baseurl }}/images/how-to-install-ejabberd-on-linux-ubuntu/ejabberd_admin_served_hosts.png" alt="ejabberd_admin_served_hosts">


Click on 192.168.1.106 and see settings relating to one host


<img src="{{ site.url }}{{ site.baseurl }}/images/how-to-install-ejabberd-on-linux-ubuntu/ejabberd_admin_host_configuration.png" alt="ejabberd_admin_host_configuration">


Click on users and the shown interface allows you to add users to your host:


<img src="{{ site.url }}{{ site.baseurl }}/images/how-to-install-ejabberd-on-linux-ubuntu/ejabberd_admin_add_users.png" alt="ejabberd_admin_add_users">


We have added two accounts to our server host ,aa@192.168.1.106 and bb@192.168.1.106.These are legitimate accounts on our server and they can actually login from an xmpp client with access to my local network.

If I login from my xmpp clients,the two accounts can exchange messages as shown below :


<img src="{{ site.url }}{{ site.baseurl }}/images/how-to-install-ejabberd-on-linux-ubuntu/ejabberd_client_two_chat.png" alt="ejabberd_client_two_chat">


And from our web admin interface ,we can see them online:


<img src="{{ site.url }}{{ site.baseurl }}/images/how-to-install-ejabberd-on-linux-ubuntu/ejabberd_users_online.png" alt="ejabberd_users_online">


This concludes our ejabberd installation tutorial.What we have done is just a tip of the iceberg and ejabberd can do much much more.Nothing beats trying it yourself and learning from the mistakes you make in the process.If you need to learn more about ejabberd and xmpp in general ,the processone help page and the xmpp standard website are good places to know about.And as always feedback is most welcome.I hope this has been helpful to you and thanks for the visit.

<a href="https://blikoon.teachable.com/p/android-xmpp-chat-app-video-tutorial">**XMPP Smack Android Comprehensive Video Course Available!**</a> By the very same author of this tutorial.
{: .notice--warning}
[![foo]({{ site.url }}{{ site.baseurl }}/images/xmpp-smack-post/teachablexmppcourses.png)](https://blikoon.teachable.com/p/android-xmpp-chat-app-video-tutorial)

<a href="https://blikoon.teachable.com/p/android-xmpp-smack-chat-app-sending-and-receiving-files-http-file-upload">**Follow up course on Sending and Receiving Files with XMPP and Smack also available!**</a>
{: .notice--info}

 2 Comments
 
 **Commenting not available on this page at the moment! Please hit the author on various social media channels or share your thoughts on this github issue. Worthy conversations will be reproduced here. Thanks for understanding**
{: .notice--info}

    Avatar	
    m7mdhassballa
    March 8, 2018 at 9:12 am Reply	

    hey gakwaya
    thank you for your amazing tutorials.
    i want to know how to connect this server with this android clients.
    https://www.blikoontech.com/tutorials/android-smack-xmpp-introductionbuilding-a-simple-client
    because i don’t no much about back-end .
    i there any changes should i do to connect this server with the android clients?
    gakwaya	
    gakwaya
    March 8, 2018 at 2:54 pm Reply	

    Hi m7mdhassballa,
    The client should connect without problems.
