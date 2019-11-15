---
permalink : /tutorials/how-to-install-ejabberd-with-os-specific-package-deb
title:  "How to install ejabberd with os specific package(.deb)"
search: false
categories: 
  - XMPP
  - Server
classes: wide
sidebar:
  - text: "[![your logo](/images/xmpp-smack-post/SmackCourseAd2.png)](https://blikoon.teachable.com/p/android-xmpp-chat-app-video-tutorial)"
last_modified_at: 2016-01-04T08:06:00-05:00
---

In a previous tutorial we showed how to install ejabberd from source code wich is rather involved process but also flexible.In this tutorial we show a less complicated way to get your ejabberd instance up and running.

This will work for you only if processOne or someone has already provided packages for the operating system flavour you are using.The ProcessOne installation page states that they provide both RPM and DEB packages respectively for RedHat and Debian based systems.

To quote them “ProcessOne now provides RPM and DEB all in one packages as well, since ejabberd version 15.06. This is self-sufficient packages also containing a minimal Erlang distribution. It ensures that it does not interfere with your existing Erlang version. This is also a good way to make sure ejabberd will run with the latest Erlang version.”

.If your operating system is not covered ,you hay have to build from the source code .I show how to install the DEB package on an Ubuntu 14.04LTS system.

Grab yourself a copy of the package from the ProcessOne download page and store it somewhere on your filesystem:


<img src="{{ site.url }}{{ site.baseurl }}/images/how-to-install-ejabberd-with-os-specific-package/ejabberd_download_deb_package.png" alt="ejabberd_download_deb_package">


Install your package by running dpkg -i on the downloaded deb package :


<img src="{{ site.url }}{{ site.baseurl }}/images/how-to-install-ejabberd-with-os-specific-package/ejabberd_install_deb_package.png" alt="ejabberd_install_deb_package">


After the dpkg -i command is done, the system only tells you that it has finished setting up ejabberd and nothing more.This makes it unclear where ejabberd was installed on the system especially if you are not very familiar with ejabberd or linux in general.If this is the case ,go to your root directory and run the locate command on the ejabberdctl program .


<img src="{{ site.url }}{{ site.baseurl }}/images/how-to-install-ejabberd-with-os-specific-package/ejabberd_deb_package_ejabberdctl_location.png" alt="ejabberd_deb_package_ejabberdctl_location">


We can see that ejabberdctl is located in my /opt/ejabberd-15.11/bin directory .If we go up one directory we can see the directory structure of our installation.


<img src="{{ site.url }}{{ site.baseurl }}/images/how-to-install-ejabberd-with-os-specific-package/ejabberd_deb_package_dir_structure.png" alt="ejabberd_deb_package_dir_structure">


Note that this directory structure is different from the one you get when you install from the source code ,which is reproduced below for reference .


<img src="{{ site.url }}{{ site.baseurl }}/images/how-to-install-ejabberd-with-os-specific-package/ejabberd_installed_in_dir.png" alt="ejabberd_installed_in_dir">


The most important directories for now are bin and conf , bin is where your control programs are located : ejabberdctl is in there and beside that there are start ,stop and status scripts that you can run directly without going through ejabberdctl.


<img src="{{ site.url }}{{ site.baseurl }}/images/how-to-install-ejabberd-with-os-specific-package/ejabberd_deb_package_bin_dir_contents.png" alt="ejabberd_deb_package_bin_dir_contents">


The conf dir contains your configuration files and the one you will need straight away is ejabberd.yml:


<img src="{{ site.url }}{{ site.baseurl }}/images/how-to-install-ejabberd-with-os-specific-package/ejabberd_deb_package_conf_dir_content.png" alt="ejabberd_deb_package_conf_dir_content">


With your control programs and configuration files in place the rest is configuring host names and admin account credentials as we did in the from source install tutorial.

If you run into any problems in your installation process ,don’t hesitate to drop me a line.I hope this will come in handy for someone around there.

<a href="https://blikoon.teachable.com/p/android-xmpp-chat-app-video-tutorial">**XMPP Smack Android Comprehensive Video Course Available!**</a> By the very same author of this tutorial.
{: .notice--warning}
[![foo]({{ site.url }}{{ site.baseurl }}/images/xmpp-smack-post/teachablexmppcourses.png)](https://blikoon.teachable.com/p/android-xmpp-chat-app-video-tutorial)

<a href="https://blikoon.teachable.com/p/android-xmpp-smack-chat-app-sending-and-receiving-files-http-file-upload">**Follow up course on Sending and Receiving Files with XMPP and Smack also available!**</a>
{: .notice--info}


 4 Comments
 
  **Commenting not available on this page at the moment! Please hit the author on various social media channels or share your thoughts on this github issue. Worthy conversations will be reproduced here. Thanks for understanding**
{: .notice--info}

    Avatar	
    mohit
    May 3, 2017 at 12:55 pm Reply	

    Sir, i am unable to register a new user in the server.
    sudo ejabberdctl register admin localhost password
    doesnt work, cant log into admin web panel for the same.
    the config file also doesnt have this info.
    gakwaya	
    gakwaya
    May 6, 2017 at 2:31 am Reply	

    Hi,
    Have you followed the instructions here : http://www.blikoon.com/networking/how-to-install-ejabberd-on-linux-ubuntu ?
    Avatar	
    WangWenshuo
    May 9, 2017 at 4:16 pm Reply	

    Sir，I installment it follow your blog.But I don’t know admin password and can you help me?Thank you very much.
        gakwaya	
        gakwaya
        May 9, 2017 at 4:55 pm Reply	

        You create a normal user account and you add it in the admin list in the configuration file
        as shown here http://www.blikoon.com/networking/how-to-install-ejabberd-on-linux-ubuntu


