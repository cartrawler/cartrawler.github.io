---
title: Standalone
position: 2
type: iOS
description:
right_code: |-
  ``` swift
  import CarTrawlerSDK

  // In application(_:didFinishLaunchingWithOptions:)
  CarTrawlerSDK.sharedInstance().initialiseSDK(
    with: nil,
    customParameters: nil,
    production: false)
  ```
  {: title="App Delegate" }

  ``` swift
  import CarTrawlerSDK

  let context = CTContext(clientID: "105614", flow: .standAlone)
  context.countryCode = "IE"
  context.currencyCode = "EUR"
  context.languageCode = "en"

  CarTrawlerSDK.sharedInstance().present(from: self, context: context)
  ```
  {: title="Standalone" }

  ``` swift
  import CarTrawlerSDK

  // Create a context for standAlone flow
  let context = CTContext(clientID: "105614", flow: .standAlone)
  context.countryCode = "IE"
  context.currencyCode = "EUR"
  context.languageCode = "EN"
  context.pickupLocationID = "11" // Dublin airport code
  context.dropOffLocationID = "1316" // Cork airport code
  context.pinnedVehicleID = "1892038" // Vehicle RefID
  context.pickupDate = Date(timeIntervalSinceNow: 2629746) // next month
  context.dropOffDate = Date(timeIntervalSinceNow: 2888946) // next month + 3 days
  context.delegate = self
  
  let viewController = UIViewController() // Your view controller from which the SDK will be presented.
  
  CarTrawlerSDK.sharedInstance().present(from: viewController, context: context)
  ```
  {: title="Deeplink" }

  ``` swift
  import CarTrawlerSDK

  // Optional. Called after payment has made, return the reservation details
  func didReceive(_ reservationDetails: CTReservationDetails) {
        
  }
  ```
  {: title="Delegate" }

  ``` swift
  import CarTrawlerSDK

  let passenger = CTPassenger(firstName: "Ryan",
                              lastName: "O'Connor",
                              addressLine1: "DunDrum",
                              addressLine2: "Dublin 14",
                              city: "Dublin",
                              postcode: "Dublin 14",
                              countryCode: "IE",
                              age: 25,
                              email: "ryan.oconnor@cartrawler.com",
                              phone: "0838880000",
                              phoneCountryPrefix: "353",
                              loyaltyProgramNumber: "1234",
                              isPrimaryDriver: true)
  ```
  {: title="CTPassenger" }

  ``` swift
  class CTReservationDetails: NSObject {
    let status: String // In this scernario it will be confirmed
    let givenName: String // First name
    let surname: String // Surname
    let resId: String // Reservation ID
    let resUid: String // Hashed customer email
    let pickUpDateTime: Date // The date & time of pickup
    let returnDateTime: Date  // The date & time of pickup 
    let pickUpLocation: CTLocationDetails // Location details of pickup
    let returnLocation: CTLocationDetails // Location details of pickup
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
  {: title="CTReservationDetails" }

  ``` swift
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
  {: title="CTVehicleDetails" }
  
  ```swift
  
  ```


---


The steps to use the SDK are:

1. Initialise the SDK in App Delegate
2. Initialise the CTContext object with the parameters required
3. Present the SDK

<h5>Initialisation of the SDK</h5>

<dl>
<dt>style</dt><dd>An optional style object, used to set the fonts and primary, secondary and accent colors in the SDK. Please ensure any custom fonts used are included in your main bundle.</dd>
<dt>customParameters</dt><dd>A dictionary of parameters, custom to a particular partner, see below for options.</dd>
<dt>production</dt><dd>A boolean to switch between endpoints, true is production, false is test.</dd>
</dl>

<h5>Custom Parameters</h5>

<dl>
  <dt>orderID</dt>
  <dd>A String value that represents the Order ID for a Flight PNR or Booking Reference, Example: IE1234</dd>
  <dt>flightNumberRequired</dt>
  <dd>A boolean key to enable Flight Number as a required field in the Payment Form. Default: 0 (optional field)</dd>
</dl>

<h5>Initialing CTContext for Standalone</h5>

To initialise standalone flow, it is necessary to instanciate a CTContext object and set the context in the SDK.

<dl>
  <dt>clientID</dt><dd>A <b>required</b> client ID, required to use the CarTrawler API.</dd>
  <dt>flow</dt><dd>A <b>required</b> Must be <b>.standAlone</b>.</dd>
  <dt>countryCode</dt><dd>An optional country code, such as "US". Default is the device location if not provided.</dd>
  <dt>currencyCode</dt><dd>An optional currency code, based on the ISO standard currency codes e.g "USD". The currency associated with the device’s system region is used by default.</dd>
  <dt>languageCode</dt><dd>An optional language code to switch between languages. Default is "EN" if not provided.</dd>
  <dt>passengers</dt><dd>An optional Array of Passengers, the first one will be the main passenger.</dd>
  <dt>delegate</dt><dd>Optional delegate to receive reservation details after the payment</dd>
