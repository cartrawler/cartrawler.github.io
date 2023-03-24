---
layout: default
title: Best Daily Rates
parent: Android SDK APIs
grand_parent: APIs
nav_order: 5
permalink: /docs/api/android/best-daily-rates
---

# Best Daily Rates

{: .no_toc }

We expose a method on CartrawlerSDK to retrieve the best rate for the products used.   

---

## Request & Receive Best Daily Rates

Calling `CartrawlerSDK.requestBestDailyRates` method will trigger a request to get the best daily price, the following parameters should be provided to the function:

- Context
- CTAvailabilityRequestData
- CTBestDailyRatesListener

A `CTBestDailyRatesListener` is passed into the `CartrawlerSDK.requestBestDailyRates` method and will call the required methods once the relevant events have occurred. 

```kotlin
val requestData = CTAvailabilityRequestData(
    clientId = "<your_client_id>",
    country = Locale.getDefault().country,
    currency = Currency.getInstance(Locale.getDefault()).currencyCode,
    iataCode = "DUB",
    pickupDateTime = LocalDateTime.of(2023, 5, 10, 10, 0),
    dropOffDateTime = LocalDateTime.of(2023, 5, 15, 10, 0),
    ctSdkEnvironment = CTSdkEnvironment.DEVELOPMENT, // or CTSdkEnvironment.PRODUCTION
    logging = false // true if you want to log the errors in logcat
)

CartrawlerSDK.requestBestDailyRates(
    context = this,
    paramsData = requestData,
    listener = object : CTBestDailyRatesListener {
        override fun onReceiveBestDailyRate(price: Double, currency: String) {
            /* do what you need here */ 
        }

        override fun onError(connectionError: CartrawlerSDK.ConnectionError) {
            /* do what you need here */ 
        }

        override fun onNoResults() {
            /* do what you need here */ 
        }
    }
)
```