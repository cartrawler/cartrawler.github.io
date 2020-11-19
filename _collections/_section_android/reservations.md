---
title: Reservations API
position: 8
type: Android
description:
right_code: >-
---

<h5>Get Booking Reservation</h5>

Calling the requestReservationDetails function will trigger a vehicles request based on a resID and email (hashed) or resuid. 

  ~~~java
                 CartrawlerSDK.Builder()
                 //..
                 builder.requestReservationDetails(
                         context = this,
                         resId = resid,
                         resuid = resuid,
                         email = null,
                         requestReservationDetailsListener = object : CartrawlerSDK.RequestReservationDetailsListener {
     
                             @SuppressLint("SetTextI18n")
                             override fun onNoResults() {
                                //Handle no results
                             }
     
                             override fun onError(connectionError: CartrawlerSDK.ConnectionError) {
                               //Handle error result
                             }
     
                             @SuppressLint("SetTextI18n")
                             override fun onReceiveReservationDetails(reservationDetails: ReservationDetails) {
                               //Handle success result
                             }
                         })
  ~~~

These properties can be found in the ReservationDetails object that is returned upon successful completion of the standalone flow, on onActivityForResult e.g. getStringExtra(CartrawlerSDK.RESERVATION)

The reservations object takes the following form:

```kotlin
    
    data class ReservationDetails (
       val status: String,// In this scernario it will be confirmed
       val givenName: String, // first name
       val surname: String, // Surname
       val resId: String, // Reservation ID
       val resuid: String, // resuid, use this along with the resId to retrieve the booking later
       val pickUpDateTime: GregorianCalendar, //The date & time of pickup
       val returnDateTime: GregorianCalendar,  //The date & time of pickup 
       val pickupLocation: LocationDetails, //Location details of pickup
       val returnLocation: LocationDetails, //Location details of pickup
       val insurance: Insurance, // Insurance, null if none attached
       val rentalInfo: RentalInfo, // Information on reservation costs
       val vehicle: VehicleDetails) // Information on the selected vehicle
      
        data class LocationDetails (
           val atAirport: Boolean, // Location at Airport? (boolean)
           val iataCode: String,  // IATA Code (if airport)
           val code: Int,  // Unique Location Code (code type is internal to Cartrawler)
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
