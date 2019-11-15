---
permalink : /tutorials/android-smack-xmpp-introductionbuilding-a-simple-client
title:  "Android Smack XMPP Introduction:Building a Simple Client"
search: false
categories: 
  - XMPP
classes: wide
sidebar:
  - text: "[![your logo](/images/xmpp-smack-post/SmackCourseAd2.png)](https://blikoon.teachable.com/p/android-xmpp-chat-app-video-tutorial)"
comments:
  provider: "facebook"
  facebook:
    appid: # optional
    num_posts: # 5 (default)
    colorscheme: # "light" (default), "dark"
last_modified_at: 2016-04-29T08:06:00-05:00
---
<img src="{{ site.url }}{{ site.baseurl }}/images/header.jpg" alt="XMPP Foundations">

Xmpp is a protocol for Presence and Messaging , and Smack is a Java/android implementation of the protocol that helps developers build fast client applications.When one embarks on the journey to build android chat apps based on xmpp using Smack ,there are a lot of pitfalls one can come across as you are trying to combine the three worlds of Java , Android and the Xmpp protocol itself to build one coherent product.This tutorial aims at documenting these and possible ways to mitigate them.By the end of this tutorial ,you will have an android chat client that can connect to and XMPP server , send and receive messages .Most importantly ,you will learn the android plumbing necessary to get all this rolling.Lets get started.Shall we.

