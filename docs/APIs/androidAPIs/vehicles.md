---
layout: default
title: Vehicles
parent: Android SDK APIs
grand_parent: APIs
nav_order: 2
permalink: /docs/api/android/vehicles/
---

# Vehicles

{: .no_toc }

The Vehicles API is responsible for returning a wrapper response object of the list of vehicles returned from our backend. 

---

We expose a method on the builder to retrieve the vehicle list based on a sort type and limit you specify.

Calling the `getVehicles` function will trigger a vehicles request based on the provided pick-up and drop-off dates, and an IATA or pick-up location code. 

A `VehiclesListener` is passed into the `getVehicleDetails` method and the SDK will call the required methods once the relevant events have occurred.

```java
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
```

{: .note }
The number of vehicles returned can be limited by setting the `numberOfVehicles` parameter. <br /><br />
The sort type can be either: <br /> `FLAG_BEST_PRICE`, which returns the cheapest cars in the list <br /> or `FLAG_RECOMMENDED`, which returns the CarTrawler recommended cars.

---

### VehicleDetails Class

```java
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
    val sizeText: String,

    //Price
    val price: Double,
    val pricePerDay: Double,
    val currencyCode: String
) : Parcelable
```