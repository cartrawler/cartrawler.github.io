---
layout: default
title: Integration
parent: iOS
nav_order: 1
---

# Integration


To add the CarTrawlerSDK to your app, you will need Cocoapods. 

---

## Cocoapods

1. Include the CarTrawler private spec repository in your podfile: source 'https://github.com/cartrawler/cartrawler-ios-pods'
2. Include the pod in your podfile: `pod 'CarTrawlerSDK'`
3. From the terminal, run: pod install.

```python
source 'https://github.com/CocoaPods/Specs.git'
source 'https://github.com/cartrawler/cartrawler-ios-pods'
  
platform :ios, '10.0'
use_frameworks!
inhibit_all_warnings!
  
target 'CarTrawlerPartner' do
  pod 'CarTrawlerSDK', '~> 12.7.1'
end
```

## App Permissions

On the SDK’s Search Screen, we have added the option for users to search for vehicles using their current location upon tapping the pick-up location text field.

To make use of this feature, please ensure your plist file has the `NSLocationWhenInUseUsageDescription` (or `NSLocationAlwaysUsageDescription` for iOS10) key.

We will need to use this when asking for permission to use location services within your app if it has not already been granted.