<a href="https://blikoon.teachable.com/p/android-xmpp-chat-app-video-tutorial">**XMPP Smack Android Comprehensive Video Course Available!**</a> By the very same author of this tutorial.
{: .notice--warning}
[![foo]({{ site.url }}{{ site.baseurl }}/images/xmpp-smack-post/teachablexmppcourses.png)](https://blikoon.teachable.com/p/android-xmpp-chat-app-video-tutorial)

<a href="https://blikoon.teachable.com/p/android-xmpp-smack-chat-app-sending-and-receiving-files-http-file-upload">**Follow up course on Sending and Receiving Files with XMPP and Smack also available!**</a>
{: .notice--info}

## XMPP Server Options

You probably know by now that xmpp applications follow the Client Server design architecture.If you don’t ,please have a look at this XMPP introduction guide where I go through the basics of xmpp.In this section we focus on the options available to you when it comes to using an XMPP server.You basically have two choises :

### (1) You can use one of the publicly available xmpp servers

There are a good number of publicly available xmpp severs out there that you can just use.They usually provide a web interface that you use to register and when the registration is successful you can use any xmpp client to connect to the server and start exchanging messages .I find this list maintained by the XMPP Standard Foundation pretty handy so I leave it here as a reference.This list available at jabber.at can also prove to be useful.This option is the easiest as you don’t have to worry about the server details and you can fully focus on client development.This is the option we choose in this tutorial.

### (2) You can install and maintain your own XMPP server

There are a lot of xmpp server implementations out there both commercial and open source.XMPP.ORG does a good job of maintaining a regularly updated list you can look at.If you are interested in installing ejabberd on your linux platform I have tutorials on how to install both from source code and binary packages.

## Building the Android User Interface

The app workflow is simple .When the user starts the app , a login user interface is shown , the user types in the Jabber ID (JID) and the password and signs in.Once in , another piece of ui is shown with a list of contacts.Once you click on one contact ,a chat ui is shown where you can type in and send messages and see messages you receive from the other party.The design looks as shown in the figure below.

<img src="{{ site.url }}{{ site.baseurl }}/images/xmpp-smack-post/Rooster_app_design_processed.png" alt="Android Ui">

## The Login Activity

Fire up Android Studio and create a new project.I name mine Rooster ,well because I love roosters and save it in the location of your choice.

<img src="{{ site.url }}{{ site.baseurl }}/images/xmpp-smack-post/android_studio_new_project_processed.png" alt="Android Studio New Project">

Click next and in the following dialog ,choose the minimun sdk .We choose API 16 as it allows us to target a reasonable amount of android devices out there.

<img src="{{ site.url }}{{ site.baseurl }}/images/xmpp-smack-post/rooster_mininum_sdk_processed1.png" alt="Android Minimum SDK">

Click next and the next dialog lets you choose the first UI your users see when they start your app.

<img src="{{ site.url }}{{ site.baseurl }}/images/xmpp-smack-post/rooster_login_activity_processed.png" alt="Rooster Login">

We obviously choose the Login Activity as it closely matches the design we want to implement by giving us ready to use data input fields and some error checking code as you will see shortly.Click next ,choose the names for your activity and layout files,you can leave in the defaults names.Click finish and see android studio generating your project.The generated project is shown below for reference.

<img src="{{ site.url }}{{ site.baseurl }}/images/xmpp-smack-post/rooster_android_studio_generated_project_processed.png" alt="Android Studio Generated Project">

The design view of our layout file has almost everything we need in place as shown below.

<img src="{{ site.url }}{{ site.baseurl }}/images/xmpp-smack-post/rooster_login_layout_design_view_processed.png" alt="Android Studio Layout Design View">


We simply need to change the text in those fields to match what we need in Rooster.Email should be changed to Jabber Id ,our password is not optional and finally the button should only say Sign in.You apply these changes in the strings.xml file located in the res/values/ folder of your resource files as shown below.

<img src="{{ site.url }}{{ site.baseurl }}/images/xmpp-smack-post/rooster_edit_strings_file_processed.png" alt="Android Studio Edit Strings">

Another useful thing to do now is to rename the names for our strings.The string “prompt_email” should be renamed “prompt_jid”. Other relevant strings should follow the same logic.In android studio there is a way to do that really fast.Click on the string you wish to edit and right click.In the context menu that shows up click Refactor and then rename as shown below.

<img src="{{ site.url }}{{ site.baseurl }}/images/xmpp-smack-post/rooster_strings_rename_processed.png" alt="Renaming Strings">

Click refactor and the string name is updated throughout your entire project.Repeat the same process for “error_invalid_email” and rename it “error_invalid_jid”. The layout for our project is ready.Run the app on a virtual device( or a real device if you choose so) and you will see the login interface as shown below.

<img src="{{ site.url }}{{ site.baseurl }}/images/xmpp-smack-post/rooster_login_ui_processed.png" alt="Login Ui">


You can see that our updates strings have taken effect.Also notice that when we typed in a wrong JID ( a valid jid should be of the form user@server.com) ,the ui shows a friendly message saying that our jid is invalid.This error checking mechanism is controlled in LoginActivity.java that we are exploring next.

LoginActivity.java implements the logic behind our layout design.It is the brains of our activity.In the particular case or Rooster ,its most important responsibility is to fire up the login mechanism.On top of that ,it performs error checking on the data input by the user.But before we see how that happens ,we need to do some clean up.

Right now ,LoginActivity.java is trying to do two things we don’t need.One is doing email autocomplete as the user types in ,and the other is trying to simulate the login logic.These are great when you need them but for now ,… thanks Android Studio ,we are getting read of all the code that does that.The updated LoginActivity.java code is shown below for reference.

```java

package com.blikoon.rooster;

import android.animation.Animator;


import static android.Manifest.permission.READ_CONTACTS;

/**
 * A login screen that offers login via jid/password.
 */
public class LoginActivity extends AppCompatActivity
{
    /**
     * Id to identity READ_CONTACTS permission request.
     */
    private static final int REQUEST_READ_CONTACTS = 0;
    

    // UI references.
    private AutoCompleteTextView mJidView;
    private EditText mPasswordView;
    private View mProgressView;
    private View mLoginFormView;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_login);
        // Set up the login form.
        mJidView = (AutoCompleteTextView) findViewById(R.id.email);
        populateAutoComplete();

        mPasswordView = (EditText) findViewById(R.id.password);
        mPasswordView.setOnEditorActionListener(new TextView.OnEditorActionListener() {
            @Override
            public boolean onEditorAction(TextView textView, int id, KeyEvent keyEvent) {
                if (id == R.id.login || id == EditorInfo.IME_NULL) {
                    attemptLogin();
                    return true;
                }
                return false;
            }
        });

        Button mJidSignInButton = (Button) findViewById(R.id.email_sign_in_button);
        mJidSignInButton.setOnClickListener(new OnClickListener() {
            @Override
            public void onClick(View view) {
                attemptLogin();
            }
        });

        mLoginFormView = findViewById(R.id.login_form);
        mProgressView = findViewById(R.id.login_progress);
    }

    private void populateAutoComplete() {
        if (!mayRequestContacts()) {
            return;
        }

        //getLoaderManager().initLoader(0, null, this);
    }

    private boolean mayRequestContacts() {
        if (Build.VERSION.SDK_INT < Build.VERSION_CODES.M) {
            return true;
        }
        if (checkSelfPermission(READ_CONTACTS) == PackageManager.PERMISSION_GRANTED) {
            return true;
        }
        if (shouldShowRequestPermissionRationale(READ_CONTACTS)) {
            Snackbar.make(mJidView, R.string.permission_rationale, Snackbar.LENGTH_INDEFINITE)
                    .setAction(android.R.string.ok, new View.OnClickListener() {
                        @Override
                        @TargetApi(Build.VERSION_CODES.M)
                        public void onClick(View v) {
                            requestPermissions(new String[]{READ_CONTACTS}, REQUEST_READ_CONTACTS);
                        }
                    });
        } else {
            requestPermissions(new String[]{READ_CONTACTS}, REQUEST_READ_CONTACTS);
        }
        return false;
    }

    /**
     * Callback received when a permissions request has been completed.
     */
    @Override
    public void onRequestPermissionsResult(int requestCode, @NonNull String[] permissions,
                                           @NonNull int[] grantResults) {
        if (requestCode == REQUEST_READ_CONTACTS) {
            if (grantResults.length == 1 && grantResults[0] == PackageManager.PERMISSION_GRANTED) {
                populateAutoComplete();
            }
        }
    }


    /**
     * Attempts to sign in or register the account specified by the login form.
     * If there are form errors (invalid email, missing fields, etc.), the
     * errors are presented and no actual login attempt is made.
     */
    private void attemptLogin() {
        // Reset errors.
        mJidView.setError(null);
        mPasswordView.setError(null);

        // Store values at the time of the login attempt.
        String email = mJidView.getText().toString();
        String password = mPasswordView.getText().toString();

        boolean cancel = false;
        View focusView = null;

        // Check for a valid password, if the user entered one.
        if (!TextUtils.isEmpty(password) && !isPasswordValid(password)) {
            mPasswordView.setError(getString(R.string.error_invalid_password));
            focusView = mPasswordView;
            cancel = true;
        }

        // Check for a valid email address.
        if (TextUtils.isEmpty(email)) {
            mJidView.setError(getString(R.string.error_field_required));
            focusView = mJidView;
            cancel = true;
        } else if (!isEmailValid(email)) {
            mJidView.setError(getString(R.string.error_invalid_jid));
            focusView = mJidView;
            cancel = true;
        }

        if (cancel) {
            // There was an error; don't attempt login and focus the first
            // form field with an error.
            focusView.requestFocus();
        } else {
            // Show a progress spinner, and kick off a background task to
            // perform the user login attempt.
            showProgress(true);
            //This is where the login login is fired up.

        }
    }

    private boolean isEmailValid(String email) {
        //TODO: Replace this with your own logic
        return email.contains("@");
    }

    private boolean isPasswordValid(String password) {
        //TODO: Replace this with your own logic
        return password.length() > 4;
    }

    /**
     * Shows the progress UI and hides the login form.
     */
    @TargetApi(Build.VERSION_CODES.HONEYCOMB_MR2)
    private void showProgress(final boolean show) {
        // On Honeycomb MR2 we have the ViewPropertyAnimator APIs, which allow
        // for very easy animations. If available, use these APIs to fade-in
        // the progress spinner.
        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.HONEYCOMB_MR2) {
            int shortAnimTime = getResources().getInteger(android.R.integer.config_shortAnimTime);

            mLoginFormView.setVisibility(show ? View.GONE : View.VISIBLE);
            mLoginFormView.animate().setDuration(shortAnimTime).alpha(
                    show ? 0 : 1).setListener(new AnimatorListenerAdapter() {
                @Override
                public void onAnimationEnd(Animator animation) {
                    mLoginFormView.setVisibility(show ? View.GONE : View.VISIBLE);
                }
            });

            mProgressView.setVisibility(show ? View.VISIBLE : View.GONE);
            mProgressView.animate().setDuration(shortAnimTime).alpha(
                    show ? 1 : 0).setListener(new AnimatorListenerAdapter() {
                @Override
                public void onAnimationEnd(Animator animation) {
                    mProgressView.setVisibility(show ? View.VISIBLE : View.GONE);
                }
            });
        } else {
            // The ViewPropertyAnimator APIs are not available, so simply show
            // and hide the relevant UI components.
            mProgressView.setVisibility(show ? View.VISIBLE : View.GONE);
            mLoginFormView.setVisibility(show ? View.GONE : View.VISIBLE);
        }
    }
}

```

In the onCreate override method ,we setup our activity to use the activity_login.xml layout file,get reference to the UI components ,namely the Jid field ,the password field and the Sign in Button.We set a click listener to the Button and when the button is clicked ,we call the attemptLogin() method.

This function first checks to see that the jid and the password are valid.A jid is valid if it contains one “@” character and passwords of more than four characters are supported.This is a simple policy that comes by default with the code that Android Studio has generated for us.We leave it that way for this tutorial.

Just to check that our code is working as expected lets add a simple log statement inside the attemptLogin() method that gets executed when the jid and password typed in by the user are all valid.

```java

private void attemptLogin() {
       ...

    if (cancel) {
        // There was an error; don't attempt login and focus the first
        // form field with an error.
        focusView.requestFocus();
    } else {
        showProgress(true);
        //This is where the login login is fired up.
        Log.d(TAG,"Jid and password are valid ,proceeding with login.");

    }
}

```


If we run the app ,we can see the debug output when the user has input a valid jid like user@server.com and a password of at least five characters.

<img src="{{ site.url }}{{ site.baseurl }}/images/xmpp-smack-post/rooster_login_log_outupt_prefered.png" alt="Login">


In the left side of the figure ,you can see the spinning wheel that signals to the user that something is going on.At this point ,we are done with the login ui.

## The Contact List Activity.

Add a new activity to your project and name it ContactListActivity as shown below.

<img src="{{ site.url }}{{ site.baseurl }}/images/xmpp-smack-post/roster_create_contact_list_activity_processed.png" alt="Create Contact List Activity">

Click finish and your activity is created.This activity will use an implementation of List View android provides called RecyclerView.We won’t go into the details of how RecyclerViews work ,if you find them confusing somehow ,please check out a previous tutorial of mine that goes into the details of how it works.

Before using the RecyclerView ,you need to add it as a dependancy to your project.To load it hit File->Project Structure.The Project Structure dialog comes up as shown below.

<img src="{{ site.url }}{{ site.baseurl }}/images/xmpp-smack-post/android_project_structure_1.png" alt="Create Contact List Activity">

Choose app in the left pane ,click on the green plus button ,in the dialog that pops up ,scoll down until you see a line containing recyclerview-v7 as shown below.

<img src="{{ site.url }}{{ site.baseurl }}/images/xmpp-smack-post/android_recyclerview_dependency.png" alt="RecyclerView Dependency">

Hit the OK button twice and Android should load your library in a few seconds.

Modify your activity_contact_list.xml file to add a recyclerView as follows.

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:paddingBottom="@dimen/activity_vertical_margin"
    android:paddingLeft="@dimen/activity_horizontal_margin"
    android:paddingRight="@dimen/activity_horizontal_margin"
    android:paddingTop="@dimen/activity_vertical_margin"
    tools:context="com.blikoon.rooster.ContactListActivity">

    <android.support.v7.widget.RecyclerView
        android:id="@+id/contact_list_recycler_view"
        android:layout_width="match_parent"
        android:layout_height="match_parent">

    </android.support.v7.widget.RecyclerView>

</RelativeLayout>

```

Our Recycler view will need a model to operate on ,we add two Java classes to take care of that : Contact and ContactModel.

#### File : Contact.java
```java
package com.blikoon.rooster;

/**
 * Created by gakwaya on 4/16/2016.
 */
public class Contact {
    private String jid;
  
    public Contact(String contactJid )
    {
        jid = contactJid;
    }
    
    public String getJid()
    {
        return jid;
    }

    public void setJid(String jid) {
        this.jid = jid;
    }
}

```

The class is very simple.It only has only one member variable to store the contact jid.Other variables may be added later as need arises.

#### File : ContactModel.java

```java

package com.blikoon.rooster;

import android.content.Context;

import java.util.ArrayList;
import java.util.List;

/**
 * Created by gakwaya on 4/16/2016.
 */
public class ContactModel {

    private static ContactModel sContactModel;
    private List<Contact> mContacts;

    public static ContactModel get(Context context)
    {
        if(sContactModel == null)
        {
            sContactModel = new ContactModel(context);
        }
        return  sContactModel;
    }

    private ContactModel(Context context)
    {
        mContacts = new ArrayList<>();
        populateWithInitialContacts(context);

    }

    private void populateWithInitialContacts(Context context)
    {
        //Create the Foods and add them to the list;

      
        Contact contact1 = new Contact("User1@server.com");
        mContacts.add(contact1);
        Contact contact2 = new Contact("User2@server.com");
        mContacts.add(contact2);
        Contact contact3 = new Contact("User3@server.com");
        mContacts.add(contact3);
        Contact contact4 = new Contact("User4@server.com");
        mContacts.add(contact4);
        Contact contact5 = new Contact("User5@server.com");
        mContacts.add(contact5);
        Contact contact6 = new Contact("User6@server.com");
        mContacts.add(contact6);
        Contact contact7 = new Contact("User7@server.com");
        mContacts.add(contact7);
    }

    public List<Contact> getContacts()
    {
        return mContacts;
    }
}

```

ContactModel implements the singleton .Our application only needs one instance of this class as it will work on the same contact data set.With our model in place ,we can now set up a ViewHolder and an Adapter inside the ContactListActivity class.But before we do that we need to define how every single contact item is going to look in the list view.Add a new layout file in the /res/layout folder and name it list_item_contact and make sure its contents are as follows.

#### File : list_item_contact.xml

```xml

<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
              android:orientation="vertical"
              android:layout_width="match_parent"
              android:layout_height="match_parent">

    <TextView
        android:id="@+id/contact_jid"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:gravity="center"
        android:text="user@example.com"
        android:textSize="35dp"/>


</LinearLayout>

```

Next we add a ContactHolder inner class to ContactListActivity as shown below.

```java
public class ContactListActivity extends AppCompatActivity {

    private static final String TAG = "ContactListActivity";

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_contact_list);
    }



    private class ContactHolder extends RecyclerView.ViewHolder
    {
        private TextView contactTextView;
        private Contact mContact;
        public ContactHolder ( View itemView)
        {
            super(itemView);

            contactTextView = (TextView) itemView.findViewById(R.id.contact_jid);

            itemView.setOnClickListener(new View.OnClickListener() {
                @Override
                public void onClick(View v) {
                    //Inside here we start the chat activity

                }
            });
        }


        public void bindContact( Contact contact)
        {
            mContact = contact;
            if (mContact == null)
            {
                Log.d(TAG,"Trying to work on a null Contact object ,returning.");
                return;
            }
            contactTextView.setText(mContact.getJid());

        }
    }
}

