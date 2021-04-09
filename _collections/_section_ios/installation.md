---
title: Installation
position: 1
type: iOS
description: 'Supports iOS versions iOS 10 through to iOS 14'
right_code: >-

  {: title="Installation" }
---


**CocoaPods**

1. Include the CarTrawler private spec repository in your podfile: source 'https://github.com/cartrawler/cartrawler-ios-pods'
2. Include the pod in your podfile: pod 'CarTrawlerSDK'
3. From the terminal, run: pod install.

```ruby
      source 'https://github.com/CocoaPods/Specs.git'
      source 'https://github.com/cartrawler/cartrawler-ios-pods'
  
      platform :ios, '10.0'
  
      target 'CarTrawlerPartner' do
        pod 'CarTrawlerSDK', '~> 11.3.0'
      end
```



**Clear Storage**

CarTrawler SDK provides a method to clear local data stored (Recent searches, Booking summary card), the data is stored in NSUserDefaults and it's cleared using the method below.

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


