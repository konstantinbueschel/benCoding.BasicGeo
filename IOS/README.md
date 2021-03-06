<h1>basicGeo for iOS</h1>

The benCoding.basicGeo module provides enhanced geo location functionality over the core Titanium framework. For example you can access the native platform reverse geo decoders, or use the built in distance calculations methods.

<h2>Before you start</h2>
* You need Titanium 3.1.1.GA or greater.
* This module will only work with iOS 5 or great.  

<h2>tiapp.xml update</h2>

If you plan to support iOS 6+ you will need to add the NSLocationUsageDescription entry in your tiapp.xml as shown below.  The string value displays the purpose message that the user will be alerted when GPS access is requested. This entry is needed in response to the purpose property being deprecated in iOS 6.
~~~
    <ios>
        <min-ios-ver>6.0</min-ios-ver>
        <plist>
            <dict>
                <key>NSLocationUsageDescription</key>
                <string>GPS access for demo</string>                
            </dict>
        </plist>
    </ios>
~~~
<h2>Setup</h2>

* Download the latest release from the [dist folder](https://github.com/benbahrenburg/benCoding.BasicGeo/tree/master/IOS/basicGeo/dist) or you can build it yourself 
* Install the bencoding.basicGeo module. If you need help here is a "How To" [guide](https://wiki.appcelerator.org/display/guides/Configuring+Apps+to+Use+Modules). 
* You can now use the module via the commonJS require method, example shown below.

<pre><code>
//Add the core module into your project
var geo = require('bencoding.basicgeo');

</code></pre>

Now we have the module installed and avoid in our project we can start to use the components, see the feature guide below for details.


<h2>Availability</h2>

<h4>reverseGeoSupported</h4>

Indicates if you can use the native Reverse Geo Location capabilities.  Returns false if the device does not support this functionality.

Please note, iOS 5 is required to use native Reverse Geo Location.

Method returns true or false

<h4>allowBackgrounding</h4>

Indicates if the device supports background operations.

Method returns true or false

<h4>significantLocationChangeMonitoringAvailable</h4>

Indicates if the device supports significant location change monitoring. If your app is running on a wifi only device you will not be able to use this feature. 

This feature is only supported on limited devices, so it is recommended you always test before using.

Method returns true or false

<h4>headingAvailable</h4>

Returns a Boolean value indicating whether the location manager is able to generate heading-related events.

<h4>locationServicesEnabled</h4>

Indicates if the user has enabled or disabled location services for the device

Method returns true or false

<h4>regionMonitoringAvailable</h4>

Indicates if the device supports region monitoring.  If your app is running on a wifi only device you will not be able to use this feature. 

This feature is only supported on limited devices, so it is recommended you always test before using.

Method returns true or false

<h4>regionMonitoringEnabled</h4>

Returns a Boolean value indicating whether region monitoring is currently enabled.

<h4>locationServicesAuthorization</h4>

Returns an authorization constant indicating if the application has access to location services.

Always returns AUTHORIZATION_UNKNOWN on pre-4.2 devices.

If locationServicesAuthorization is AUTHORIZATION_RESTRICTED, you should not attempt to re-authorize: this will lead to issues.

<b>Sample</b>

<pre><code>
//Add the core module into your project
var geo = require('bencoding.basicgeo');
//Create our class with all of the availability information
var available = geo.createAvailability();
Ti.API.info("Let's see what is available");
Ti.API.info("can we use reverse geo decoding? " + available.reverseGeoSupported());
Ti.API.info("can we run in the background? " + available.allowBackgrounding());
Ti.API.info("can we save battery and use significant change events? " + available.significantLocationChangeMonitoringAvailable());
Ti.API.info("can we determine heading? " + available.headingAvailable());
Ti.API.info("are location services enabled for this device and app? " + available.locationServicesEnabled());
Ti.API.info("is region monitoring (geo fencing) available? " + available.regionMonitoringAvailable());
Ti.API.info("are we using region monitoring (geo fencing)? " + available.regionMonitoringEnabled());
Ti.API.info("what is our location services authorization status? " + available.locationServicesAuthorization());

</code></pre>

<h2>CurrentGeolocation</h2>

<h4>getCurrentPlace</h4>

Retrieves the last known place information (address) from the device.

Parameters
* callback : Function to invoke on success or failure of obtaining the current place information.

<b>Sample</b>

<pre><code>
//Add the core module into your project
var geo = require('bencoding.basicgeo');

function resultsCallback(e){
	Ti.API.info("Did it work? " + e.success);
	if(e.success){
		Ti.API.info("It worked");
	}	

	var test = JSON.stringify(e);
	Ti.API.info("Results stringified" + test);
};

Ti.API.info("Now let's check out the GeoCoders")
var geoCurrent = geo.createCurrentGeolocation();

Ti.API.info("Let's get the places information (address) for our current location");
Ti.API.info("We make our call and provide a callback then wait...");
geoCurrent.getCurrentPlace(resultsCallback);
</code></pre>

<h4>getCurrentPosition</h4>

Retrieves current coordinates from the device

Parameters
* callback : Function to invoke on success or failure of obtaining the current coordinates.

<b>Sample</b>

<pre><code>
//Add the core module into your project
var geo = require('bencoding.basicgeo');

function resultsCallback(e){
	Ti.API.info("Did it work? " + e.success);
	if(e.success){
		Ti.API.info("It worked");
	}	

	var test = JSON.stringify(e);
	Ti.API.info("Results stringified" + test);
};

Ti.API.info("Now let's check out the GeoCoders")
var geoCurrent = geo.createCurrentGeolocation();

Ti.API.info("Let's get the coordinates for our current location");
Ti.API.info("We make our call and provide a callback then wait...");
geoCurrent.getCurrentPosition(resultsCallback);

</code></pre>

<h2>Geocoder</h2>

<h4>reverseGeocoder</h4>

Tries to resolve a location to an address.

The callback receives a results object. If the request is successful, the object includes one or more addresses that are possible matches for the requested coordinates.

Parameters:
* latitude : Number
* longitude : Number
* callback : Function to invoke on success or failure.


All three parameters are required.

<b>Sample</b>

<pre><code>
//Add the core module into your project
var geo = require('bencoding.basicgeo');

function reverseGeoCallback(e){
	Ti.API.info("Did it work? " + e.success);
	if(e.success){
		Ti.API.info("This is the number of places found, it can return many depending on your search");
		Ti.API.info("Places found = " + e.placeCount);	
	}	

	var test = JSON.stringify(e);
	Ti.API.info("Forward Results stringified" + test);
};

Ti.API.info("Now let's check out the GeoCoders")
var geoCoder = geo.createGeocoder();

Ti.API.info("Let's now try to do a reverse Geo lookup using the Time Square coordinates");
Ti.API.info("Pass in our coordinates and callback then wait...");
geoCoder.reverseGeocoder(40.75773,-73.985708,reverseGeoCallback);

</code></pre>

<h4>forwardGeocoder</h4>

Resolves an address to a location.
Parameters:
* Address : String
* callback : Function to invoke on success or failure.

Both parameters are required.

<b>Sample</b>

<pre><code>
//Add the core module into your project
var geo = require('bencoding.basicgeo');

function forwardGeoCallback(e){
	Ti.API.info("Did it work? " + e.success);
	if(e.success){
		Ti.API.info("This is the number of places found, it can return many depending on your search");
		Ti.API.info("Places found = " + e.placeCount);	
	}	

	var test = JSON.stringify(e);
	Ti.API.info("Forward Results stringified" + test);
};

Ti.API.info("Now let's check out the GeoCoders")
var geoCoder = geo.createGeocoder();

Ti.API.info("Now let's do some forward Geo and lookup the address for Appcelerator HQ");
var address="440 N. Bernardo Avenue Mountain View, CA";

Ti.API.info("We call the forward Geocoder providing an address and callback");
Ti.API.info("Now we wait for the lookup");
geoCoder.forwardGeocoder(address,forwardGeoCallback);

</code></pre>

<h2>SignificantChange</h2>

The SignificantChange sub module provides a battery efficient way to monitor location change events.  This sub module is a Titanium wrapper around Apple's native [startMonitoringSignificantLocationChanges](http://developer.apple.com/library/ios/#DOCUMENTATION/CoreLocation/Reference/CLLocationManager_Class/CLLocationManager/CLLocationManager.html) and [stopMonitoringSignificantLocationChanges](http://developer.apple.com/library/ios/#DOCUMENTATION/CoreLocation/Reference/CLLocationManager_Class/CLLocationManager/CLLocationManager.html) methods. 

It is <b>IMPORTANT</b> to note this will only run while in the background.  You will see in the below example, we start and stop monitoring using the Ti.App.addEventListener('pause') and Ti.App.addEventListener('resumed') events.

The distance or event used in determining what is an significant change is controlled by Apple.  The change event is typically fired when the device switches between cell towers.

This functionality depends on having a cell connection, therefore wifi only devices such as the iPad (wifi only) or iPod Touch cannot use this component.  Please use the availability checks as shown below when implementing.

<b>Methods:</b>
* startSignificantChange - Starts monitoring for location change
* stopSignificantChange - Stops monitoring for location change

<b>Properties:</b>
* purpose - text to display in the permission dialog when requesting location services.
* staleLimit - seconds provided to determine if the output should be considered stale. This is used in generating the slate boolean indicator included in the results.

<b>Listeners:</b>
* error - this listener is triggered when an error happens during the monitoring process
* start - this listener is triggered when the startMonitoring method has completed successfully
* stop - this listener is triggered when the stopMonitoring method has completed successfully
* change - this listener is triggered when the monitor detects the device has crossed a threshold such as the distanceFilter

<b>Sample</b>

<pre><code>
//Create our class with all of the availability information
var available = require('bencoding.basicgeo').createAvailability();

//Define our location monitor object
var significantChange = {
	module : null,
	errorEvt : function(e){
		Ti.API.info("significantChange Error " + JSON.stringify(e));
	},
	changeEvt : function(e){
		Ti.API.info("significantChange Change " + JSON.stringify(e));			
	},
	startEvt : function (e){
		Ti.API.info("significantChange Start " + JSON.stringify(e));	
	},
	stopEvt : function(){
		Ti.API.info("significantChange Stop " + JSON.stringify(e));				
	},				
	start : function(){
		//First we start everything up
		if(significantChange.module==null){
			significantChange.module = require("bencoding.basicgeo").createSignificantChange();
			significantChange.module.addEventListener('error', significantChange.errorEvt);
			significantChange.module.addEventListener('start', significantChange.startEvt);
			significantChange.module.addEventListener('stop', significantChange.stopEvt);
			significantChange.module.addEventListener('change',significantChange.changeEvt);	
			Ti.API.info('significantChange Listeners Added');									
		}
		//Add our configuration parameters
		significantChange.module.purpose = "demo";	
		significantChange.module.staleLimit = 5;
							
		//Start monitoring for changes
		significantChange.module.startSignificantChange();
	 },
	 stop : function(){
		if(significantChange.module!=null){
			significantChange.module.stopSignificantChange();
			Ti.API.info('significantChange Stopped');					
			significantChange.module.removeEventListener('error', significantChange.errorEvt);
			significantChange.module.removeEventListener('start', significantChange.startEvt);
			significantChange.module.removeEventListener('stop',  significantChange.stopEvt);
			significantChange.module.removeEventListener('change',significantChange.changeEvt);
			Ti.API.info('significantChange Listeners Removed');						
			significantChange.module=null;	
		}		
	}	
};


Ti.App.addEventListener('resumed',function(e){
	//Stop location monitoring
	significantChange.stop();
});

Ti.App.addEventListener('pause',function(e){
	//Does this device support background actions?
	if(available.allowBackgrounding()){
		//Check that the device can use Significant Location Change Monitoring
		if(available.significantLocationChangeMonitoringAvailable()){
			//Start location monitoring
			significantChange.start();
		}
	}
});

</code></pre>
<h2>LocationMonitor</h2>

The Location Monitor provides an high accuracy way to monitor for location change.  

This sub module is designed for repeated geo location activity, not a single look-up.  This can have an impact on battery usage.  If you are concerned about battery usage, and do not need a high level of accuracy, I would recommend using the SignificantChange functionality.

<b>Methods:</b>
* startMonitoring - Starts monitoring for location change
* stopMonitoring - Stops monitoring for location change

<b>Properties:</b>
* purpose - text to display in the permission dialog when requesting location services.
* staleLimit - seconds provided to determine if the output should be considered stale. This is used in generating the slate boolean indicator included in the results.
* accuracy - specifies the requested accuracy for location updates.
* distanceFilter - the distance in meters the location monitor should wait before triggering the change event.
* timerInterval - (optional) monitor timer in seconds. If a positive number is provided, the monitor timer will be enabled and repeat at the interval provided.

<b>Listeners:</b>
* error - this listener is triggered when an error happens during the monitoring process
* start - this listener is triggered when the startMonitoring method has completed successfully
* stop - this listener is triggered when the stopMonitoring method has completed successfully
* change - this listener is triggered when the monitor detects the device has crossed a threshold such as the distanceFilter
* timerFired - this listener is triggered when the montir timer, if engaged, is elapsed.

<b>Sample</b>

<pre><code>
//Define our location monitor object
var locationMonitor = {
	module : null,
	errorEvt : function(e){
		Ti.API.info("Location Monitor Error " + JSON.stringify(e));
	},
	changeEvt : function(e){
		Ti.API.info("Location Monitor Change " + JSON.stringify(e));			
	},
	startEvt : function (e){
		Ti.API.info("Location Monitor Start " + JSON.stringify(e));	
	},
	stopEvt : function(){
		Ti.API.info("Location Monitor Stop " + JSON.stringify(e));				
	},	
	timerEvt : function(){
		Ti.API.info("Location Monitor Stop " + JSON.stringify(e));				
	},				
	start : function(){
		//First we start everything up
		if(locationMonitor.module==null){
			locationMonitor.module = require("bencoding.basicgeo").createLocationMonitor();
			locationMonitor.module.addEventListener('error', locationMonitor.errorEvt);
			locationMonitor.module.addEventListener('start', locationMonitor.startEvt);
			locationMonitor.module.addEventListener('stop', locationMonitor.stopEvt);
			locationMonitor.module.addEventListener('change',locationMonitor.changeEvt);	
			locationMonitor.module.addEventListener('timerFired', locationMonitor.timerEvt);
			Ti.API.info('Location Monitor Listeners Added');									
		}
		//Add our configuration parameters
		locationMonitor.module.purpose = "demo";	
		locationMonitor.module.staleLimit = 5;
		locationMonitor.module.accuracy = Ti.Geolocation.ACCURACY_LOW;
		locationMonitor.module.distanceFilter = 300;
							
		//Start monitoring for changes
		locationMonitor.module.startMonitoring();
	 },
	 stop : function(){
		if(locationMonitor.module!=null){
			locationMonitor.module.stopMonitoring();
			Ti.API.info('locationMonitor Stopped');					
			locationMonitor.module.removeEventListener('error', locationMonitor.errorEvt);
			locationMonitor.module.removeEventListener('start', locationMonitor.startEvt);
			locationMonitor.module.removeEventListener('stop',  locationMonitor.stopEvt);
			locationMonitor.module.removeEventListener('change',locationMonitor.changeEvt);
			locationMonitor.module.removeEventListener('timerFired', locationMonitor.timerEvt);
			Ti.API.info('locationMonitor Listeners Removed');						
			locationMonitor.module=null;	
		}		
	}	
};

//Start up location monitoring
locationMonitor.start();

</code></pre>
	

<h2>Telephony</h2>

The Telephony class provides access to the geo location methods related to your SIM card.

Below shows how to use this class to obtain the ISO country code for the user’s cellular service provider. This is the carrier on the SIM.  Here is a listing of ISO codes [wikipedia](http://en.wikipedia.org/wiki/ISO_3166-1)

<h5>Sample</h5>

<pre><code>
//Add the core module into your project
var geo = require('bencoding.basicgeo');
//Create the Telephony object
var geoTelephony = geo.createTelephony();
//Return the country code associated with your SIM
Ti.API.info("Your SIM Country Code is " + geoTelephony.mobileCountryCode());
</code></pre>

<h2>Helpers</h2>

<h4>distanceBetweenInMeters</h4>

Determine the distance (in meters) between two sets of coordinates

<h4>bearingInDegrees</h4>

Determine the bearing between two sets of coordinates

<h5>Sample</h5>

<pre><code>
var geo = require('bencoding.basicgeo');
Ti.API.info("We have a few helpers here are some examples");
var helpers = geo.createHelpers();
Ti.API.info("How far is it between time square and the empire state building?");
var timeSq2Emipre=helpers.distanceBetweenInMeters(40.75773,-73.985708,40.748433, -73.985656);
Ti.API.info(timeSq2Emipre +" meters");
Ti.API.info("How far is it from Times Square to Red Square?");
var timeSq2Red=helpers.distanceBetweenInMeters(40.75773,-73.985708,55.754167, 37.62);
Ti.API.info(timeSq2Red +" meters");	
</code></pre>

<h2>Examples</h2>

Please check the [example](https://github.com/benbahrenburg/benCoding.BasicGeo/tree/master/IOS/basicGeo/example) for samples on how to call the different functions contained within the module.

The examples for this project are still a work in progress, until I can completely document the project please contact me on twitter if you have a specific question.

<h2>Licensing & Support</h2>

This project is licensed under the OSI approved Apache Public License (version 2). For details please see the license associated with each project.

Developed by [Ben Bahrenburg](http://bahrenburgs.com) available on twitter [@benCoding](http://twitter.com/benCoding)

<h2>Contributing</h2>

The benCoding.basicGeo is a open source project.  Please help us by contributing to documentation, reporting bugs, forking the code to add features or make bug fixes or promoting on twitter, etc.

<h2>Learn More</h2>

<h3>Twitter</h3>

Please consider following the [@benCoding Twitter](http://www.twitter.com/benCoding) for updates and more about Titanium.

<h3>Blog</h3>

For module updates, Titanium tutorials and more please check out my blog at [benCoding.Com](http://benCoding.com). 
