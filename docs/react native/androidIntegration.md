---
layout: default
title: Android Integration
parent: React Native
nav_order: 1
has_children: false
permalink: docs/react-native/android-integration/
---

# Android Integration
{: .no_toc }

To integrate the Android SDK's within your React Native app, open the Android folder inside your React Native project using Android Studio and follow the steps listed below.

<details open markdown="block">
  <summary>
    Steps
  </summary>
  {: .text-delta }
1. TOC
{:toc}
</details>

<details markdown="block">
  <summary>
    Optional Extras
  </summary>
  {: .text-delta }
1. <a href="/docs/react-native/android-integration/#resources">Resources</a>
</details>

---

## Add our Maven Repository and the Artifactory Credentials

* Locate the file gradle.properties in your pc under ~/.gradle/gradle.properties. 
* If the file does not exist create one and add the following credentials to it replacing the placeholder with the right values.

```groovy
nativeArtifactoryUsername=placeholder
nativeArtifactoryPassword=placeholder
```

{: .note}
If you do not have these credentials contact you manager, IT or another developer in your team.

--- 
## Add the CarTrawler Dependency to build.gradle
Please use the version number sent to you by the CarTrawler team

```groovy 
implementation "com.cartrawler.android:car-rental:$latestVersion" 
```

If not migrated yet, it is recommended to migrate the project to Android X: 
<a href="https://developer.android.com/jetpack/androidx/migrate">Migration guide</a> 

---

## Expose Methods to React Native
Inside your Android project, create a new class that extends ReactContextBaseJavaModule, this class will be the Module that we will expose methods to the React Native side.

```kotlin
/**
 * Expose Java to JavaScript. Methods annotated with [ReactMethod] are exposed.
 */
class CTSDKStarterModule(reactContext: ReactApplicationContext?) :
    ReactContextBaseJavaModule(reactContext) {
 
    /**
     * @return the name of this module. This will be the name used to `require()` this module
     * from JavaScript.
     */
    override fun getName(): String {
        return "CTSDKStarterModule"
    }
 
//Example of method we can expose to Javascript
    @ReactMethod
    fun startBookingFlow() {
        val activity = currentActivity
        if (activity != null) {
            val intent = Intent(activity, CTFlowActivity::class.java)
            activity.startActivity(intent)
        }
    }
}

```
Create a class that implements ReactPackage and overrides the 'createNativeModules' method, so we can add our new module to the modules list.

```kotlin
/**
 * Exposes [CTSDKStarterModule] and [EventEmitterModule]  to JavaScript.
 * One [ReactPackage] can expose any number of [NativeModule]s.
 */
internal class CTSDKStarterReactPackage : ReactPackage {
    override fun createNativeModules(reactContext: ReactApplicationContext): List<NativeModule> {
        val modules: MutableList<NativeModule> = ArrayList()
        modules.add(CTSDKStarterModule(reactContext))
        modules.add(EventEmitterModule(reactContext))
        return modules
    }
 
    override fun createViewManagers(reactContext: ReactApplicationContext): List<ViewManager<*, *>> {
        return emptyList()
    }
}
```

Update MainApplication to include the new package. The one created above.

```kotlin
@Override
protected List<ReactPackage> getPackages() {
  List<ReactPackage> packages = new PackageList(this).getPackages();
  // Packages that cannot be autolinked yet can be added manually here, for example:
  packages.add(new CTSDKStarterReactPackage());
  return packages;
}
```

* Save, clean and rebuild project.

---

## Access Functions in JavaScript

We should now be able to access the function created inside the Module, on the Javascript side.

```javascript
...
import {
 NativeModules
} from 'react-native';
 
const ctSDKStarterModule = NativeModules.CTSDKStarterModule;
 
export default class CTSDKDemoComponent extends Component {
  render() {
    return (
      <View style={styles.container}>
        <View style={styles.buttonContainer}>
          <Button
            onPress={() => {
                  ctSDKStarterModule.startBookingFlow()
              }
            }
            title='Start Booking flow'
          />
        </View>
      </View>
    );
  }
}
...
```

---

## Receive Events from the SDK in JavaScript

To send information back to Javascript such as a booking result, we create another class that extends ReactContextBaseJavaModule on the Android side. 

```kotlin
class EventEmitterModule(reactContext: ReactApplicationContext?) :
    ReactContextBaseJavaModule(reactContext) {

    override fun initialize() {
        super.initialize()
        eventEmitter = reactApplicationContext.getJSModule(
            RCTDeviceEventEmitter::class.java
        )
    }

    /**
     * @return the name of this module. This will be the name used to `require()` this module
     * from JavaScript.
     */
    override fun getName(): String {
        return "EventEmitter"
    }

    override fun getConstants(): Map<String, Any>? {
        val constants: MutableMap<String, Any> = HashMap()
        constants["MyEventName"] = "BookingEvent"
        return constants
    }

    companion object {
        private lateinit var eventEmitter: RCTDeviceEventEmitter

        /**
         * To pass a JavaScript object instead of a simple string, create a [WritableNativeMap] and populate it.
         */
        fun emitEvent(message: String) {
            eventEmitter.emit("BookingEvent", message)
        }
    }
}
```

On the Javascript side we can then subscribe to this event:

```javascript
const eventEmitter = new NativeEventEmitter(eventEmitterModule);
eventEmitter.addListener("BookingEvent", (params) => {
  alert("Hello from React Native side, the booking details were received"+ "\n\n" + params);
});
```

---

## Initiate the Flow 
To trigger the CarTrawler flow and send the result back to the React Native side, create an activity in your Android project called CTFlowActivity.kt.

```kotlin
class CTFlowActivity : ReactActivity() {

    @CallSuper
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.ctflow_activity)
        init()
    }

    private fun init(){
        CartrawlerSDK.Builder()
            .setRentalStandAloneClientId(CLIENT_ID)
            .setEnvironment(ENVIRONMENT)
            .setFlightNumberRequired(false)
            .setLogging(BuildConfig.DEBUG)
            .setTheme(R.style.AppTheme)
            .setDarkModeConfig(AppCompatDelegate.MODE_NIGHT_FOLLOW_SYSTEM)
            .startRentalStandalone(this@CTFlowActivity, REQUEST_CODE_STANDALONE)
       
     findViewById<View>(R.id.go_back_button).setOnClickListener { onBackPressed() }
    }

    override fun onActivityResult(requestCode: Int, resultCode: Int, data: Intent?) {
        super.onActivityResult(requestCode, resultCode, data)

        val gson = GsonBuilder().setPrettyPrinting().create()

        if (resultCode == RESULT_OK && requestCode == REQUEST_CODE_STANDALONE) {
            val reservationExtra =
                data?.getParcelableExtra<ReservationDetails>(CartrawlerSDK.RESERVATION_DETAILS)
            if (reservationExtra != null) {
                reservation = gson.toJson(reservationExtra)
                sendBookingResult()
            }

        }else if(resultCode == RESULT_CANCELED){
            this@CTFlowActivity.finish()
        }
    }

    private fun sendBookingResult(){
        EventEmitterModule.emitEvent(reservation)
    }
```

After completing the above steps and running your project, you will be able to initiate the CarTrawler booking flow from your React Native project and receive booking details once the flow is complete.

---

## Resources
{: .no_toc}

* <a href="https://github.com/cartrawler/cartrawler-react-android-demo">Sample code</a> <br/>  
* More details on <a href="https://reactnative.dev/docs/native-modules-intro">Native Modules</a> <br/> 