---
title: Installation
position: 1
type: iOS
description: 'Supports iOS versions iOS 10 through to iOS 15'
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
      use_frameworks!
      inhibit_all_warnings!
  
      target 'CarTrawlerPartner' do
        pod 'CarTrawlerSDK', '~> 11.9.0'
      end
```


