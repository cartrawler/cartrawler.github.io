---
layout: default
title: Booking Reservations
parent: Android SDK APIs
grand_parent: APIs
nav_order: 6
permalink: /docs/api/android/reservations/
---

# Booking Reservations

{: .no_toc }

Car rental booking reservations can be fetched using the `CartrawlerSDK.requestReservationDetails` function. 

---

Calling `CartrawlerSDK.requestReservationDetails` method will trigger a request to get the booking reservation, the following parameters should be provided to the function: 

- Context,
- CTReservationRequestData,
- CTRequestReservationListener

```kotlin
val requestData = CTReservationRequestData(
    clientId = "<your_client_id>",
    reservationId = "IE123456789", 
    reservationUserId = "user@email.com", // resUid (reservation user id) can be used instead
)

CartrawlerSDK.requestReservationDetails(
    context = this,
    paramsData = requestData,
    requestReservationListener = object : CTRequestReservationListener {

        override fun onNoResults() {
            /* do what you need here */
        }

        override fun onError(connectionError: CartrawlerSDK.ConnectionError) {
            /* do what you need here */
        }

        override fun onReceiveReservationDetails(reservationDetails: ReservationDetails) {
            /* do what you need here */
        }
    }
)
```

These properties can be found in the `ReservationDetails` object that is returned upon successful completion of the standalone flow, on `onActivityForResult` e.g. `getStringExtra(CartrawlerSDK.RESERVATION)`

The reservations object takes the following form:

```java
data class ReservationDetails (
   val status: String,// In this scernario it will be confirmed
   val givenName: String, // first name
   val surname: String, // Surname
   val resId: String, // Reservation ID
   val resuid: String, // resuid, use this along with the resId to retrieve the booking later
   val pickUpDateTime: String, //The date & time of pick-up
   val returnDateTime: String,  //The date & time of pick-up 
   val pickupLocation: LocationDetails, //Location details of pick-up
   val returnLocation: LocationDetails, //Location details of pick-up
   val insurance: Insurance, // Insurance, null if none attached
   val rentalInfo: RentalInfo, // Information on reservation costs
   val vehicle: VehicleDetails
) // Information on the selected vehicle
  
data class LocationDetails (
   val atAirport: Boolean, // Location at Airport? (boolean)
   val iataCode: String,  // IATA Code (if airport)
   val code: Int,  // Unique Location Code (code type is internal to CarTrawler)
   val name: String, // Text description of location
   val address: Address, // Postal address of location
   val phoneNumber: String // Vendor contact number
)

data class Insurance (
   val company: String, // Insurance company name
   val id: String, // Code of offered insurance product
   val cost: Double, // base cost
   val currency: String, // base currency
   val createDate: String, // date insurance was purchased
   val costCharge: Double, // Cost converted into charged currency (presented currency)
   val currencyCharge: String, // the presented currency to the customer
   val companyLogo: String, // a link to the company logo
   val companyPolicyURL: String, // a link to the policy terms and conditions
   val text: String, // A marketing description of the insurance (markup)
 )

data class RentalInfo (
   val cost: Double, // base cost
   val currency: String, // base currency
   val customerCost: Double, // /cost in the currency of the customer
   val customerCurrency: String // the presented currency to the customer
 )

data class Address (
   val addressLine: String, // Post adddress of location
   val countryNameCode: String // 2 letter country code.
 )
   
data class VehicleDetails(
   //OTA values
   val referenceId: String,
   val name: String,
   val orSimilar: String,
   val code: String,
   val vehicleAssetNumber: String,
   val pictureURL: String,
   val passengerQuantity: Int,
   val doorCount: Int?,
   val baggageQuantity: Int,
   val fuelType: String,
   val driveType: String,
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