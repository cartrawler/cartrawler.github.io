---
title: Current Location Search
position: 4
type: iOS
description:
right_code: >-

---


On the SDK’s Search Screen, we have added the option for users to search for vehicles using their current location upon tapping the pickup location text field. 

To make use of this feature, please ensure your plist file has the ```NSLocationWhenInUseUsageDescription``` (or ```NSLocationAlwaysUsageDescription``` for iOS10) key. 

We will need to use this when asking for permission to use location services within your app if it has not already been granted. 
