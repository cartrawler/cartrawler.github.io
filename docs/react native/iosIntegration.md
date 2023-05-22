---
layout: default
title: iOS Integration
parent: React Native
nav_order: 2
has_children: false
permalink: docs/react-native/ios-integration/
---

# iOS Integration
{: .no_toc }

To integrate the iOS SDK into your React Native app, open the iOS folder inside your React Native project using Xcode, and follow the steps listed below.

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
1. <a href="/docs/react-native/ios-integration/#resources">Resources</a>
</details>

---

## Add our Private Spec Repository and CocoaPod to your Podfile <br />

1. Include the CarTrawler private spec repository in your podfile: source 'https://github.com/cartrawler/cartrawler-ios-pods'
2. Include the pod in your podfile: `pod 'CarTrawlerSDK'`
3. From the terminal, run: pod install.

```python
source 'https://github.com/CocoaPods/Specs.git'
source 'https://github.com/cartrawler/cartrawler-ios-pods'
  
platform :ios, '11.0'
use_frameworks!
inhibit_all_warnings!
  
target 'CarTrawlerPartner' do
  pod 'CarTrawlerSDK', '~> (add latest version here from Release Notes section)'
end
```

--- 

## Initialise the SDK in your App Delegate and create a View Controller to present the SDK From

```java
import CarTrawlerSDK

// In AppDelegate application(_:didFinishLaunchingWithOptions:)
RCTBridge *bridge = [[RCTBridge alloc] initWithDelegate:self launchOptions:launchOptions];
RCTRootView *rootView = [[RCTRootView alloc] initWithBridge:bridge
                                                 moduleName:@"ReactDemo"
                                          initialProperties:nil];
    
[CarTrawlerSDK.sharedInstance initialiseSDKWithStyle:nil 
                                    customParameters:nil 
                                          production:NO];
  
self.window = [[UIWindow alloc] initWithFrame:[UIScreen mainScreen].bounds];
SDKViewController *rootViewController = [SDKViewController new];
rootViewController.view = rootView;
self.window.rootViewController = rootViewController;
[self.window makeKeyAndVisible];
```

```java
// In SDKViewController.m
import CarTrawlerSDK

- (void)viewDidLoad {
  [super viewDidLoad];
}

- (void)launchStandalone {
  CTContext *context = [[CTContext alloc] initWithImplementationID:@"your implementation ID" 
                                                          clientID:@"your client ID" 
                                                              flow:CTFlowTypeStandalone];
  context.delegate = self;
  [CarTrawlerSDK.sharedInstance presentFromViewController:self context:context];
}

- (void)launchInPath {
  CTContext *context = [[CTContext alloc] initWithImplementationID:@"your implementation ID" 
                                                          clientID:@"your client ID" 
                                                              flow:CTFlowTypeInPath];
  context.countryCode = @"IE";
  context.currencyCode = @"EUR";
  context.languageCode = @"EN";
  context.pickupLocation = @"DUB";
  context.pickupDate = [NSDate dateWithTimeIntervalSinceNow:86400];
  context.delegate = self;
  [CarTrawlerSDK.sharedInstance setContext:context];
  [CarTrawlerSDK.sharedInstance presentInPathFromViewController:self];
}
```
--- 

## Expose Methods to React Native

Inside your iOS project, create a new class that implements the RCTBridgeModule protocol, this class will be the Module that we will expose methods to the React Native side.

```java
/**
 * CTSDKStarterModule.h
 */

#import <Foundation/Foundation.h>
#import <React/RCTBridgeModule.h>

NS_ASSUME_NONNULL_BEGIN

@interface CTSDKStarterModule : NSObject <RCTBridgeModule>

+ (void)startBookingFlow:(BOOL)isInPath;

@end

NS_ASSUME_NONNULL_END

```

