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

Calling `CartrawlerSDK.requestVehicles` method will trigger a vehicles request, the following parameters should be provided to the function:
- Context
- CTAvailabilityRequestData
- Limit (integer)
- CTVehicleAPISort (BEST_PRICE or RECOMMENDED)
- CTVehiclesListener

A `CTVehiclesListener` is passed into the `CartrawlerSDK.requestVehicles` method and the SDK will call the required methods once the relevant events have occurred.

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

CartrawlerSDK.requestVehicles(
    context = this,
    paramsData = requestData,
    numberOfVehicles = 5,
    sortType = CTVehicleAPISort.BEST_PRICE,
    listener = object : CTVehiclesListener { 
        override fun onNoResults() {
            /* do what you need here */
        }

        override fun onError(connectionError: CartrawlerSDK.ConnectionError) {
            /* do what you need here */
        }

        override fun onReceiveVehiclesDetails(vehicleDetails: List<VehicleDetails>) {
            /* do what you need here */
        }
    }
)
```

{: .note }
> The number of vehicles returned can be limited by setting the `numberOfVehicles` parameter.
> 
> The sort type can be either:  
> `CTVehicleAPISort.BEST_PRICE` - returns the cheapest cars in the list<br/>
> `CTVehicleAPISort.RECOMMENDED` - returns the CarTrawler recommended cars.

---

### VehicleDetails Class

```kotlin
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
)
```