</dl>

<h5>Initialising CTContext for Standalone with Deeplinking</h5>

This is a variant on the standalone flow whereby the vehicle list is shown based on the pickup and dropoff parameters, rather than the regular initial search screen.
Optionally, if a vehicle refId is provided, this will be become the pinned item in the list.
If a user backs out of the list, it will return the user to the Cartrawler search.

- If the pickup and drop off dates are invalid, out of date, or not present the SDK will fallback to regular standalone search.
- If the vehicle refId is invalid (or out of date), the list will be shown without the vehicle being pinned.
- If the parameters are valid but no search results are returned by the CarTrawler system, the SDK will fallback to the regular standalone search.

<dl>
  <dt>clientID</dt><dd>A <b>required</b> client ID, required to use the CarTrawler API.</dd>
  <dt>flow</dt><dd>A <b>required</b> Must be <b>.standAlone</b>.</dd>
  <dt>countryCode</dt><dd>An optional country code, such as "US". Default is the device location if not provided.</dd>
  <dt>currencyCode</dt><dd>An optional currency code, based on the ISO standard currency codes e.g "USD". The currency associated with the device’s system region is used by default.</dd>
  <dt>languageCode</dt><dd>An optional language code to switch between languages. Default is "EN" if not provided.</dd>
  <dt>pickupDate</dt><dd>A <b>required</b> Pickup Date.</dd>
  <dt>dropOffDate</dt><dd>A <b>required</b> Drop-off Date.</dd>
  <dt>pickupLocation</dt><dd>An IATA code for pickup location. <b>If is set, won't be used pickupLocationID and dropOffLocationID</b></dd>
  <dt>pickupLocationID</dt><dd>An <b>required (if pickupLocation is not set)</b> OTA Location ID for pickup location.</dd>
  <dt>dropOffLocationID</dt><dd>An optional OTA Location ID for drop off location.</dd>
  <dt>pinnedVehicleID</dt><dd>An optional refId to highlight and pin a vehicle to the top of the list. Returned by the abandonment deeplink.</dd>
  <dt>passengers</dt><dd>An optional Array of Passengers, the first one will be the main passenger.</dd>
  <dt>delegate</dt><dd>Optional delegate to receive reservation details after the payment</dd>
</dl>

<h5>Presenting standalone</h5>

After the initialisation of CTContext object filled with your parameters, is needed to use the presentation method:

```swift
let viewController = UIViewController() // Your view controller from which the SDK will be presented.
self.carTrawlerSDK.present(from: viewController, context: context)
```

<h5>Reservation Retrieval from Standalone Process</h5>

If a user booked a car during the standalone process, we will use the delegate to pass the callback informations.
The reservation object is accessed via the return delegate method didReceive(reservationDetails:)

CarTrawlerSDKDelegate method called after a user booked a vehicle:
```swift
  func didReceive(_ reservationDetails: CTReservationDetails) {
      // Reservation object
      print("\(reservationDetails)")
  }
```

CTReservationDetails description objects:
```swift
class CTReservationDetails: NSObject {
  let status: String // In this scernario it will be confirmed
  let customerGivenName: String // first name
  let customerSurname: String // Surname
  let resId: String // Reservation ID
  let resUid: String // Hashed customer email
  let pickUpDateTime: Date //The date & time of pickup
  let returnDateTime: Date  //The date & time of pickup 
  let pickUpLocation: CTLocationDetails //Location details of pickup
  let returnLocation: CTLocationDetails //Location details of pickup
  let insurance: CTInsuranceDetails? // Insurance, null if none attached
  let rentalInfo: RentalInfo?) // Information on reservation costs
  let vehicleDetails: CTVehicleDetails // Information on booked vehicle
  let loyaltyProgramId: String // Loyalty program ID
  let loyaltyNumber: String // Loyalty number
}

class CTLocationDetails: NSObject (
  let atAirport: Bool // Location at Airport? (boolean)
  let iataCode: String  // IATA Code (if airport)
  let code: Int  // Unique Location Code (code type is internal to Cartrawler)
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
  let customerCost: Double // /cost in the currency of the customer
  let customerCurrency: String // the presented currency to the customer
)

class CTAddress: NSObject (
  let addressLine: String // Post adddress of location
  let countryNameCode: String // 2 letter country code.
)
```
