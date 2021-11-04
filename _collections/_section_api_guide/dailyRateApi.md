---
title: Best Rates API
position: 4
type: 
description:
right_code: >-
---

Daily Rates API is responsible for returning a wrapper response object of the best rate for a car rental.

### Daily Rates API - iOS
Calling the requestBestDailyRate function will trigger a best daily rate request based on the provided IATA or PickupLocationCode and pickup and dropoff dates.

  ``` swift
  
    import CarTrawlerSDK
  
    // This will trigger a new best daily rate fetch, and the subsequent delegate callbacks
    // The SDK must be initialised, and a CTAPIQueryParams object with the necessary parameters must be set before calling this method
  
    let params = CTAPIQueryParams()  
    params.delegate = self
    params.clientID = "12345"
    params.countryCode = "IE"
    params.currencyCode = "EUR"
    params.languageCode = "EN"
    params.iataCode = "DUB"
    params.pickupDate = Date(timeIntervalSinceNow: 86400) // next day
    params.dropOffDate = Date(timeIntervalSinceNow: 86400 * 3) // next day + 3 days
  
    CarTrawlerSDK.sharedInstance().requestBestDailyRate(params)
  ```

The best daily rate (price and currency) will be returned in the CarTrawlerSDKDelegate function didReceiveBestDailyRate.


  ``` swift
  // Called when best daily rate received
  func didReceiveBestDailyRate(_ price: NSNumber, currency: String) {
  }

  // Called when best daily rate fails
  func didFailToReceiveBestDailyRate(error: Error) {
  }
  ```

### Daily Rates API - Android
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
