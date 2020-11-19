---
title: Best Rates API
position: 6
type: Android
description:
right_code: >-
---

We expose a method on the builder to retrieve the best rate for the products used.  

A BestDailyRatesListener is past into the getBestDailyRates method and will call the relevant methods once the relevant events have happen. 
 
A flag parameter is used to specify which products are required.

  ~~~kotlin      

   CartrawlerSDK.Builder()
     //..
     .getBestDailyRates(
              context = this,
              bestDailyRatesListener = object: CartrawlerSDK.BestDailyRatesListener{
                 override fun onReceiveBestDailyRate(type: Int, price: Double, currency: String) {
                 //Handle success result
                 }
     
                 override fun onError(type: Int, connectionError: CartrawlerSDK.ConnectionError) {
                 //Handle error result
                 }
     
                 override fun onNoResults(type: Int) {
                 //Handle empty result
                 }

  ~~~
