---
title: Vehicles API
position: 2
type: 
description:
---

Vehicles API is responsible for returning a wrapper response object of the list of vehicles returned from our backend. 

### Vehicle API - iOS

We expose a method to retrieve the vehicle list based on a sort type and limit you specify.

Calling the requestVehicles function will trigger a vehicles request based on the provided IATA or PickupLocationCode, pickup and dropoff date, also we limit the list returned to a set number of vehicles you specify.

For the sort type can be either:
- .bestPrice, which returns the cheapest cars in the list
- .recommended, which returns the cartrawler recommended cars.

Code sample

  ```swift
    import CarTrawlerSDK
  
    // This will trigger a new vehicles fetch, and the subsequent delegate callbacks
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
    params.numberOfVehicles = 20 // Default 0, it will return all vehicles
    params.sortType = .recommended // .recommended (Default) or .bestPrice

    CarTrawlerSDK.sharedInstance().requestVehicles(params)
  ```

A list of vehicle objects (CTVehicleDetails) will be returned in the CarTrawlerSDKDelegate function didReceiveVehicles.

  ``` swift
  // Called when best daily rate received
  func didReceiveVehicles(_ vehicles: [CTVehicleDetails]) {
  }

  // Called when best daily rate fails
  func didFailToReceiveVehicles(error: Error) {
  }
  ```

The vehicle object takes the following form:
```swift
class CTVehicleDetails: NSObject {
  let referenceId: String // vehicle reference ID 
  let name: String // vehicle name
  let orSimilar: String // localised "or similar" text
  let code: String // vehicle code 
  let vehicleAssetNumber: String // vehicle asset number
  let pictureURL: URL // vehicle image url 
  let passengerQuantity: Int // vehicle number of passengers
  let doorCount: Int // vehicle number of doors 
  let baggageQuantity: Int // vehicle number of bags
  let fuelType: String // vehicle fuel type
  let driveType: String // vehicle drive type
  let airConditionInd: Bool // vehicle is airconditioning included
  let transmissionType: String // vehicle transmission type 
  let size: String // ota size number
  let supplier: String // vehicle supplier name
  let supplierRating: NSNumber // vehicle supplier rating
  let supplierImageURL: URL // vehicle supplier logo
  let passengersText: String // localised "passengers" text
  let baggageText: String // localised "baggage" text
  let doorsCountText: String // localised "doors" text
  let transmissionText: String // localised "transmission" text
  let sizeText: String // localised "size" text
  let categortyText: String // localised category
  let price: NSNumber // vehicle price
  let pricePerDay: NSNumber // vehicle price per day
  let currencyCode: String // vehicle price currency code
}
```

### Vehicle API - Android

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
       

