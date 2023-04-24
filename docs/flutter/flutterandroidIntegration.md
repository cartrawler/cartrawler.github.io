---
layout: default
title: Android Integration
parent: Flutter
nav_order: 1
has_children: false
permalink: docs/flutter/android-integration/
---

# Flutter Android Integration
{: .no_toc }

To integrate the CarTrawler SDK with your Flutter app, please follow these steps.

<details open markdown="block">
  <summary>
    Steps
  </summary>
  {: .text-delta }
1. TOC
{:toc}
</details>

---

## Add the CarTrawler Maven Repository and the Artifactory Credentials

* In the Android platform section of your project add the repository details to android/build.gradle

```groovy
repositories {
    maven {
        url "http://artifactory.cartrawler.com/artifactory/libs-release-local"
        credentials {
            username = "your_username_here"
            password = "your_password_here"
        }
    }
}
```

{: .note}
If you do not have these credentials yet then get in touch with CarTrawler.

--- 
## Add the CarTrawler Dependency to build.gradle
Please use the version number sent to you by the CarTrawler team. For compatibility with older Android versions you must also add a desugaring library.

```groovy 
implementation "com.cartrawler.android:car-rental:$latestVersion" 
coreLibraryDesugaring 'com.android.tools:desugar_jdk_libs:$latestVersion'
```
---

## Create a Method Channel
The Method Channel is the mechanism used to pass information between your app can the CarTrawler SDK. The channel identifier should be unique.

```dart
static const platform = MethodChannel('com.cartrawler.rental/carTrawlerSDK');
```
## Invoke a method on the Platform Channel
To communicate between the client Flutter app and the host platform the method channel already created is invoked from Flutter code.

```dart
/**
 * Call this from some UI code in your Flutter app
 */
 Future _loadCarTrawlerSDK() async {
    try {
        await platform //defined in the previous step
                .invokeMethod('carTrawlerSDK')
            .then((result) {
                print ("Result received");
            });
    } on PlatformException catch (e) {
        print (e.message);
    }
}
```

## Add the Android Native implementation
In order to receive the message on the method channel sent from Flutter this needs to be caught in your Android platform code. This is combined with the call to build and start the Cartrawler SDK as outlined in the main body of this doccumentation.

```kotlin
class MainActivity: FlutterActivity() {

    //This matches the definition in your Dart code
    private val METHOD_CHANNEL = "com.cartrawler.rental/carTrawlerSDK"
    //Local variable to listen to the method result
    private lateinit var result: MethodChannel.Result

    override fun configureFlutterEngine(@NonNull flutterEngine: FlutterEngine) {
        super.configureFlutterEngine(flutterEngine)
        MethodChannel(
            flutterEngine.dartExecutor.binaryMessenger,
            METHOD_CHANNEL
        ).setMethodCallHandler { call, reservationResult ->
            if (call.method == "carTrawlerSDK") {
                //set local result variable to our method result variable
                 result = reservationResult
                //Sample call to start SDK flow in standalone mode
                CartrawlerSDK.start(
                activity = this,
                requestCode = REQUEST_CODE,
                ctSdkData = sdkData,
                flow = CTSdkFlow.Standalone()
                )
            } else {
                //other call methods can be defined
            }
        }
    }

    //Wait for the result that is returned by SDK 
    override fun onActivityResult(requestCode: Int, resultCode: Int, data: Intent?) {
    if (resultCode == Activity.RESULT_OK) {
        if (requestCode == REQUEST_CODE) {
            val reservationData = data?.getParcelableExtra<ReservationDetails>(CartrawlerSDK.RESERVATION_DETAILS)
            //Send the reservation number back to Dart code
            result.success(reservationData)
          }
       }
    }

     companion object{
        const  val REQUEST_CODE = 567
    }

}
```

After completing the above steps and running your project, you will be able to initiate the CarTrawler booking flow from your Flutter project and receive booking details once the flow is complete.