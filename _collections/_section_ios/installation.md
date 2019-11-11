---
title: Installation
position: 0
type: iOS
description: 'Supports iOS versions 9, 10, 11 and 12'
right_code: |-
  ~~~ruby
    source 'https://github.com/CocoaPods/Specs.git'
    source 'https://github.com/cartrawler/cartrawler-ios-pods'

    platform :ios, '9.0'

    target 'CarTrawlerPartner' do
      pod 'CarTrawlerSDK', '~> 9.1.0'
    end
  ~~~
    {: title="Podfile" }
  
  ``` swift
  // Clear local storage
  CarTrawlerSDK.sharedInstance().clearStorage { (error) in
    if let error = error {
        print("Storage not cleared with error:" + error.localizedDescription)
    } else {
        print("Storage cleared with success")
    }
  }
  ```
  {: title="clearStorage" }
  
  ```swift
  
  ```

---


**CocoaPods**

1. Include the CarTrawler private spec repository in your podfile: source 'https://github.com/cartrawler/cartrawler-ios-pods'
2. Include the pod in your podfile: pod 'CarTrawlerSDK'
3. From the terminal, run: pod install.



**Clear Storage**

CarTrawler SDK provides a method to clear local data stored (Recent searches, Booking summary card), the data is stored in application NSUserDefaults and it's cleared using the method below.

<b>It's not posible to clear the storage while the SDK is being presented on screen.</b>

```swift
  // Clear local storage
  CarTrawlerSDK.sharedInstance().clearStorage { (error) in
    if let error = error {
        print("Storage not cleared with error:" + error.localizedDescription)
    } else {
        print("Storage cleared with success")
    }
  }
```


