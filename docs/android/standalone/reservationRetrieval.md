---
layout: default
title: Reservation Retrieval
parent: Standalone
grand_parent: Android
nav_order: 2
permalink: /docs/android/standalone/reservation-retrieval/
---

# Reservation Retrieval from Standalone 

{: .no_toc }

If a user booked a car during the standalone process, we will use the delegate to pass the callback information.
The reservation object is accessed via the return intent by onActivityForResult.

---

```java
returnIntent.getStringExtra(CartrawlerSDK.RESERVATION)
    
override fun onActivityForResult(requestCode: Int, resultCode: Int, data: Intent?) {
   if (resultCode == Activity.RESULT_OK) {
       if (requestCode == 123) {
            yourMethod(data!!.getStringExtra(CartrawlerSDK.RESERVATION))
        }      
}

// The Reservation Object is defined as the following

data class ReservationDetails (
   // In this scernario it will be confirmed
   val status: String,
   // first name
   val givenName: String, 
   // Surname
   val surname: String, 
   // Reservation ID
   val resId: String, 
   // resuid, use this along with the resId to retrieve the booking later
   val resuid: String, 
   //The date & time of pick-up
   val pickUpDateTime: GregorianCalendar, 
   //The date & time of pick-up 
   val returnDateTime: GregorianCalendar,  
   //Location details of pick-up
   val pickupLocation: LocationDetails, 
   //Location details of pick-up
   val returnLocation: LocationDetails, 
   // Insurance, null if none attached
   val insurance: Insurance, 
   // Information on reservation costs
   val rentalInfo: RentalInfo, 
   //Information on selected vehicle
   val vehicle: VehicleDetails,
   //Loyalty membership id and program name
   var custLoyalty: CustLoyalty? = null) 
  
data class LocationDetails (
   // Determines if the location is at airport
   val atAirport: Boolean, 
   // IATA Code
   val iataCode: String,  
   // Unique Location Code (code type is internal to Cartrawler)
   val code: Int,  
   // Text description of location
   val name: String, 
   // Postal address of location
   val address: Address, 
   // Vendor contact number
   val phoneNumber: String)

data class Insurance (
   // Insurance company name
   val company: String, 
   // Code of offered insurance product
   val id: String, 
   // base cost
   val cost: Double, 
   // base currency
   val currency: String, 
   // date insurance was purchased
   val createDate: String, 
   // Cost converted into charged currency (presented currency)
   val costCharge: Double, 
   // the presented currency to the customer
   val currencyCharge: String,
   // a link to the company logo
   val companyLogo: String, 
   // a link to the policy terms and conditions
   val companyPolicyURL: String, 
   // A marketing description of the insurance (markup)
   val text: String)

data class RentalInfo (
   // base cost
   val cost: Double, 
   // currency (base currency is EUR)
   val currency: String, 
   // cost in the currency of the customer
   val customerCost: Double, 
   // the presented currency to the customer
   val customerCurrency: String )

data class Address (
   // Post adddress of location
   val addressLine: String, 
   // 2 letter country code.
   val countryNameCode: String

@Parcelize
data class VehicleDetails(
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
   val supplier: String,
   val supplierRating: Double,
   val supplierImageURL: String,
   val passengersText: String,
   val baggageText: String?,
   val doorsCountText: String,
   val transmissionText: String,
   val categoryText: String,
   val sizeText: String
   val price: Double,
   val pricePerDay: Double,
   val currencyCode: String) : Parcelabl
   
@Parcelize
data class CustLoyalty(
   val membershipID: String? = null, 
   val programID: String? = null,
   val pointsEarned: String? = null): Parcelable
}
```    