```

ContactHolder embodies the looks of each Contact item in the list of Contacts that RecyclerView contains.It also provides a public method that the adapter is going to use to set the text shown in each item.We add the adapter class next.ContactAdapter is also added as an inner class as shown below.

```java
private class ContactAdapter extends RecyclerView.Adapter<ContactHolder>
{
    private List<Contact> mContacts;

    public ContactAdapter( List<Contact> contactList)
    {
        mContacts = contactList;
    }

    @Override
    public ContactHolder onCreateViewHolder(ViewGroup parent, int viewType) {

        LayoutInflater layoutInflater = LayoutInflater.from(parent.getContext());
        View view = layoutInflater
                .inflate(R.layout.list_item_contact, parent,
                        false);
        return new ContactHolder(view);
    }

    @Override
    public void onBindViewHolder(ContactHolder holder, int position) {
        Contact contact = mContacts.get(position);
        holder.bindContact(contact);

    }

    @Override
    public int getItemCount() {
        return mContacts.size();
    }
}

```

It overrides the three methods a RecyclerView’s adapter needs to operate properly.For more on these methods please check the tutorial specific to the [LINK]RecyclerView.With the ViewHolder and the Adapter in place ,we can add final touches to our ContactListActivity by defining its RecyclerView , defining the data model and applying it to the adapter and finaly let the RecyclerView use our adapter.The details are shown below.

#### File : ContactListActivity.java

```java
package com.blikoon.rooster;

import android.content.Intent;
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.support.v7.widget.LinearLayoutManager;
import android.support.v7.widget.RecyclerView;
import android.util.Log;
import android.view.LayoutInflater;
import android.view.View;
import android.view.ViewGroup;
import android.widget.TextView;

import java.util.List;

public class ContactListActivity extends AppCompatActivity {

    private static final String TAG = "ContactListActivity";

    private RecyclerView contactsRecyclerView;
    private ContactAdapter mAdapter;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_contact_list);

        contactsRecyclerView = (RecyclerView) findViewById(R.id.contact_list_recycler_view);
        contactsRecyclerView.setLayoutManager(new LinearLayoutManager(getBaseContext()));

        ContactModel model = ContactModel.get(getBaseContext());
        List<Contact> contacts = model.getContacts();

        mAdapter = new ContactAdapter(contacts);
        contactsRecyclerView.setAdapter(mAdapter);
    }



    private class ContactHolder extends RecyclerView.ViewHolder
    {
       ...
    }


    private class ContactAdapter extends RecyclerView.Adapter<ContactHolder>
    {
       ...
    }
}

```

How do we test that ContactListActivity is working correctly?For now we can start it when the user clicks on the SignIn button from LoginActivity.Change your attemptLogin() method as follows.

```java
private void attemptLogin() {
    ...

    if (cancel) {
        // There was an error; don't attempt login and focus the first
        // form field with an error.
        focusView.requestFocus();
    } else {
        // Show a progress spinner, and kick off a background task to
        // perform the user login attempt.
        //showProgress(true);<<---FOR NOW WE DON'T WANT TO SEE THIS PROGRESS THING.
        //This is where the login login is fired up.
        Log.d(TAG,"Jid and password are valid ,proceeding with login.");
        startActivity(new Intent(this,ContactListActivity.class));

    }
}

```

Run the app ,type in a jid and a password and hit login .Our shiny ContactListActivity should show up as shown below.

<img src="{{ site.url }}{{ site.baseurl }}/images/xmpp-smack-post/rooster_contact_list_activity_test_processed.png" alt="Test App">

yes the list looks like it could use some spacing but that’s an easy fix by specifying some reasonable height for the TextView inside the list_item_contact.xml layout file.

## Building the ChatActivity.

Building a good looking chat activity is a bit involved.Luckily somebody has already done the heavy lifting for us and we are just going to build on top of their work.Add a new activity to the project and name it ChatActivity.  The library in this github [LINK]repo provides a good library we can use to build our chat activity.Just follow the instructions they give on how to use it.Add

```xml
implementation 'com.github.timigod:android-chat-ui:v0.1.3'
```
to your build.gradle file (Module:app) as shown below and sync the project.

The “ext” block allows us to control the versions of the libraries we import in one place.After your project finishes syncing ,you can now use the components from the library, we define the supportLibVersion variable which is used down in the dependencies block.The android-chat-ui library is going to give us a ready to use Chat Activity.Modify the activity_chat.xml layout file as shown below to add a ChatView to ChatActivity.

#### File : activity_chat.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:paddingBottom="@dimen/activity_vertical_margin"
    android:paddingLeft="@dimen/activity_horizontal_margin"
    android:paddingRight="@dimen/activity_horizontal_margin"
    android:paddingTop="@dimen/activity_vertical_margin"
    tools:context="com.blikoon.rooster.ChatActivity">

    <co.devcenter.androiduilibrary.ChatView
        android:id="@+id/rooster_chat_view"
        android:layout_width="match_parent"
        android:layout_height="match_parent">

    </co.devcenter.androiduilibrary.ChatView>

</RelativeLayout>
```

