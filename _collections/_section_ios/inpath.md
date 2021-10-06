---
title: Inpath
position: 3
type: iOS
description:
right_code: >-

---


The steps to use the SDK are:

<b>Step 1. Initialise the SDK in App Delegate:</b>

<b>Note that production parameter must be true when your application is deployed to production.</b>

```swift
  // In application(_:didFinishLaunchingWithOptions:)
  CarTrawlerSDK.sharedInstance().initialiseSDK(with: nil,
                                 customParameters: nil,
                                 production: false)
```

<b>Step 2. Initialise the CTContext object with the parameters required</b>

Object description:
  ``` swift
    class CTContext: NSObject {
        let clientID: String
        let flowType: CTFlowType
        let countryCode: String
        let currencyCode: String
        let pickupDate: Date
        let dropOffDate: Date
        let pickupLocation: String
        let dropOffLocation: String
        let flightNumber: String
        let pickupLocationID: String
        let dropOffLocationID: String
        let pinnedVehicleID: String
        let passengers: [CTPassenger]
        let loyaltyRegex: String,
        let customCashTreatment: Bool,
        let promotionCode: String
   }
  ```

  ``` swift
  import CarTrawlerSDK

  // Create a context for inPath flow
  let context = CTContext(clientID: "12345", flow: .inPath)
  context.countryCode = "IE"
  context.currencyCode = "EUR"
  context.languageCode = "EN"
  context.pickupLocation = "DUB"
  context.pickupDate = Date(timeIntervalSinceNow: 86400)
  context.flightNumber = "FL1234"
  context.delegate = self
  ```

  Optional addition of passengers:
```swift
  //Passenger object
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

  context.passengers = [passenger]
  ```

<b>Step 3 - Set the InPath context on the SDK, to trigger the initial requests</b>
```swift
  // It will trigger a first automatically bestDailyRate request
  CarTrawlerSDK.sharedInstance().setContext(context)
```

<b>Step 4 - Present the SDK</b>

After the initialisation of inPath flow and setting a CTContext object filled with your parameters, is needed to use the presentation method:

```swift
let viewController = UIViewController() // Your view controller from which the SDK will be presented.
self.carTrawlerSDK.presentInPath(from: viewController)
```

<b>Step 5 - Collect the data after user make a booking</b>

After the the user go through the entire flow and select a vehicle, SDK will use a delegate callback to send all data booking data:

CarTrawlerSDKDelegate:
``` swift
// Required. Called when user wants to add in path booking to their flight booking.
func didProduce(inPathPaymentRequest request: [AnyHashable : Any], vehicle: CTInPathVehicle, payment: Payment) {
  print("\(request)")

  print("Total \(vehicle.totalCost)")
  print("Insurance \(vehicle.insuranceCost)")

  print("Vehicle Name \(vehicle.vehicleName)")
  print("Vehicle First Name \(vehicle.firstName)")
  print("Vehicle LastName \(vehicle.lastName)")

  print("*** PAYNOW: \(vehicle.payNowPrice)\n" ,
  "*** PAYLATER: \(vehicle.payLaterPrice)\n" ,
  "*** PAYDESK: \(vehicle.payAtDeskPrice)\n" ,
  "*** BOOKINGFEE: \(vehicle.bookingFeePrice)\n")

  print("*** Payment ***")
  print("authTotal: \(payment.authTotal)")
  print("authCurrency: \(payment.authCurrency)")

}

// Called when best daily rate received, setContext: method will trigger this request automatically
func didReceiveBestDailyRate(_ price: NSNumber, currency: String) {
}

// Called when best daily rate fails
func didFailToReceiveBestDailyRate(error: Error) {
}
```

