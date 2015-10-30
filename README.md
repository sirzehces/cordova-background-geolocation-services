# cordova-background-geolocation-services
Background Geolocation For Android and iOS with pure javascript callbacks.

#### What is this?
This plugin is for enabling background geolocation in your cordova project. It was aimed with the specific goal of normalizing the API for android and iOS and retrieving constant location updates in the background until you tell it to stop (If you tell it you want updates every 3 seconds it will give you one every 3 seconds). This is currently in active development. Feel free to make any requests. Below are features I am currently working on.

### Todo: 
 * Both: 
  * SQLlite Storage
  * Enable better distance filtering, possibly by speed readings
  * Test
  * Documentation
  * Demos (Regular Node, Meteor)
  * Meteor Specific Atmosphere Package
  * Npm
 * Android:
  * Integrated Detected Activities for better battery life
  * ~~Make Icon Notification user configureable~~
  * ~~Get Android callbacks to work, always.~~
 * iOS
  * Enable switching to only using significant changes and back 

### Techniques used:

**Android** : Uses an android Service and some trickery to bind it to your main App.

**iOS** : Uses a timer based approach to enable endless background tracking.

###Setup: 
* Need to make sure you have Google Play Services AND Google Repository installed via your android-sdk manager prior to building your application with this. It will be under the extras part of the sdk manager. More information can be found here: http://developer.android.com/sdk/installing/adding-packages.html.

### How to use: 

This plugin exports an object at 
````javascript
window.plugins.backgroundLocationServices
````

````javascript

//Make sure to get at least one GPS coordinate in the foreground before starting background services
navigator.geolocation.getCurrentPosition();

//Get plugin
var bgLocationServices =  window.plugins.backgroundLocationServices;

//Congfigure Plugin
bgLocationServices.configure({
     desiredAccuracy: 20, // Desired Accuracy of the location updates (lower means more accurate but more battery consumption)
     distanceFilter: 5, // How far you must move from the last point to trigger a location update
     notificationTitle: 'BG Plugin', // <-- android only, customize the title of the notification
     notificationText: 'Tracking', // <-- android only, customize the text of the notification
     activityType: 'AutomotiveNavigation',
     debug: true, // <-- enable this hear sounds for background-geolocation life-cycle.
     interval: 9000, // Requested Interval in between location updates, in seconds
     fastestInterval: 5000, // <-- android only Fastest interval your app / server can handle updates
});

//Register a callback for location updates, this is where location objects will be sent in the background
bgLocationServices.registerForLocationUpdates(function(location) {
     console.log("We got an BG Update" + JSON.stringify(location));
}, function(err) {
     console.log("Error: Didnt get an update", err);
});

//Start the Background Tracker. When you enter the background tracking will start, and stop when you enter the foreground.
bgLocationServices.start();


///later, to stop
bgLocationServices.start();
````

Other methods -

Enable Aggressive Mode: Sets Location tracking to its most accurate, most intensive state.

````javascript
bgLocationServices.startAggressiveTracking();
````