At this moment ,you can see your ChatActivity and interact with it in the app.To see it in action ,we are going to start the ChatActivity whenever a user presses any item in ContactListActivity.ContactListActivity will also pass along the Jid of the item that has been clicked.Modify the clickListener of ContactHolder inside ContactListActivity as shown below.

```java
private class ContactHolder extends RecyclerView.ViewHolder
{
    private TextView contactTextView;
    private Contact mContact;
    public ContactHolder ( View itemView)
    {
        super(itemView);

        contactTextView = (TextView) itemView.findViewById(R.id.contact_jid);

        itemView.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                //Inside here we start the chat activity
                Intent intent = new Intent(ContactListActivity.this
                        ,ChatActivity.class);
                intent.putExtra("EXTRA_CONTACT_JID",mContact.getJid());
                startActivity(intent);


            }
        });
    }


   .....
}
```

We pass the current item’s Jid to ChatActivity as an intent extra.We also take this chance to modify the list_item_contact.xml layout file so there is some breathing room between items.


```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
              android:orientation="vertical"
              android:layout_width="match_parent"
              android:layout_height="match_parent">

    <TextView
        android:id="@+id/contact_jid"
        android:layout_width="match_parent"
        android:layout_height="70dp"
        android:gravity="center"
        android:text="user@example.com"
        android:textSize="35dp"/>

</LinearLayout>
```
Finally ,modify ChatActivity so it retrieves the current contact jid and shows it as the title label of the activity.

```java
package com.blikoon.rooster;

public class ChatActivity extends AppCompatActivity {
    private ChatView mChatView;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_chat);
        mChatView =(ChatView) findViewById(R.id.rooster_chat_view);

        Intent intent = getIntent();
        String contactJid = intent.getStringExtra("EXTRA_CONTACT_JID");
        setTitle(contactJid);
    }
}
```
Run the app ,click on one item and you should see your ChatActivity as shown below.

<img src="{{ site.url }}{{ site.baseurl }}/images/xmpp-smack-post/rooster_chat_activity_preview_processed.png" alt="Login">

The label is showing the current contact’s jid and right at the bottom you can see the text field where the user types in the message and the blue send button at the bottom right.The ChatActivity is not doing anything interesting for the moment but as shown at the librarie’s github page it can be customized to do more stuff like showing messages you send and receive and responding to the send button click.We will be using these when we start messing around with Smack to send and receive XMPP messages.Our UI work is done for now.Next we’ll see how to get our shiny Rooster to connect to an XMPP server.

## Connecting to the XMPP Server.

### Adding Smack dependencies to your Android Studio project.

To be able to use Smack in our project ,we need to import it in our android studio project.To do that you add the following lines of code to the build.gradle file of your app module as shown in the figure below. The “ext” block allows us to have the Smack Version defined in one place and for it to be picked up wherever it is needed. We import mack-tcp, smack-experimental and smack-android as shown in the lower section of the image below.

<img src="{{ site.url }}{{ site.baseurl }}/images/xmpp-smack-post/smack4.2.3_good.png" alt="Smack Dependency">

With this in place you can start importing smack classes and using them.But before we do that ,we need to understand something important about how android works.

### A word about android Threads and Services.

In android ,it is recommended to run all your networking code in background threads.This is because such long running operations can cause your UI to freeze or even worse ,show a message like “Rooster is not responding do you want to close it?”. You have seen one of these right?So we need our code that connects to and XMPP Server to run in a background thread.

Another thing we want is for our app to stick around even the user navigates away from it so when messages come in we can notify the user by a beep sound , a notification or whatever you see fit.For that to happen the code needs to run inside a Service.You can think of a service like a wrapper that contains code that keeps running even if the activity that started it dies and disappears.But Services run on the UI thread by default ; so to get the behavior we want for our app ,XMPP connection code has to live inside a background thread that is hosted by a Service.The structure of our code can be summarized in the figure shown below.

<img src="{{ site.url }}{{ site.baseurl }}/images/xmpp-smack-post/rooster_connection_thread_service_model_processed.png" alt="Thread Service Model">

Now we can get back to the real business .Add a new class to your project and name it RoosterConnectionService.You guessed it ,we are creating the service first.Add in the code as shown below.

#### File : RoosterConnectionService.java

```java
package com.blikoon.rooster;

import android.app.Service;
import android.content.Intent;
import android.os.Handler;
import android.os.IBinder;
import android.os.Looper;
import android.support.annotation.Nullable;
import android.util.Log;

/**
 * Created by gakwaya on 4/28/2016.
 */
public class RoosterConnectionService extends Service {
    private static final String TAG ="RoosterService";
    private boolean mActive;//Stores whether or not the thread is active
    private Thread mThread;
    private Handler mTHandler;//We use this handler to post messages to
    //the background thread.
    
    public RoosterConnectionService() {

    }

    @Nullable
    @Override
    public IBinder onBind(Intent intent) {
        return null;
    }

    @Override
    public void onCreate() {
        super.onCreate();
        Log.d(TAG,"onCreate()");
    }


    public void start()
    {
        Log.d(TAG," Service Start() function called.");
        if(!mActive)
        {
            mActive = true;
            if( mThread ==null || !mThread.isAlive())
            {
                mThread = new Thread(new Runnable() {
                    @Override
                    public void run() {

                        Looper.prepare();
                        mTHandler = new Handler();
                        //initConnection();
                        //THE CODE HERE RUNS IN A BACKGROUND THREAD.
                        Looper.loop();

                    }
                });
                mThread.start();
            }


        }


    }

    public void stop()
    {
        Log.d(TAG,"stop()");
        mActive = false;
        mTHandler.post(new Runnable() {
            @Override
            public void run() {
                if( mConnection != null)
                  //CODE HERE IS MEANT TO SHUT DOWN OUR CONNECTION TO THE SERVER.
            }
        });

    }

    
    @Override
    public int onStartCommand(Intent intent, int flags, int startId) {
        Log.d(TAG,"onStartCommand()");
        start();
        return Service.START_STICKY;
        //RETURNING START_STICKY CAUSES OUR CODE TO STICK AROUND WHEN THE APP ACTIVITY HAS DIED.
    }

    @Override
    public void onDestroy() {
        Log.d(TAG,"onDestroy()");
        super.onDestroy();
        stop();
    }
}
```
In android when you start a service , its onStartCommand override function is called.Inside our onStartCommand() function we initialize our mThread variable and pass it a Runnable whose run() method will contain the code that runs in the background thread.I want you to pay particular attention to the code snippet below if you still find this confusing.

```java
mThread = new Thread(new Runnable() {
    @Override
    public void run() {

        Looper.prepare();
        mTHandler = new Handler();
        //initConnection();
        //THE CODE HERE RUNS IN A BACKGROUND THREAD.
        Looper.loop();

    }
});
mThread.start();
```

Every thread in android has something called a Looper that keeps looping around waiting for things to happen.It is called an event loop in some other technologies.Lopper.prepare() and Looper.loop() are kind of making sure we have that kind of thing.You can read more about these methods in android docs .The code that we want to run in the background thread has to live between these two function calls.

When you want to tell a thread to do something from another thread ,you use a thing called a Handler.For every thread corresponds one handler.The handler operates on the thread it was defined into.For exmple mTHandler was defined inside mThread ,so it can be used to pass work to mThread.After we setup the thread it is started.

Another thing you should notice is that onStartCommand returns START_STICKY.This effectively tells our service to run forever until someone or something explicitly stops it.

Next ,we layout the details of how we connect to an XMPP Server.Add a new class to your project and name it RoosterConnection.The initial code of the class is shown below.

#### File : RoosterConnection.java

