---
layout: default
title: Best Daily Rates
parent: Android SDK APIs
grand_parent: Android
nav_order: 5
permalink: /docs/android/apis/best-daily-rates
---

# Best Daily Rates

{: .no_toc }

We expose a method on the builder to retrieve the best rate for the products used.   

---

## Request & Receive Best Daily Rates

A `BestDailyRatesListener` is passed into the `getBestDailyRates` method and will call the required methods once the relevant events have occurred. 
 
A flag parameter is used to specify which products are required.

```java      
CartrawlerSDK.Builder()
  //..
  .getBestDailyRates(
    context = this,
    bestDailyRatesListener = object: CartrawlerSDK.BestDailyRatesListener {
        override fun onReceiveBestDailyRate(type: Int, price: Double, currency: String) {
        //Handle success result
        }

        override fun onError(type: Int, connectionError: CartrawlerSDK.ConnectionError) {
        //Handle error result
        }

        override fun onNoResults(type: Int) {
        //Handle empty result
        }
```