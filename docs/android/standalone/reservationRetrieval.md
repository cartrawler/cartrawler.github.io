---
layout: default
title: Reservation Retrieval
parent: Standalone
grand_parent: Android Integration
nav_order: 3
permalink: /docs/android/standalone/reservation-retrieval/
---

# Reservation Retrieval from Standalone 

{: .no_toc }

When the SDK booking flow finishes it gives the control back to its consumer, so the app
consuming the SDK should override the method `onActivityResult` in order to get the data related
to the reservation details.

---

The process is pretty simple as it follows the Android development standards to get results back
from one activity to another.

Remember when we called the method `startRentalStandalone` in the <a href="/docs/android/standalone/implementation-steps/" target="_blank">implementation steps</a>?
The second argument is called `requestCode` and we'll need it in two different places:

<ol>
  <li>When starting the Standalone flow;</li>
  <li>Getting the reservation details data after finishing the booking flow;</li>
</ol>

`RESERVATION_CALLBACK_REQUEST_CODE` is an Integer <b>user-defined</b> constant used in two places as follows:

Starting the Standalone flow:
```kotlin
CartrawlerSDK
    .Builder()
    // <other methods here>
    .startRentalStandalone(activity, requestCode = RESERVATION_CALLBACK_REQUEST_CODE)
```

Getting the reservation details:
```kotlin
override fun onActivityResult(requestCode: Int, resultCode: Int, data: Intent?) {
    if (resultCode == Activity.RESULT_OK) {
        if (requestCode == RESERVATION_CALLBACK_REQUEST_CODE) {
            val reservationData = data?.getParcelableExtra<ReservationDetails>(CartrawlerSDK.RESERVATION_DETAILS)
            // do what you need with the ReservationDetails object
        }
    }      
}
```
---

### The `ReservationDetails` Object and its dependencies are defined as following:
{: .no_toc}

```kotlin
@Parcelize
data class ReservationDetails (
   // In this scenario it will be CONFIRMED
   val status: String,
   val givenName: String,
   val surname: String,
   val resId: String,
   val resuid: String,
   val pickUpDateTime: GregorianCalendar,
   val returnDateTime: GregorianCalendar,
   val pickupLocation: LocationDetails,
   val returnLocation: LocationDetails, 
   // Insurance, null if none attached
   val insurance: Insurance, 
   // Information on reservation costs
   val rentalInfo: RentalInfo, 
   // Information on selected vehicle
   val vehicle: VehicleDetails,
   // Loyalty membership id and program name
   var custLoyalty: CustLoyalty? = null,
   var supplierBenefitCodesApplied: Set<SupplierBenefitCodeApplied>? = null
) : Parcelable

@Parcelize
data class LocationDetails (
   // Determines if the location is at airport
   val atAirport: Boolean, 
   // IATA Code
   val iataCode: String,  
   // Unique Location Code (code type is internal to CarTrawler)
   val code: Int,  
   // Text description of location
   val name: String, 
   // Postal address of location
   val address: Address, 
   // Vendor contact number
   val phoneNumber: String
) : Parcelable

@Parcelize
data class Insurance (
   // Insurance company name
   val company: String, 
   // Code of offered insurance product
   val id: String, 
   // Base cost
   val cost: Double, 
   // Base currency
   val currency: String, 
   // Date insurance was purchased
   val createDate: String, 
   // Cost converted into charged currency (presented currency)
   val costCharge: Double, 
   // The presented currency to the customer
   val currencyCharge: String,
   // A link to the company logo
   val companyLogo: String, 
   // A link to the policy terms and conditions
   val companyPolicyURL: String, 
   // A marketing description of the insurance (markup)
   val text: String
) : Parcelable

@Parcelize
data class RentalInfo (
   // Base cost
   val cost: Double, 
   // Currency (base currency is EUR)
   val currency: String, 
   // Cost in the currency of the customer
   val customerCost: Double, 
   // The presented currency to the customer
   val customerCurrency: String 
) : Parcelable

@Parcelize
data class Address (
   // Post address of location
   val addressLine: String, 
   // 2 letter country code.
   val countryNameCode: String
) : Parcelable

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
   val baggageQuantity: Int,
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
   val sizeText: String,
   val price: Double,
   val pricePerDay: Double,
   val currencyCode: String
) : Parcelable
   
@Parcelize
data class CustLoyalty(
   val membershipID: String? = null, 
   val programID: String? = null,
   val pointsEarned: String? = null
): Parcelable

@Parcelize
data class SupplierBenefitCodeApplied(
    val name: String,
    val xmlType: String,
    val codeType: String,
    val codeTypeText: String,
    val rentalCompanyName: String,
    val code: String
) : Parcelable
```    