```java
package com.blikoon.rooster;

import android.content.Context;
import android.preference.PreferenceManager;
import android.util.Log;

import org.jivesoftware.smack.ConnectionListener;
import org.jivesoftware.smack.ReconnectionManager;
import org.jivesoftware.smack.SmackException;
import org.jivesoftware.smack.XMPPConnection;
import org.jivesoftware.smack.XMPPException;
import org.jivesoftware.smack.tcp.XMPPTCPConnection;
import org.jivesoftware.smack.tcp.XMPPTCPConnectionConfiguration;

import java.io.IOException;

/**
 * Created by gakwaya on 4/28/2016.
 */
public class RoosterConnection implements ConnectionListener {

    private static final String TAG = "RoosterConnection";

    private  final Context mApplicationContext;
    private  final String mUsername;
    private  final String mPassword;
    private  final String mServiceName;
    private XMPPTCPConnection mConnection;


    public static enum ConnectionState
    {
        CONNECTED ,AUTHENTICATED, CONNECTING ,DISCONNECTING ,DISCONNECTED;
    }

    public static enum LoggedInState
    {
        LOGGED_IN , LOGGED_OUT;
    }


    public RoosterConnection( Context context)
    {
        Log.d(TAG,"RoosterConnection Constructor called.");
        mApplicationContext = context.getApplicationContext();
        String jid = PreferenceManager.getDefaultSharedPreferences(mApplicationContext)
                .getString("xmpp_jid",null);
        mPassword = PreferenceManager.getDefaultSharedPreferences(mApplicationContext)
                .getString("xmpp_password",null);

        if( jid != null)
        {
            mUsername = jid.split("@")[0];
            mServiceName = jid.split("@")[1];
        }else
        {
            mUsername ="";
            mServiceName="";
        }
    }


    public void connect() throws IOException,XMPPException,SmackException
    {
        Log.d(TAG, "Connecting to server " + mServiceName);
        XMPPTCPConnectionConfiguration.XMPPTCPConnectionConfigurationBuilder builder=
                XMPPTCPConnectionConfiguration.builder();
        builder.setServiceName(mServiceName);
        builder.setUsernameAndPassword(mUsername, mPassword);
        builder.setRosterLoadedAtLogin(true);
        builder.setResource("Rooster");

        //Set up the ui thread broadcast message receiver.
        //setupUiThreadBroadCastMessageReceiver();

        mConnection = new XMPPTCPConnection(builder.build());
        mConnection.addConnectionListener(this);
        mConnection.connect();
        mConnection.login();

        ReconnectionManager reconnectionManager = ReconnectionManager.getInstanceFor(mConnection);
        reconnectionManager.setEnabledPerDefault(true);
        reconnectionManager.enableAutomaticReconnection();

    }



    public void disconnect()
    {
        Log.d(TAG,"Disconnecting from serser "+ mServiceName);
        try
        {
            if (mConnection != null)
            {
                mConnection.disconnect();
            }

        }catch (SmackException.NotConnectedException e)
        {
            RoosterConnectionService.sConnectionState=ConnectionState.DISCONNECTED;
            e.printStackTrace();

        }
        mConnection = null;


    }


    @Override
    public void connected(XMPPConnection connection) {
        RoosterConnectionService.sConnectionState=ConnectionState.CONNECTED;
        Log.d(TAG,"Connected Successfully");
    }

    @Override
    public void authenticated(XMPPConnection connection) {
        RoosterConnectionService.sConnectionState=ConnectionState.CONNECTED;
        Log.d(TAG,"Authenticated Successfully");

    }

    @Override
    public void connectionClosed() {
        RoosterConnectionService.sConnectionState=ConnectionState.DISCONNECTED;
        Log.d(TAG,"Connectionclosed()");

    }

    @Override
    public void connectionClosedOnError(Exception e) {
        RoosterConnectionService.sConnectionState=ConnectionState.DISCONNECTED;
        Log.d(TAG,"ConnectionClosedOnError, error "+ e.toString());

    }

    @Override
    public void reconnectingIn(int seconds) {
        RoosterConnectionService.sConnectionState = ConnectionState.CONNECTING;
        Log.d(TAG,"ReconnectingIn() ");

    }

    @Override
    public void reconnectionSuccessful() {
        RoosterConnectionService.sConnectionState = ConnectionState.CONNECTED;
        Log.d(TAG,"ReconnectionSuccessful()");

    }

    @Override
    public void reconnectionFailed(Exception e) {
        RoosterConnectionService.sConnectionState = ConnectionState.DISCONNECTED;
        Log.d(TAG,"ReconnectionFailed()");

    }
}
```
The class implements the ConnectionListener interface to allow us to listen in on the connection status events.We override the event status methods reproduced below for convenience.

```java
@Override
    //CALLED WHEN WE ARE SUCCESSFULY CONNECTED TO THE SERVER.
    public void connected(XMPPConnection connection) {
        RoosterConnectionService.sConnectionState=ConnectionState.CONNECTED;
        Log.d(TAG,"Connected Successfully");
    }

    @Override
    //CALLED WHEN WE ARE SUCCESSFULY AUTHENTICATED WITH THE SERVER
    public void authenticated(XMPPConnection connection) {
        RoosterConnectionService.sConnectionState=ConnectionState.CONNECTED;
        Log.d(TAG,"Authenticated Successfully");

    }

    @Override
    //CALLED WHEN THE CONNECTION IS CLOSED FOR SOME REASON
    public void connectionClosed() {
        RoosterConnectionService.sConnectionState=ConnectionState.DISCONNECTED;
        Log.d(TAG,"Connectionclosed()");

    }

    @Override
    //CALLED WHEN THE CONNECTION IS CLOSED AS A RESULT OF SOME ERROR.
    public void connectionClosedOnError(Exception e) {
        RoosterConnectionService.sConnectionState=ConnectionState.DISCONNECTED;
        Log.d(TAG,"ConnectionClosedOnError, error "+ e.toString());

    }

    @Override
    //CALLED WHEN THE APP IS TRYING TO RECONNECT 
    public void reconnectingIn(int seconds) {
        RoosterConnectionService.sConnectionState = ConnectionState.CONNECTING;
        Log.d(TAG,"ReconnectingIn() ");

    }

    @Override
    //CALLED WHEN THE APP SUCCESSFULLY RECONNECTS TO THE SERVER
    public void reconnectionSuccessful() {
        RoosterConnectionService.sConnectionState = ConnectionState.CONNECTED;
        Log.d(TAG,"ReconnectionSuccessful()");

    }

    @Override
    //CALLED WHEN RECONNECTION HAS FAILED.
    public void reconnectionFailed(Exception e) {
        RoosterConnectionService.sConnectionState = ConnectionState.DISCONNECTED;
        Log.d(TAG,"ReconnectionFailed()");

    }
```
You respond to these events inside these methods.In our case we simply set the connection status.Besides these override methods ,we also have a constructor method that simply retrieves the jid and password from Shared Preferences.

The connect() method handles all the heavy lifting to connect to the server.

```java
public void connect() throws IOException,XMPPException,SmackException
{
    Log.d(TAG, "Connecting to server " + mServiceName);
    XMPPTCPConnectionConfiguration.XMPPTCPConnectionConfigurationBuilder builder=
            XMPPTCPConnectionConfiguration.builder();
    builder.setServiceName(mServiceName);
    builder.setUsernameAndPassword(mUsername, mPassword);
    builder.setRosterLoadedAtLogin(true);
    builder.setResource("Rooster");

    //Set up the ui thread broadcast message receiver.
    //setupUiThreadBroadCastMessageReceiver();

    mConnection = new XMPPTCPConnection(builder.build());
    mConnection.addConnectionListener(this);
    mConnection.connect();
    mConnection.login();

    ReconnectionManager reconnectionManager = ReconnectionManager.getInstanceFor(mConnection);
    reconnectionManager.setEnabledPerDefault(true);
    reconnectionManager.enableAutomaticReconnection();

}
```

XMPPTCPConnectionConfiguration helps wrap the connection data into one object.In our case it stores the username ,the password ,the server domain and resource of the XMPP client.We also use it to specify that the roster is loaded at login( ignore this if you don’t know what a roster is).

We then initialise our XMPPTCPConnection object passing in the configuration .We setup its ConnectionListener ,connect and login into the server.The disonnect method simply …disconnects from the server

```java
public void disconnect()
    {
        Log.d(TAG,"Disconnecting from serser "+ mServiceName);
        try
        {
            if (mConnection != null)
            {
                mConnection.disconnect();
            }

        }catch (SmackException.NotConnectedException e)
        {
            RoosterConnectionService.sConnectionState=ConnectionState.DISCONNECTED;
            e.printStackTrace();

        }
        mConnection = null;


    }
```

