---
layout: default
title: iOS Integration
parent: Flutter
nav_order: 2
has_children: false
permalink: docs/flutter/ios-integration/
---

# Flutter iOS Integration
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

## Add the CarTrawler Pod

To add the CarTrawlerSDK to your app, you will need Cocoapods. 

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

## Add the iOS Native implementation
In order to receive the message on the method channel sent from Flutter this needs to be caught in your iOS platform code. The CarTrawler iOS SDK must be initialised in your AppDelegate.

```java
import CarTrawlerSDK
// In application(_:didFinishLaunchingWithOptions:)

CarTrawlerSDK.sharedInstance().initialiseSDK(with: nil,
                                customParameters: nil,
                                production: false)
        
let context = CTContext(implementationID: "your implementation ID", 
                        clientID: "your client ID", 
                        flow: .standAlone)
        
let controller = window?.rootViewController as! FlutterViewController
context.delegate = controller
        
let sdkChannel = FlutterMethodChannel(name: "com.cartrawler.rental/carTrawlerSDK", binaryMessenger: controller.binaryMessenger)
        
sdkChannel.setMethodCallHandler({
    (call: FlutterMethodCall, result: @escaping FlutterResult) -> Void in
    switch call.method {
    case "carTrawlerSDK":
        CarTrawlerSDK.sharedInstance().present(from: controller, context: context)
        break
    default:
        result(FlutterMethodNotImplemented)
    }
})
```

After completing the above steps and running your project, you will be able to initiate the CarTrawler booking flow from your Flutter project.