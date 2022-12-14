---
layout: default
title: Best Daily Rates
parent: iOS SDK APIs
grand_parent: APIs
nav_order: 5
permalink: /docs/api/ios/best-daily-rates
---

# Best Daily Rates

{: .no_toc }

The `requestBestDailyRate` function can be called to receive the cheapest vehicle's price for a given location based on the pick-up and drop-off dates. 

---

## Request Best Daily Rates

The SDK must be initialised, and a `CTAPIQueryParams` object with the necessary parameters must be set before calling this method.

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

## Receive Best Daily Rates 

The best daily rate (price and currency) will be returned in the `CarTrawlerSDKDelegate` function `didReceiveBestDailyRate`.

```java
// Called when best daily rate received
func didReceiveBestDailyRate(_ price: NSNumber, currency: String) {
}

// Called when best daily rate fails
func didFailToReceiveBestDailyRate(error: Error) {
}
```