Now we need to modify our RoosterConnectionService class to use the RoosterConnection class to connect to the server.The changes are highlighted below.

#### File : RoosterConnectionService.java

```java
package com.blikoon.rooster;


 
public class RoosterConnectionService extends Service {
    private static final String TAG ="RoosterService";

    public static RoosterConnection.ConnectionState sConnectionState;
    public static RoosterConnection.LoggedInState sLoggedInState;
    private boolean mActive;//Stores whether or not the thread is active
    private Thread mThread;
    private Handler mTHandler;//We use this handler to post messages to
    //the background thread.
    private RoosterConnection mConnection;

    public RoosterConnectionService() {

    }
    public static RoosterConnection.ConnectionState getState()
    {
        if (sConnectionState == null)
        {
            return RoosterConnection.ConnectionState.DISCONNECTED;
        }
        return sConnectionState;
    }

    public static RoosterConnection.LoggedInState getLoggedInState()
    {
        if (sLoggedInState == null)
        {
            return RoosterConnection.LoggedInState.LOGGED_OUT;
        }
        return sLoggedInState;
    }

   ...

    private void initConnection()
    {
        Log.d(TAG,"initConnection()");
        if( mConnection == null)
        {
            mConnection = new RoosterConnection(this);
        }
        try
        {
            mConnection.connect();

        }catch (IOException |SmackException |XMPPException e)
        {
            Log.d(TAG,"Something went wrong while connecting ,make sure the credentials are right and try again");
            e.printStackTrace();
            //Stop the service all together.
            stopSelf();
        }

    }


    public void start()
    {
        Log.d(TAG," Service Start() function called.");
        if(!mActive)
        {
            mActive = true;
            if( mThread ==null || !mThread.isAlive())
            {
                mThread = new Thread(new Runnable() {
                    @Override
                    public void run() {

                        Looper.prepare();
                        mTHandler = new Handler();
                        initConnection();
                        //THE CODE HERE RUNS IN A BACKGROUND THREAD.
                        Looper.loop();

                    }
                });
                mThread.start();
            }


        }

    }

    public void stop()
    {
        Log.d(TAG,"stop()");
        mActive = false;
        mTHandler.post(new Runnable() {
            @Override
            public void run() {
                if( mConnection != null)
                {
                    mConnection.disconnect();
                }
            }
        });

    }
     ...
}
```

We added asConnectionState and sLoggedInState static variables that we will use to track down the connection state and their respective getter functions.Also we add a RoosterConnection object that is initialized in a new added function called initConnection().The function simply calls connect() on the RoosterConnection object.

Inside the start() method ,we simply call initConnection() to connect to the server.Notice that RoosterConnection is initialized inside the background thread that is hosted inside a service.This means that everything that RoosterConnection does happens of the android UI thread and once the service has started ,closing the app by hitting the back button or whatever is not going to interrupt its operation.Also notice that we call disconnect in the stop() method.

We can now put finishing touches to the LoginActivity class and the androidManifest.xml file of our project and see our Rooster app connect to an xmpp server for the first time!In LoginActivity class and the following function to read data from the input fields ,save the data in Shared Preferences and fire up the actual login process.

```java
private void saveCredentialsAndLogin()
{
    Log.d(TAG,"saveCredentialsAndLogin() called.");
    SharedPreferences prefs = PreferenceManager.getDefaultSharedPreferences(this);
    prefs.edit()
            .putString("xmpp_jid", mJidView.getText().toString())
            .putString("xmpp_password", mPasswordView.getText().toString())
            .putBoolean("xmpp_logged_in",true)
            .commit();

    //Start the service
    Intent i1 = new Intent(this,RoosterConnectionService.class);
    startService(i1);

}
```
As you can see the login process is started by calling startService with an intent that wraps around the RoosterConnectionService class.This causes RoosterConnectionService.onStartCommand() to be called,the method calls RoosterConnectionService.start() wich in turn creates our RoosterConnection object on a background thread and calls its connect() method.

Modify your LoginActivity.attemptLogin() method to actually call saveCredentialsAndLogin() when the user clicks on the Sign in button.

```java
private void attemptLogin() {

        // Reset errors.
        mJidView.setError(null);
        mPasswordView.setError(null);

        // Store values at the time of the login attempt.
        String email = mJidView.getText().toString();
        String password = mPasswordView.getText().toString();

        boolean cancel = false;
        View focusView = null;

        // Check for a valid password, if the user entered one.
        if (!TextUtils.isEmpty(password) && !isPasswordValid(password)) {
            mPasswordView.setError(getString(R.string.error_invalid_password));
            focusView = mPasswordView;
            cancel = true;
        }

        // Check for a valid email address.
        if (TextUtils.isEmpty(email)) {
            mJidView.setError(getString(R.string.error_field_required));
            focusView = mJidView;
            cancel = true;
        } else if (!isEmailValid(email)) {
            mJidView.setError(getString(R.string.error_invalid_jid));
            focusView = mJidView;
            cancel = true;
        }

        if (cancel) {
            // There was an error; don't attempt login and focus the first
            // form field with an error.
            focusView.requestFocus();
        } else {
            // Show a progress spinner, and kick off a background task to
            // perform the user login attempt.
            //showProgress(true);
            //This is where the login login is fired up.
//            Log.d(TAG,"Jid and password are valid ,proceeding with login.");
//            startActivity(new Intent(this,ContactListActivity.class));

            //Save the credentials and login
            saveCredentialsAndLogin();

        }
    }
```

If you run the app now and try to login nothing will happen.This is because we didn’t declare our RoosterConnectionService in the androidManifest.xml file.We also need to add the permission to use the internet.All this is taken care of by modifying the androidManifest.xml file as shown below.

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest package="com.blikoon.rooster"
          xmlns:android="http://schemas.android.com/apk/res/android">

    <uses-permission android:name="android.permission.INTERNET"/>
    <!-- To auto-complete the email text field in the login form with the user's emails -->
    <uses-permission android:name="android.permission.GET_ACCOUNTS"/>
    <uses-permission android:name="android.permission.READ_PROFILE"/>
    <uses-permission android:name="android.permission.READ_CONTACTS"/>

    <application
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:supportsRtl="true"
        android:theme="@style/AppTheme">
        <activity
            android:name=".LoginActivity"
            android:label="@string/app_name">
            <intent-filter>
                <action android:name="android.intent.action.MAIN"/>

                <category android:name="android.intent.category.LAUNCHER"/>
            </intent-filter>
        </activity>
        <activity android:name=".ContactListActivity">
        </activity>
        <activity android:name=".ChatActivity">
        </activity>
        <service android:name=".RoosterConnectionService">

        </service>

    </application>

</manifest>
```
Run the app and if everything is setup as it should ,you’ll see the login screen.Type in your Jid and password.You must have an account on some XMPP server to be able to try this.My login screen looks as shown below.

<img src="{{ site.url }}{{ site.baseurl }}/images/xmpp-smack-post/rooster_login_test_processed.png" alt="Test App">

Hit Sign in and watch your debug .You can see below that the debug output of RoosterConnection.connect() is shown and that we both connect and authenticate successfully.

<img src="{{ site.url }}{{ site.baseurl }}/images/xmpp-smack-post/rooster_connected_output_message_processed.png" alt="Rooster Connected">

If you have trouble seeing this debug output it might help to set up a filter so that output from your package is shown.In the filter combobox on the bottom left of Android Studio,choose Edit Filter Configuration and type in the package you want to filter for ,mine is com.blikoon.rooster as shown below .Type in one that matches your project and try again.

<img src="{{ site.url }}{{ site.baseurl }}/images/xmpp-smack-post/rooster_debug_filter_processed.png" alt="Debug Filter">

## Showing the ContactListActivity when the user is authenticated.

I agree that just seeing debug output when we get connected isn’t that exciting.We need to show the ContactListActivity so the user can actually start sending and receiving messages.But we can’t just create and intent that starts ContactListActivity inside the authenticated() override method of RoosterConnection.Why?Because everything RoosterConnection does lives in a background thread and we can’t start activities or more generally do anything related to the User Interface inside a background thread.Android forbids it and if you do it you will get nasty errors.

There are many ways to deal with this problem and sending and receiving broadcasts is one of them.Broadcasts are mechanism android provides to send messages that are only received by individuals who have explicitly stated that they are interested in those messages.LoginActivity is going to state that it is interested in specific messages sent by RoosterConnection and RoosterConnection is going to send one of such message when the user is successfully authenticated.

When LoginActivity receives the message it can then start ContactListActivity.Since both Activities live on the UI thread ,everything goes flawlessly.First we define the specific message in RoosterConnectionService as shown below.

```java
public class RoosterConnectionService extends Service {
    private static final String TAG ="RoosterService";
    public static final String UI_AUTHENTICATED = "com.blikoon.rooster.uiauthenticated";

