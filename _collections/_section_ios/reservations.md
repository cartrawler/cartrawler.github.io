---
title: Reservations API
position: 8
type: iOS
description:
right_code: >-
---
<h5>Get Booking Reservation</h5>

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
