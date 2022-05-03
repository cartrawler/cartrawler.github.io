---
layout: default
title: Best Daily Rates
parent: iOS SDK APIs
grand_parent: iOS
nav_order: 5
permalink: /docs/ios/apis/best-daily-rates
---

# Best Daily Rates

{: .no_toc }

The `requestBestDailyRate` function can be called to receive the cheapest vehicle's price for a given location based on the pick-up and drop-off dates. 

---

The SDK must be initialised, and a `CTAPIQueryParams` object with the necessary parameters must be set before calling this method.

#### Request best daily rates sample code
```java
import CarTrawlerSDK

let params = CTAPIQueryParams()  
params.delegate = self
params.clientID = "your clientID"
params.countryCode = "IE" // The country code associated with the device’s system region is used by default.
params.currencyCode = "EUR" // The currency associated with the device’s system region is used by default.
params.languageCode = "EN" // The language associated with the device’s system region is used by default.
params.iataCode = "DUB"
params.pickupDate = Date(timeIntervalSinceNow: 86400) // next day
params.dropOffDate = Date(timeIntervalSinceNow: 86400 * 3) // next day + 3 days
  
CarTrawlerSDK.sharedInstance().requestBestDailyRate(params)
```
---
The best daily rate (price and currency) will be returned in the `CarTrawlerSDKDelegate` function `didReceiveBestDailyRate`.

#### Receive best daily rates sample code

```java
// Called when best daily rate received
func didReceiveBestDailyRate(_ price: NSNumber, currency: String) {
}

// Called when best daily rate fails
func didFailToReceiveBestDailyRate(error: Error) {
}
```