   ...
}
```

Then explicitly state that we are interested in(register for) the UI_AUTHENTICATED broadcast message in LoginActivity.We register in the onResume activity lifecycle method and unregister in the onPause life cycle method.First we define the broadcast receiver.

```java
...
private View mLoginFormView;
private BroadcastReceiver mBroadcastReceiver;
private Context mContext;
....
```
and put in the onResume and onPause methods as shown below.

```java
public class LoginActivity extends AppCompatActivity
{

    ...
    private BroadcastReceiver mBroadcastReceiver;
    private Context mContext;
   ...

   

    @Override
    protected void onPause() {
        super.onPause();
        this.unregisterReceiver(mBroadcastReceiver);
    }

    @Override
    protected void onResume() {
        super.onResume();
        mBroadcastReceiver = new BroadcastReceiver() {
            @Override
            public void onReceive(Context context, Intent intent) {

                String action = intent.getAction();
                switch (action)
                {
                    case RoosterConnectionService.UI_AUTHENTICATED:
                        Log.d(TAG,"Got a broadcast to show the main app window");
                        //Show the main app window
                        showProgress(false);
                        Intent i2 = new Intent(mContext,ContactListActivity.class);
                        startActivity(i2);
                        finish();
                        break;
                }

            }
        };
        IntentFilter filter = new IntentFilter(RoosterConnectionService.UI_AUTHENTICATED);
        this.registerReceiver(mBroadcastReceiver, filter);
    }

      ...
}
```
With this in place ,we can add a method to RoosterConnection that sends the broadcast

```java
private void showContactListActivityWhenAuthenticated()
{
    Intent i = new Intent(RoosterConnectionService.UI_AUTHENTICATED);
    i.setPackage(mApplicationContext.getPackageName());
    mApplicationContext.sendBroadcast(i);
    Log.d(TAG,"Sent the broadcast that we are authenticated");
}
```

and call it when the user is authenticated

```java
@Override
public void authenticated(XMPPConnection connection) {
    RoosterConnectionService.sConnectionState=ConnectionState.CONNECTED;
    Log.d(TAG,"Authenticated Successfully");
    showContactListActivityWhenAuthenticated();

}
```

Run the app ,and type in your jid and password as usual.The ContactListActivity should show up at successful authentication.

## Sending and Receiving Messages

To be able to send messages ,we need to carry out the following changes in ChatActivity

```java
public class ChatActivity extends AppCompatActivity {
    private static final String TAG ="ChatActivity";

    private String contactJid;
    private ChatView mChatView;
    private SendButton mSendButton;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_chat);
        mChatView =(ChatView) findViewById(R.id.rooster_chat_view);
        mChatView.setEventListener(new ChatViewEventListener() {
            @Override
            public void userIsTyping() {
                //Here you know that the user is typing
            }

            @Override
            public void userHasStoppedTyping() {
                //Here you know that the user has stopped typing.
            }
        });

        mSendButton = mChatView.getSendButton();
        mSendButton.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {

                //Only send the message if the client is connected
                //to the server.

                if (RoosterConnectionService.getState().equals(RoosterConnection.ConnectionState.CONNECTED)) {
                    Log.d(TAG, "The client is connected to the server,Sendint Message");
                    //Send the message to the server

                    Intent intent = new Intent(RoosterConnectionService.SEND_MESSAGE);
                    intent.putExtra(RoosterConnectionService.BUNDLE_MESSAGE_BODY,
                            mChatView.getTypedString());
                    intent.putExtra(RoosterConnectionService.BUNDLE_TO, contactJid);

                    sendBroadcast(intent);

                    //Update the chat view.
                    mChatView.sendMessage();
                } else {
                    Toast.makeText(getApplicationContext(),
                            "Client not connected to server ,Message not sent!",
                            Toast.LENGTH_LONG).show();
                }
            }
        });

        Intent intent = getIntent();
        contactJid = intent.getStringExtra("EXTRA_CONTACT_JID");
        setTitle(contactJid);
    }
}
```

Again we have a situation of passing a message from the UI thread to the background thread ,using RoosterConnection to send the message.We use the same mechanism of broadcasts ,only now ChatActivity is sending the broadcast and RoosterConnection is receiving it.The message types you see in the clickListener of mSendButton are defined in RoosterConnectionService as shown below.

```java
private static final String TAG ="RoosterService";

public static final String UI_AUTHENTICATED = "com.blikoon.rooster.uiauthenticated";
public static final String SEND_MESSAGE = "com.blikoon.rooster.sendmessage";
public static final String BUNDLE_MESSAGE_BODY = "b_body";
public static final String BUNDLE_TO = "b_to";

public static RoosterConnection.ConnectionState sConnectionState;

RoosterConnection needs to register to receive these messages.We define the BroadCastReceiver

private XMPPTCPConnection mConnection;
private BroadcastReceiver uiThreadMessageReceiver;//Receives messages from the ui thread.
...

```
add a method the does the actual registering and sends the message to the server when the UI thread passes the message

```java
private void setupUiThreadBroadCastMessageReceiver()
{
    uiThreadMessageReceiver = new BroadcastReceiver() {
        @Override
        public void onReceive(Context context, Intent intent) {
            //Check if the Intents purpose is to send the message.
            String action = intent.getAction();
            if( action.equals(RoosterConnectionService.SEND_MESSAGE))
            {
                //SENDS THE ACTUAL MESSAGE TO THE SERVER
                sendMessage(intent.getStringExtra(RoosterConnectionService.BUNDLE_MESSAGE_BODY),
                        intent.getStringExtra(RoosterConnectionService.BUNDLE_TO));
            }
        }
    };

    IntentFilter filter = new IntentFilter();
    filter.addAction(RoosterConnectionService.SEND_MESSAGE);
    mApplicationContext.registerReceiver(uiThreadMessageReceiver,filter);

}
```
and add the actual sendMessage() method as shown below

```java
private void sendMessage ( String body ,String toJid)
{
    Log.d(TAG,"Sending message to :"+ toJid);

    EntityBareJid jid = null;


    ChatManager chatManager = ChatManager.getInstanceFor(mConnection);

    try {
        jid = JidCreate.entityBareFrom(toJid);
    } catch (XmppStringprepException e) {
        e.printStackTrace();
    }
    Chat chat = chatManager.chatWith(jid);
    try {
        Message message = new Message(jid, Message.Type.chat);
        message.setBody(body);
        chat.send(message);

    } catch (SmackException.NotConnectedException e) {
        e.printStackTrace();
    } catch (InterruptedException e) {
        e.printStackTrace();
    }
}
```
Of all these ChatManager can be found in the Smack documentation .The line

```java
 ChatManager chatManager = ChatManager.getInstanceFor(mConnection);
