---
layout: default
title: Reservation Retrieval
parent: Standalone
grand_parent: iOS
nav_order: 2
permalink: /docs/ios/standalone/reservation-retrieval
---

# Reservation Retrieval from Standalone 

{: .no_toc }

If a user booked a car during the standalone process, we will use the delegate to pass the callback information.
The reservation object is accessed via the return delegate method didReceive(reservationDetails:)

---

CarTrawlerSDKDelegate method called after a user booked a vehicle:

```java
  func didReceive(_ reservationDetails: CTReservationDetails) {
      // Reservation object
      print("\(reservationDetails)")
  }
```

CTReservationDetails description objects:

```java
class CTReservationDetails: NSObject {
  let status: String // In this scenario it will be confirmed
  let customerGivenName: String // first name
  let customerSurname: String // Surname
  let resId: String // Reservation ID
  let resUid: String // Hashed customer email
  let pickUpDateTime: Date // The date & time of pick-up
  let returnDateTime: Date  // The date & time of pick-up
  let pickUpLocation: CTLocationDetails // Location details of pick-up
  let returnLocation: CTLocationDetails // Location details of pick-up
  let insurance: CTInsuranceDetails? // Insurance, null if none attached
  let rentalInfo: RentalInfo?) // Information on reservation costs
  let vehicleDetails: CTVehicleDetails // Information on booked vehicle
  let loyaltyProgramId: String // Loyalty program ID
  let loyaltyNumber: String // Loyalty number
}

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
    let airConditionInd: Bool // vehicle is air conditioning included
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
    let categoryText: String // localised category
    let price: NSNumber // vehicle price
    let pricePerDay: NSNumber // vehicle price per day
    let currencyCode: String // vehicle price currency code
  }

class CTLocationDetails: NSObject (
  let atAirport: Bool // Location at Airport? (boolean)
  let iataCode: String  // IATA Code (if airport)
  let code: Int  // Unique Location Code (code type is internal to CarTrawler)
  let name: String // Text description of location
  let address: CTAddress // Postal address of location
  let phoneNumber: String // Vendor contact number
)

class CTInsuranceDetails: NSObject (
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
)

class CTRentalInfo: NSObject (
  let cost: Double // base cost
  let currency: String // base currency
  let customerCost: Double // cost in the currency of the customer
  let customerCurrency: String // the presented currency to the customer
)

class CTAddress: NSObject (
  let addressLine: String // Post address of location
  let countryNameCode: String // 2 letter country code.
)

```