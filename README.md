# What is RudderStack?

[RudderStack](https://rudderstack.com/) is a **customer data pipeline** tool for collecting, routing and processing data from your websites, apps, cloud tools, and data warehouse.

More information on RudderStack can be found [here](https://github.com/rudderlabs/rudder-server).

# Installing RudderStack's JavaScript SDK on your Vue.js app

Follow the steps below to install our JavaScript SDK on your vue app -

## Step 1: Install RudderStack using the code snippet

To integrate the SDK, place the following code snippet in the `<head>` section of your Vue app's `index.html`.

You can use either the minified or non-minified version of the code:

The minified version is as follows:
```
<script> 
rudderanalytics=window.rudderanalytics=[];for(var methods=["load","page","track","alias","group","identify","ready","reset"],i=0;i<methods.length;i++){var method=methods[i];rudderanalytics[method]=function(d){return function(){rudderanalytics.push([d,...arguments])}}(method)}rudderanalytics.load("YOUR_WRITE_KEY","DATA_PLANE_URI"),rudderanalytics.page();
</script>

<script  src="https://cdn.rudderlabs.com/v1/rudder-analytics.min.js"></script>
```
The non-minified version of the code is shown below:
```
<script>
	rudderanalytics = window.rudderanalytics = [];
	
	var  methods = [
		"load",
		"page",
		"track",
		"identify",
		"alias",
		"group",
		"ready",
		"reset"
	];

	for (var i = 0; i < methods.length; i++) {
  		var method = methods[i];
  		rudderanalytics[method] = function (methodName) {
    			return function () {
      				rudderanalytics.push([methodName].concat(Array.prototype.slice.call(arguments)));
    			};
  			}(method);
	}
	rudderanalytics.load("YOUR_WRITE_KEY", "DATA_PLANE_URI");
	//For example,
	//rudderanalytics.load("1Qb1F3jSWv0eKFBPZcrM7ypgjVo", "http://localhost:8080");
	rudderanalytics.page();
</script>

<script  src="https://cdn.rudderlabs.com/v1/rudder-analytics.min.js"></script>
```

**NOTE**: Whichever version of the code you use, you need to replace `YOUR_WRITE_KEY` with the write key in the RudderStack Control Plane and `DATA_PLANE_URL` with the URL of the RudderStack backend data plane.

You can also execute the min file in async/defer way, as shown below:
```
<script async src="https://cdn.rudderlabs.com/rudder-analytics.min.js"></script>
```
**NOTE**: We are moving our SDK to a diiferent path from the earlier https://cdn.rudderlabs.com/rudder-analytics.min.js to
https://cdn.rudderlabs.com/v1/rudder-analytics.min.js. The earlier path may not be maintained in coming releases.


## Step 2: Identify your users using the `identify()` method

The `identify()` method allows you to link users and their actions to a specific userid.

A sample example of how the `identify()` method works is as shown:
```
rudderanalytics.identify(
      "12345",
      { email: "name@domain.com" },
      {
        page: {
          path: "",
          referrer: "",
          search: "",
          title: "",
          url: ""
        }
      },
  () => {console.log("in identify call");}
);
```
In the above example, information such as the user ID, email along with contextual information such as IP address, anonymousId, etc. will be captured.

**NOTE**: There is no need to call `identify()` for anonymous visitors to your website. Such visitors are automatically assigned an `anonymousId`.

## Step 3: Track your users’ actions using the `track()` method
The `track()` method allows you to track any actions that your users might perform.

A sample example of how the track() method works is as shown:
```
rudderanalytics.track(
  "test track event GA3",
  {
    revenue:  30,
    currency:  'USD' ,
    user_actual_id:  12345
  },
  () => {console.log("in track call");}
);
```
In the above example, the method tracks the event ‘test track event GA3’, information such as the revenue, currency, anonymousId.

You can use this method to track various other success metrics for your website, such as user signups, item purchases, article bookmarks, and much more.

**NOTE**: To override contextual information, for ex: anonymising IP and other contextual fields like page properties, the following template can be used. Similarly one can override the auto generated anonymousId with provided id. For this:

```
rudderanalytics.track(
  "test track event GA3",
  {
    revenue:  30,
    currency:  'USD' ,
    user_actual_id:  12345
  },
  {
    page: {
          path: "",
          referrer: "",
          search: "",
          title: "",
          url: ""
    },
    context: {
      ip:  "0.0.0.0"
    },
    anonymousId:  "00000000000000000000000000"
  }, 
  () => {console.log("in track call");}
);
```

And we’re done! You’ve successfully installed `rudder-analytics.js` tracking. Now you can enable and use any event destination to send your processed event data that you want, in no time at all.

For a detailed technical documentation and troubleshooting guide on the RudderStack’s JavaScript SDK, click [here](https://docs.rudderlabs.com/sdk-integration-guide/getting-started-with-javascript-sdk).

## Step 4: Check Ready State
There are cases when one may want to tap into the features provide by end destination SDKs to enhance tracking and other functionality. Rudder SDK exposes a `ready` api with a `callback` parameter that fires when the SDK is done initialising itself and other third-party native-sdk destinations.

Ex: 
```
rudderanalytics.ready(
	() => {console.log("we are all set!!!");}
);
```
# Adding callbacks to standard methods

One can also define callbacks to common methods of  ```rudderanalytics``` object.
***Note***: For now, the functionality is supported for ```syncPixel``` method which is called in Rudder SDK when making sync calls in integrations for relevant destinations.

Ex:
```
<script>
rudderanalytics.syncPixelCallback = obj  => {
    rudderanalytics.track(
         "sync lotame",
         { destination: obj.destination },
         { integrations: { All: false, S3: true } }
    ); 
};
</script>

<script src="https://cdn.rudderlabs.com/rudder-analytics.min.js"></script>
```
In the above example, we are defining a  ```syncPixelCallback``` on the analytics object before the call to load the SDK. This will lead to calling of this registered callback with the parameter 
```{destination: <destination_name>}``` whenever a sync call is made from Rudder SDK for relevant integrations like *Lotame*.

We will be adding similar callbacks for apis such as ```track, page, identify``` etc.

# Running your Vue Application

Once you have succesfully installed our JS SDK, you are all set to send your tracking data to RudderStack.All you need to do now is to install dependencies for your app and run it.

## Installing Dependencies

Run the following command depending on whether you use `npm` or `yarn` to manage your packages -
```
npm install
```
OR
```
yarn install
```
## Running your Application

To run your application, you need to run `npm start` or `yarn start` from your project's `root` directory. Your events should now appear in the live events section of your source.

## Contact Us

If you come across any issues while configuring or using the JavaScript SDK on your Vue app, please feel free to start a conversation on our [Slack](https://resources.rudderstack.com/join-rudderstack-slack) channel. We will be happy to help you.
