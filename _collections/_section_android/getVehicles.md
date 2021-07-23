---
title: Vehicles API
position: 8
type: Android
description:
right_code: >
---

We expose a method on the builder to retrieve the vehicle list based on a sort type you specify. We limit the list returned to a set number of vehicles you specify. For the sort type can be either 
- FLAG_BEST_PRICE, which returns the cheapest cars in the list or FLAG_RECOMMENDED, which returns the cartrawler recommended cars.


A VehiclesListener is past into the getVehicleDetails method and the SDK will call the relevant methods once the relevant events have happen.

  ~~~kotlin      

   CartrawlerSDK.Builder()
           //..
           .getVehicles(
               context = this,
               numberOfVehicles = 10, //Range from 1 to 10
               sortType = CartrawlerSDK.Builder.FLAG_BEST_PRICE, // OR CartrawlerSDK.Builder.FLAG_RECOMMENDED
                   vehiclesListener = object: CartrawlerSDK.VehiclesListener {
                       override fun onReceiveVehicleDetails(vehicleDetails: List<VehicleDetails>) {
                        //Handle success result
                       }
   
                       override fun onError(type: Int, connectionError: CartrawlerSDK.ConnectionError) {
                        //Handle error result
                       }
   
                       override fun onNoResults(type: Int) {
                        //Handle no results
                       }


  ~~~

The vehicle object takes the following form:

```kotlin
       @Parcelize
       data class VehicleDetails(
           //OTA
            val referenceId: String,
            val name: String,
            val orSimilar: String,
            val code: String,
            val vehicleAssetNumber: String,
            val pictureURL: String,
            val passengerQuantity: Int,
            val doorCount: Int?,
            val baggageQuantity: Int
            val fuelType: String,
            val driveType: Stringl,
            val airConditionInd: Boolean,
            val transmissionType: String,
            val size: String,
       
            //Supplier
            val supplier: String,
            val supplierRating: Double,
            val supplierImageURL: String,
       
            //Widget localization values
            val passengersText: String,
            val baggageText: String?,
            val doorsCountText: String,
            val transmissionText: String,
            val categoryText: String,
            val sizeText: String
       
            //Price
            val price: Double,
            val pricePerDay: Double,
            val currencyCode: String
       ) : Parcelable
```
       