```

Gives us a ChatManager that we are going to use to get a Chat object that is in turn going to send the message.The line

```java
Chat chat = chatManager.chatWith(jid);
```

creates our Chat object. The chatWith() method of the ChatManager takes a Jid we create with the line below

```java
jid = JidCreate.entityBareFrom(toJid);
```

If any method doesn’t make sense , you can select it and hit “Ctrl+B”, Android Studio will take you straight to the documentation where you can read about it. We send the message down the pipe with the code block below

```java
Message message = new Message(jid, Message.Type.chat);
message.setBody(body);
chat.send(message);
```

We simply package the Jid and the body into a Message object that we send down the netowork pipe with the send() method of the our Chat object.By now ,Rooster is fully capable of sending messages to any xmpp client in the world.Go back to ContactModel class and modify the populateWithInitialContacts() method to include at least one XMPP contact that you know is connected and able to receive messages.Mine is highlighted in the code snippet below.

```java
private void populateWithInitialContacts(Context context)
{
    //Create the Foods and add them to the list;


    Contact contact1 = new Contact("usr10@someXmppServer.com");
    mContacts.add(contact1);
    Contact contact2 = new Contact("User2@server.com");
   ...
}
```

Run the app ,click on that contact to open his chatActivity ,type in some message and hit send.You can see below that my messages are getting across.

<img src="{{ site.url }}{{ site.baseurl }}/images/xmpp-smack-post/rooster_sending_message_processed.png" alt="Sending Messages">

To really live up to the promises of this tutorial ,we need to be able to receive messages sent to us by other people.Remember the processMessage() override method we just created above?Messages land in there when our friends respond back to us.We modify the method as shown below.

```java
public void connect() throws IOException,XMPPException,SmackException
{
    Log.d(TAG, "Connecting to server " + mServiceName);

    XMPPTCPConnectionConfiguration conf = XMPPTCPConnectionConfiguration.builder()
            .setXmppDomain(mServiceName)
            ....


    //Set up the ui thread broadcast message receiver.
    setupUiThreadBroadCastMessageReceiver();

    mConnection = new XMPPTCPConnection(conf);
    mConnection.addConnectionListener(this);
    try {
        Log.d(TAG, "Calling connect() ");
        mConnection.connect();
        mConnection.login(mUsername,mPassword);
        Log.d(TAG, " login() Called ");
    } catch (InterruptedException e) {
        e.printStackTrace();
    }

    ChatManager.getInstanceFor(mConnection).addIncomingListener(new IncomingChatMessageListener() {
        @Override
        public void newIncomingMessage(EntityBareJid messageFrom, Message message, Chat chat) {
            ///ADDED
            Log.d(TAG,"message.getBody() :"+message.getBody());
            Log.d(TAG,"message.getFrom() :"+message.getFrom());

            String from = message.getFrom().toString();

            String contactJid="";
            if ( from.contains("/"))
            {
                contactJid = from.split("/")[0];
                Log.d(TAG,"The real jid is :" +contactJid);
                Log.d(TAG,"The message is from :" +from);
            }else
            {
                contactJid=from;
            }

            //Bundle up the intent and send the broadcast.
            Intent intent = new Intent(RoosterConnectionService.NEW_MESSAGE);
            intent.setPackage(mApplicationContext.getPackageName());
            intent.putExtra(RoosterConnectionService.BUNDLE_FROM_JID,contactJid);
            intent.putExtra(RoosterConnectionService.BUNDLE_MESSAGE_BODY,message.getBody());
            mApplicationContext.sendBroadcast(intent);
            Log.d(TAG,"Received message from :"+contactJid+" broadcast sent.");
            ///ADDED

        }
    });
...
}
```

We add a listener to listen on incoming messages with the line

```java
ChatManager.getInstanceFor(mConnection).addIncomingListener(new IncomingChatMessageListener() {
```

IncomingChatMessageListener as an override called  newIncomingMessage. It is inside this override that we capture the received message.

```java
@Override
public void newIncomingMessage(EntityBareJid messageFrom, Message message, Chat chat) {
    ///ADDED
    Log.d(TAG,"message.getBody() :"+message.getBody());
    Log.d(TAG,"message.getFrom() :"+message.getFrom());

    String from = message.getFrom().toString();

    String contactJid="";
    if ( from.contains("/"))
    {
        contactJid = from.split("/")[0];
        Log.d(TAG,"The real jid is :" +contactJid);
        Log.d(TAG,"The message is from :" +from);
    }else
    {
        contactJid=from;
    }

    //Bundle up the intent and send the broadcast.
    Intent intent = new Intent(RoosterConnectionService.NEW_MESSAGE);
    intent.setPackage(mApplicationContext.getPackageName());
    intent.putExtra(RoosterConnectionService.BUNDLE_FROM_JID,contactJid);
    intent.putExtra(RoosterConnectionService.BUNDLE_MESSAGE_BODY,message.getBody());
    mApplicationContext.sendBroadcast(intent);
    Log.d(TAG,"Received message from :"+contactJid+" broadcast sent.");
    ///ADDED

}
```

Basically the function extracts the jid of the sender and the actual message from the parameters of the function and wraps all that info into an intent that it uses to send the broadcast message.For this to work we added two message types in RoosterConnectionService

```java
public static final String BUNDLE_TO = "b_to";

public static final String NEW_MESSAGE = "com.blikoon.rooster.newmessage";
public static final String BUNDLE_FROM_JID = "b_from";

public static RoosterConnection.ConnectionState sConnectionState;

The last thing we need to do is setup ChatActivity to receive those broadcast messages ,we already know how to do that.

public class ChatActivity extends AppCompatActivity {
   ...
    private SendButton mSendButton;
    private BroadcastReceiver mBroadcastReceiver;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        ........
    }

    @Override
    protected void onPause() {
        super.onPause();
        unregisterReceiver(mBroadcastReceiver);
    }

    @Override
    protected void onResume() {
        super.onResume();
        mBroadcastReceiver = new BroadcastReceiver() {
            @Override
            public void onReceive(Context context, Intent intent) {
                String action = intent.getAction();
                switch (action)
                {
                    case RoosterConnectionService.NEW_MESSAGE:
                        String from = intent.getStringExtra(RoosterConnectionService.BUNDLE_FROM_JID);
                        String body = intent.getStringExtra(RoosterConnectionService.BUNDLE_MESSAGE_BODY);

                        if ( from.equals(contactJid))
                        {
                            mChatView.receiveMessage(body);

                        }else
                        {
                            Log.d(TAG,"Got a message from jid :"+from);
                        }

                        return;
                }

            }
        };

        IntentFilter filter = new IntentFilter(RoosterConnectionService.NEW_MESSAGE);
        registerReceiver(mBroadcastReceiver,filter);


    }
}
```

When the broadcast receiver gets the message it tests to see if the message is of type NEW_MESSAGE.If the sender’s Jid is the same as the current chatActivitie’s contactJid ,we show the message in chatActivity ,otherwise we simply print a debug statement saying that we got some message.My shinny Rooster ChatActivity is shown below in action.

<img src="{{ site.url }}{{ site.baseurl }}/images/xmpp-smack-post/rooster_full_chat_processed.png" alt="Finished App">


Like any xmpp client that respects itself ,Rooster can now connect to a server ,send and receive messages.Feels great to make it doesn’t it.It has a lot of serious problems as of now though ,for example if you hit the back button ,all your messages are lost and you need to do more work in managing your connections.But the purpose of this tutorial was getting new people up to the point of using smack in their own projects.If you hit any bumpy roads in this tutorial don’t hesitate to ping me in the comments below.The complete source code for this tutorial can be found at my git repository.I hope this has been informative to you and I would like to thank you for reading.

<a href="https://blikoon.teachable.com/p/android-xmpp-chat-app-video-tutorial">**XMPP Smack Android Comprehensive Video Course Available!**</a> By the very same author of this tutorial.
{: .notice--warning}
[![foo]({{ site.url }}{{ site.baseurl }}/images/xmpp-smack-post/teachablexmppcourses.png)](https://blikoon.teachable.com/p/android-xmpp-chat-app-video-tutorial)

<a href="https://blikoon.teachable.com/p/android-xmpp-smack-chat-app-sending-and-receiving-files-http-file-upload">**Follow up course on Sending and Receiving Files with XMPP and Smack also available!**</a>
{: .notice--info}

## Comments

<a href="{{ site.url }}{{ site.baseurl }}/android-xmpp-smack-client-tutorial-comments">**OLD COMMENTS FOR THE POST AVAILABLE HERE!**</a>
{: .notice--info}


<a href="{{ site.url }}{{ site.baseurl }}/android-xmpp-smack-client-tutorial-comments">**NEW COMMENTS OR FEEDBACK ACCEPTED AS GITHUB ISSUES HERE**</a>
{: .notice--info}




