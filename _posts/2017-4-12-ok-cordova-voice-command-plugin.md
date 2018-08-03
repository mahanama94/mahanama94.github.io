---
layout: post
title: Ok Cordova, voice command plugin for Cordova
tags: [Mobile, Cordova]
---

It has been a while since Google (Android ) has introduced the voice interaction API for the developers.
The Voice interactions has enabled the developers to make their apps more user friendly by directing the user to
the right place in a simple command.

<img src="{{ site.baseurl }}/images/ok-cordova/ok-google.jpg" width="60%" alt="Ok Google, Y U No Cordova" title="Ok Google, Y U No Cordova">

Even though the development API has been there for a while, a Cordova plugin was not properly available till the date of the blog post.
So I thought of building one for the Hybrid developers to Harness the power of native features of the mobile.

### Architecture

The voice command API provided by the android directs the user to an activity specified in the manifest of the Application. Cordova being a single Activity application, we can use the only Activity of the Application as the entry point. However, Instead of using the only activity of the application, I thought of building another entry point for the application for the implicit calls of the application.

Once an implicit call is recieved by the application, the application will push the implicit call data to the plugin for the voice command.
In the JS end, the developer has to check for the state of the implicit calls. As required by the application.

Due to various performance scenarios, JS and Java ends may perform differently.

##### JS being ahead

In this case the JS will not recieve the event immediately. Instead, it will recieve as an explicit call.
Once the Java end completes the necessary event handlings and identifies the type of call, the plugin will call the success back with the data.

##### Java being ahead

In the case if Java being ahead will have no impact for the application.
JS will recieve the event and the parameters immediately.

### Usage

```javascript

voicecommand.check(ConfigurationObject, SuccessCallback, ErrorCallback)

```

<br>

| **`ConfigurationObject`**  | `Contains the configuration settings for the voice interactions.`                                           |
| **`SuccessCallback`**     |  `Callback for the success of finding the application invocation.( For both implicit and explicit calls). With data containing the information of the event`    |
| **`ErrorCallback`**       | `Callback for the error of the process.`                                                                     |


<br><br>

##### ConfigurationObject

| **`remember`** | `The application should remember the type of Application call` | ```true ``` |

<br>
##### Data

| **`type`** | `Type of the implicit call Ex - SetAlarm (See Documentation)` |

<br>

### Sample Code

```javascript

function voiceCommandTest(){

  voicecommand.check([], function(message){
    // receive and handle the event parameters here
    // for explicit calls,  you can ignore
    console.log(message)
  }, function(){
    console.log('Error');
  })
}
```
You can attach the above function to an event according to the requirements of the application.

### Source

[https://github.com/mahanama94/cordova-voice-command-plugin](https://github.com/mahanama94/cordova-voice-command-plugin "Source Code")

### Hire me

I currently work as a freelancer for projects in mobile applications, web applications, desktop applications, data mining
and machine learning. I provide my services mainly using Ionic framework, PHP, NodeJs, Java, electronJs, Erlang and Python.
You can directly contact me for your projects or can visit my Fiverr profile. Please contact me before placing an order.

[mahanama94 Fiverr](https://www.fiverr.com/mahanama94/)

Cheers !!!

**mahanama94**
