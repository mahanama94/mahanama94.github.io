---
layout: post
title: Ok Cordova, First voice controlled Cordova App
---

In a previous blog post, I have included how to build a voice plugin for Cordova. In the blog we will be using the plugin
built to make Android-Cordova application to set alarms.

### Getting started

- First of all, you are required to have node, npm, cordova-cli android sdk installed in your Computer. (Which I will not be covering)

- Once you have installed them, we can get off by initiating a cordova application with

**`cordova create helloApp`**

- Then changes to the directory by command

**`cd helloApp`**

(Open the folder in your favorite text editor or IDE )

### Installing the Voice Plugin

- Install the Voice Command plugin using the command

**```cordova plugin add https://github.com/mahanama94/cordova-voice-command-plugin.git```**

### Coding

- For the demonstration purpose, we will be adding a button to test the plugin status. Add the following code in `index.html`.

``` html

  <button id="button">Test Button</button>

```
<img src="{{ site.baseurl }}/images/set-alarm/app-home.png" width="50%" alt="App Home" title="App Home">

- Then connect the button with the plugin using javascript. Add the following lines of code at the end of `index.js` file.

```javascript

  function myFunction(){
    voicecommand.check({}, function(message){
      alert(JSON.stringify(message));
    },
    function(){
        alert("error occurred");
    });
    }

  document.getElementById("button").addEventListener('click', myFunction);
```
- Add the android platform.

**```cordova platform add android```**

- Now the App is ready to go with "VOICE COMMANDS" :D. Deploy the app.

**```cordova run android```**

### Testing the Application

- Launch the application with the Default Launcher (Icon) and try out the button. You will be alerted that the App has been called with no Voice Command.

<img src="{{ site.baseurl }}/images/set-alarm/no-voice-action.png" width="50%" alt="No Voice Action" title="No Voice Action">

- To launch the app with voice commands, you will have to ask google to Set an alarm for you. Then Google will prompt you apps to choose from which will include clock apps and your cordova App :o. Then select your cordova app respond Google.

<img src="{{ site.baseurl }}/images/set-alarm/app-select.png" width="50%" alt="App Select" title="App Select">

- Once the app opens, hit the button. You will be prompted with information you have said to Google.

<img src="{{ site.baseurl }}/images/set-alarm/voice-action.png" width="50%" alt="Voice Action" title="Voice Action">

### Special Notes

- Plugin is still in development stage and changes on response patterns are still to be completed. They might result in application not performing as expected.

- It is recommended to follow the Guide included with the repository.

- Plugin is currently supporting only SetAlarm operations only and once the plugin is added app is registered as an Application capable pf setting alarms.

Cheers !!!

**mahanama94**
