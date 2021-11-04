---
title: Reservations API
position: 3
type: 
description:
right_code: >-
---
Reservations API is responsible for returning a wrapper response object of the reservation of the user's car rental booking.

### Reservations API - iOS

Calling the requestReservationDetails function will trigger a vehicles request based on a resID and email (hashed) or resUid.

  ```swift
    import CarTrawlerSDK
  
    // This will trigger a booking reservation fetch
    // The SDK must be initialised, and a CTAPIQueryParams object with the necessary parameters must be set before calling this method

    let params = CTAPIQueryParams()  
    params.delegate = self
    params.clientID = "12345"
    params.languageCode = "EN"
    params.resId = "IE3453453"
    params.resUid = "12424345446466464"

    CarTrawlerSDK.sharedInstance.requestReservationDetails(params { (reservation, error) in
            if let reservation = reservation {
                // reservation fetched successfully
            } else if let error = error {
                // error occured
            }
        }
  ```

These properties can be found in the ReservationDetails object that is returned upon successful completion of the standalone flow

```swift
didReceive(_ reservationDetails: CTReservationDetails)
```

The reservations object takes the following form:
```swift
class CTReservationDetails: NSObject {
    let status: String // In this scernario it will be confirmed
    let givenName: String // First name
    let surname: String // Surname
    let resId: String // Reservation ID
    let resUid: String // Hashed customer email
    let pickUpDateTime: Date //The date & time of pickup
    let returnDateTime: Date  //The date & time of pickup 
    let pickUpLocation: CTLocationDetails //Location details of pickup
    let returnLocation: CTLocationDetails //Location details of pickup
    let insurance: CTInsuranceDetails? // Insurance, null if none attached
    let rentalInfo: RentalInfo? // Information on reservation costs
    let vehicleDetails: CTVehicleDetails // Information on booked vehicle
    let loyaltyProgramId: String // Loyalty program ID
    let loyaltyNumber: String // Loyalty number
}

class CTLocationDetails: NSObject {
    let atAirport: Bool // Location at Airport? (boolean)
    let iataCode: String  // IATA Code (if airport)
    let code: Int  // Unique Location Code (code type is internal to Cartrawler)
    let name: String // Text description of location
    let address: CTAddress // Postal address of location
    let phoneNumber: String // Vendor contact number
}

class CTInsuranceDetails: NSObject {
    let upSell: Bool
    let company: String // Insurance company name
    let insuranceID: String // Code of offered insurance product
    let cost: Double // base cost
    let currency: String // base currency
    let costCharge: Double // Cost converted into charged currency (presented currency)
    let currencyCharge: String // the presented currency to the customer
    let companyLogo: URL // a link to the company logo
    let companyPolicyURL: URL // a link to the policy terms and conditions
    let text: String // A marketing description of the insurance (markup)
}

class CTRentalInfo: NSObject {
    let cost: Double // base cost
    let currency: String // base currency
    let customerCost: Double // /cost in the currency of the customer
    let customerCurrency: String // the presented currency to the customer
}

class CTAddress: NSObject {
    let addressLine: String // Post adddress of location
    let countryNameCode: String // 2 letter country code.
}



```

### Reservations API - Android

Calling the requestReservationDetails function will trigger a vehicles request based on a resID and email (hashed) or resuid. 

  ~~~kotlin
                 CartrawlerSDK.Builder()
                 //..
                 builder.requestReservationDetails(
                         context = this,
                         resId = resid,
                         resuid = resuid,
                         email = null,
                         requestReservationDetailsListener = object: CartrawlerSDK.RequestReservationDetailsListener {
     
                             override fun onNoResults() {
                                //Handle no results
                             }
     
                             override fun onError(connectionError: CartrawlerSDK.ConnectionError) {
                               //Handle error result
                             }
     
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
