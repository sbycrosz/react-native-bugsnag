# React Native Bugsnag

Easily add **[Bugsnag](https://bugsnag.com/)** exception monitoring support to your React Native application.

_Although this is not affiliated with Bugsnag directly, we do have [their support](https://twitter.com/bugsnag/status/749027008085045252)._

## Installation

### iOS

1. Install the official iOS Bugsnag sdk into your app according to their **[iOS instructions][ios-installation]**.
	(note: We did not add KSCrash.framework (step 4) to our native project at all.)

   Ensure that **[Symbolication](#symbolication)** is properly setup in your project as well.

2. Install the React Native Bugsnag package:

  ```bash
  npm install --save react-native-bugsnag
  ```

  _(Make sure to restart your package manager afterwards.)_
  
3. In your `AppDelegate.m` file, add the following code changes:

  a. Import our RNBugsnag library:

  ```objective-c
  #import "RNBugsnag.h"  // Add this line.

  @implementation AppDelegate
  ```

  b. Add your BUSNAG Api Key inside the Info.Plist like so:
  
  	Add a new entry with a key of: `BUGSNAG_API_KEY` and a value of your Bugsnag API KEY ([Usually found within your project here](https://bugsnag.com/settings/)).
  	Opening the Info.Plist with a text editor your addition should look like this:
  	
  	```
  	<key>BUGSNAG_API_KEY</key>
	<string>whatever_your_api_key_is</string>
	```

  c. Drag and drop the `./node_modules/react-native-bugsnag/ios/RNBugsnag.xcodeproj within your Libraries group in your Xcode project`
  
  d. Go to your Project-->Target-->Build Settings--> Header Search Paths and add the following line at the end:
  `$(SRCROOT)/../node_modules/react-native-bugsnag/ios/RNBugsnag`
  
  e. Go to your Project-->Target-->General-->Linked Frameworks and Libraries and add libRNBugsnag.a to the list.
  
  f. Initialize RNBugsnag inside of `didFinishLaunchingWithOptions`:
  

  ```objective-c
  - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
  {

    // ... other code

    [RNBugsnag init];	//initialize it

  }
  ```
  
  f. Enjoy!
  
  


### Android

1. Go to your settings.gradle and add the following lines after 
 	
 	```java
 	//somewhere after include ':app' add the following 2 lines
 	
	include ':react-native-bugsnag'
	project(':react-native-bugsnag').projectDir = new File(rootProject.projectDir, '../node_modules/react-native-bugsnag/android')
	```

2. Go to your app.gradle and add the following line in the end:
	```java
	dependencies {
	    compile fileTree(dir: 'libs', include: ['*.jar'])
	    compile 'com.android.support:appcompat-v7:23.0.1'
	    compile 'com.facebook.react:react-native:+'
	    //...whatever code
	
	    compile project(':react-native-bugsnag')	//<--Add this line
	}
	```
3. Go to your `manifest.xml` and add the following line within the application tag replacing `YOUR_API_KEY`:

	```
	<meta-data android:name="com.bugsnag.android.API_KEY" android:value="YOUR_API_KEY"/>
	```
	
4. Go to your `MainActivity.java` and add the following code:

	```
	import com.pintersudoplz.rnbugsnag.RNBugsnagPackage;
	```
	and then within your `getPackages` add the line with the comment
	
	```java
	@Override
    protected List<ReactPackage> getPackages() {
	    // ...whatever code
	    return Arrays.<ReactPackage>asList(
            new MainReactPackage(),            
            new RNBugsnagPackage()  //add this line
        );
    }
	```


## Usage


#### Unhandled errors (Automatic dispatch):
1. Include React Native Bugsnag in your React Native app:  

  ```js
  import Bugsnag from 'react-native-bugsnag';
  ```
 

2. Anywhere in the code:


  ```js
  Bugsnag();	//or Bugsnag({identifier:{userId: "aUserId", userEmail:"anEmail@domain.com", userFullname:"aFullName"}})
  ```

Congratulations!! 

At that point you have basic error reporting functionality working. Any unhandled javascript or native errors thrown will be reported to Bugsnag.


#### Handled errors (Manual dispatch):

You can manually create an exception using the following command:

  ```js
  Bugsnag.notify("TestExceptionName", "TestExceptionReason", "error");
  ```

The third parameter is the severity of the notification, it can be one of the following:

- "error"
- "warning"
- "info"

<!-- | method | parameters (body) | Description | Returns|
|---------------|-------------------------------------------------|--------------------------------------------------------------|-----|
| **setIdentifier** | {`userId`:string, `userEmail`: string, `userFullname`: string} |  This function sets the id of the user that we will be logging.| Promise | -->



## Symbolication

This is an important part of the process in order to get the actual method names and line numbers of the exceptions from iOS.

http://docs.bugsnag.com/platforms/ios-objc/symbolication-guide/


## TODO

- [x] Configure Bugsnag from JS.
- [ ] Handle different handled exceptions in JS.
- [x] Show line numbers (and method names?) in JS errors.
- [ ] Create some nice graphics for this README.
- [ ] Test RNPM installation process.
- [ ] Submit to js.coach and Bugsnag.
- [x] Fully integrate with Android.


[android-installation]: http://docs.bugsnag.com/platforms/android/#installation
[ios-installation]:     http://docs.bugsnag.com/platforms/ios-objc/#installation