Inpath Vehicle Object

  ``` swift
  class CTInPathVehicle: NSObject {
    let firstName: String // First name
    let lastName: String // Last name
    let vehicleName: String // Vehicle name
    let vehicleOrSimilar: String // localized "or similar" text
    let vendorName: String // Vendor name
    let vehicleImageURL: URL // vehicle image url
    let vendorImageURL: URL // vendorImageURL
    let pickUpLocationName: String // Pickup location name
    let dropOffLocationName: String // Drop off location name
    let pickupDate: Date // Pick up date
    let dropoffDate: Date // Drop off date
    let extrasIncludedForFree: Array // Array of included extras
    let extrasPayableAtDesk: Array // Array of extras payable at desk
    let extrasPayableNow: Array // Array of extras payable now
    let isBuyingInsurance: Bool // is buying insurance
    let insuranceCost: Number // Insurance cost
    let vehicleCharges: Array // Array of Vehicle Charges
    let loyalty: CTLoyalty? // User loyalty info
  }

  class CTExtraEquipment: NSObject {
    let qty: Integer // Quantity of the extra
    let isIncludedInRate: Bool  // If the extra is included
    let isTaxInclusive: Bool  // If extra is tax inclusive
    let isGuaranteedInd: Bool // If extra is pre paid
    let chargeAmount: Number // Cost of extra
    let currencyCode: String // Currency code of extra
    let equipType: String // Raw extra type code
    let equipmentType: CTExtraEquipmentType // Extra type
    let name: String // Name of extra
    let equipDescription: String // Description of extra
  }

  class CTVehicleCharge: NSObject {
    let chargeDescription: String // The localized description
    let isIncludedInRate: Bool  // If the charge is included (this case always true)
    let isTaxInclusive: Bool  // If charge is tax inclusive
    let amount: Number // Cost of charge
    let currencyCode: String // Currency code of charge
    let purpose: String // Purpose code of the charge
    let calculation: CTVehicleChargeCalculation // .beforePickup
  }

  class CTLoyalty: NSObject {
    let programName: String // Loyalty program name
    let points: Number // Loyalty points earned
  }
  ```

<br/>
<br/>
<br/>
<br/>

<h2>Initialisation of the SDK</h2>

<dl>
<dt>style</dt><dd>An optional style object, used to set the fonts and primary, secondary and accent colors in the SDK. Please ensure any custom fonts used are included in your main bundle.</dd>
<dt>customParameters</dt><dd>A dictionary of parameters, custom to a particular partner, see below for options.</dd>
<dt>production</dt><dd>A boolean to switch between endpoints, true is production, false is test.</dd>
</dl>

<h5>Initialising CTContext for Inpath</h5>

Allows a customer to reserve a car rental product to accompany their flight, this product forms part of the customer’s  flight itenary. 
See code snippet above for available methods and callbacks for in path.

<dl>
  <dt>clientID</dt><dd>A <b>required</b> client ID, required to use the CarTrawler API.</dd>
  <dt>flow</dt><dd>A <b>required</b> Must be <b>.inPath</b>.</dd>
  <dt>countryCode</dt><dd>An optional country code, such as "US". Default is the device location if not provided.</dd>
  <dt>currencyCode</dt><dd>An optional currency code, based on the ISO standard currency codes e.g "USD". The currency associated with the device’s system region is used by default.</dd>
  <dt>languageCode</dt><dd>An optional language code to switch between languages. Default is "EN" if not provided.</dd>
  <dt>pickupDate</dt><dd>A <b>required</b> Pickup Date.</dd>
  <dt>dropOffDate</dt><dd>A <b>required</b> Drop-off Date.</dd>
  <dt>pickupLocation</dt><dd>An <b>required</b> IATA code for pickup location.</dd>
  <dt>dropOffLocation</dt><dd>An optional IATA code for dropoff location.</dd>
  <dt>pinnedVehicleID</dt><dd>An optional refId to highlight and pin a vehicle to the top of the list. Returned by the abandonment deeplink.</dd>
  <dt>passengers</dt><dd>An optional Array of Passengers, the first one will be the main passenger.</dd>
  <dt>delegate</dt><dd>Your class that will handle callback methods</dd>
  <dt>customCashTreatment</dt><dd>An optional boolean used in the SDK as the main toggle to display enhanced cash voucher merchandising throughout the booking flow.</dd>
  <dt>promotionCode</dt><dd>An optional string used in the SDK as the main toggle to display promotion code field on the search form or not. Use empty string to show the field or a promotion code string to show the field and pre populate it.</dd>
</dl>
