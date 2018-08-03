---
layout: post
title: Search in your app with voice
tags: [Mobile, Cordova, Plugins]
---

In a previous blog we saw how to create the first voice controlled cordova application with cordova voice plugin. In this blog we will be using the same plugin to direct the user right in to a search functionality in your awesome cordova application.

Search feature can come in handy if the user of the application usually searches for content rather than going through some series of steps. For instance consider youtube. Lets start off. :D

### Getting started

-  I will be using the app we have developed in a previous blog post as the starting point for the awesome app.

[Ok Cordova, First voice controlled Cordova App](https://mahanama94.github.io/first-voice-controlled-cordova-application/ "Ok Cordova, First voice controlled Cordova App")

### Installing the plugin

- Once you have configured the app properly, you can add the cordova voice plugin using command,

**```cordova plugin add https://github.com/mahanama94/cordova-voice-command-plugin.git```**

- The voice command plugin comes with all the voice actions enabled by default at the time of the post. So we will be having no configurations to use the plugin.

### Coding

- Since we have already coded in the previous blog post, we won't be coding for the app. Instead we will be using the same codebase to demonstrate the capability of your awesome app.


- Add the android platform

**```cordova platform add android```**

- Deploy the app

**```cordova run android```**

### Testing

- Launch the application with the default launcher. The app will prompt you that No voice command has been used for the application.

<img src="{{ site.baseurl }}/images/search-in-app/no-voice-action.png" width="50%" alt="No Voice Action" title="No Voice Action">

- To launch the app with search, you will have to use Google assist to search in the application. Try,
`OK Google, Search for Memes in my awesome app`

<img src="{{ site.baseurl }}/images/search-in-app/app-select.png" width="50%" alt="No Voice Action" title="No Voice Action">

- Which will open up your app. If you hit the test button we have added, you will see the query we have submitted Google assist to search in my awesome app.

<img src="{{ site.baseurl }}/images/search-in-app/voice-action.png" width="50%" alt="No Voice Action" title="No Voice Action">

### Special Notes

- At the moment of the blog post, the plugin supports all the types of voice actions by the user.

- Since the plugin is in development stage, it is recommended to follow the guide in the documentation.

### Hire me

I currently work as a freelancer for projects in mobile applications, web applications, desktop applications, data mining
and machine learning. I provide my services mainly using Ionic framework, PHP, NodeJs, Java, electronJs, Erlang and Python.
You can directly contact me for your projects or can visit my Fiverr profile. Please contact me before placing an order.

[mahanama94 Fiverr](https://www.fiverr.com/mahanama94/)

Cheers!!!

**mahanama94**