```java
 
/**
 * CTSDKStarterModule.m
 */

@implementation CTSDKStarterModule

RCT_EXPORT_METHOD(startBookingFlow:(BOOL)isInPath) {
  [CTSDKStarterModule startBookingFlow:isInPath];
}

+ (void)startBookingFlow:(BOOL)isInPath {
  
  dispatch_async(dispatch_get_main_queue(), ^{
    SDKViewController *rootController = (SDKViewController *)[[(AppDelegate *)
                                                               [[UIApplication sharedApplication]delegate] window] rootViewController];
    if(isInPath) {
      [rootController launchInPath];
    } else {
      [rootController launchStandalone];
    }
    
  });
  
}

RCT_EXPORT_MODULE();

@end

```

---

## Receive Events from the SDK in JavaScript

To send information back to Javascript such as a booking result, we create another class that extends RCTEventEmitter on the iOS side.

```java
/**
 * EventEmitterModule.h
 */

#import <React/RCTEventEmitter.h>

NS_ASSUME_NONNULL_BEGIN

@interface EventEmitterModule : RCTEventEmitter <RCTBridgeModule>

- (void)sendEventName:(NSString *)eventName body:(id)body;
- (bool)hasListeners;

@end

NS_ASSUME_NONNULL_END
```

```java
/**
 * EventEmitterModule.m
 */

@implementation EventEmitterModule
{
  bool hasListeners;
}

RCT_EXPORT_MODULE(EventEmitterModule);

+ (id)allocWithZone:(NSZone *)zone {
  static EventEmitterModule *sharedInstance = nil;
  static dispatch_once_t onceToken;
  dispatch_once(&onceToken, ^{
    sharedInstance = [super allocWithZone:zone];
  });
  return sharedInstance;
}

- (NSArray<NSString *> *)supportedEvents {
    NSArray<NSString *> *arrayOfEvents = [[NSArray alloc]initWithObjects:@"BookingEvent", nil];
    return arrayOfEvents;
}

// Will be called when this module's first listener is added.
- (void)startObserving {
  hasListeners = YES;
  // Set up any upstream listeners or background tasks as necessary
}

// Will be called when this module's last listener is removed, or on dealloc.
- (void)stopObserving {
  hasListeners = NO;
  // Remove upstream listeners, stop unnecessary background tasks
}

- (bool)hasListeners {
  return hasListeners;
}

- (void)sendEventName:(NSString *)eventName body:(id)body {
  if (hasListeners) {
    [self sendEventWithName:eventName body:body];
  }
}

@end
```

We also have to implement the CarTrawlerSDK didReceiveReservationDetails delegate method in our SDKViewController: 

```java 
// In SDKViewController.m

// #import ... 

@interface SDKViewController ()<CarTrawlerSDKDelegate>
@end

@implementation SDKViewController

// - (void)... 

- (void)didReceiveReservationDetails:(CTReservationDetails *)reservationDetails {
  EventEmitterModule *eventModule = [EventEmitterModule allocWithZone:nil];
  NSString *bookingIDString = [NSString stringWithFormat:@"BookingID: %@", reservationDetails.resId];
  [eventModule sendEventName:@"BookingEvent" body:bookingIDString];
}

@end
```

{: .note }
Make sure to set the CTContext delegate to self(the view controller): <br />
context.delegate = self;

On the Javascript side we can then subscribe to this event:

```javascript
const eventEmitter = new NativeEventEmitter(eventEmitterModule);
eventEmitter.addListener("BookingEvent", (params) => {
  alert("Hello from React Native side, the booking details were received"+ "\n\n" + params);
});
```

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
        <Text style={styles.title}>Reactive Native Screen</Text>
        <View style={styles.buttonContainer}>
          <Button color="#004958" style={styles.btn}
            onPress={() => {
              ctSDKStarterModule.startBookingFlow(false);
            }
            }
            title='Start Standalone flow'
          />
          <Button color="#004958" style={styles.btn}
            onPress={() => {
              ctSDKStarterModule.startBookingFlow(true)
            }
            }
            title='Start InPath flow'
          />
        </View>
      </View>
    );
  }
}
```

After completing the above steps and running your project, you will be able to initiate the CarTrawler booking flow from your React Native project and receive booking details once the flow is complete.

---

## Resources

{: .no_toc}

* <a href="https://github.com/cartrawler/cartrawler-react-android-demo">Sample code</a> <br/>  
* More details on <a href="https://reactnative.dev/docs/native-modules-intro">Native Modules</a> <br/> 
