---
title: Inpath
position: 3
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

  // Create a context for inPath flow
  let context = CTContext(clientID: "105614", flow: .inPath)
  context.countryCode = "IE"
  context.currencyCode = "EUR"
  context.languageCode = "EN"
  context.pickupLocation = "DUB"
  context.pickupDate = Date(timeIntervalSinceNow: 86400)
  context.flightNumber = "FL1234"
  context.delegate = self

  // Will trigger a first automatically bestDailyRate request
  CarTrawlerSDK.sharedInstance().setContext(context)
  ```
  {: title="Initialisation" }

  ```swift
  let viewController = UIViewController() // Your view controller from which the SDK will be presented.
  self.carTrawlerSDK.presentInPath(from: viewController)
  ```
  {: title="Presentation" }

  ``` swift
  import CarTrawlerSDK

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
  {: title="Delegate" }

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
  ```
  {: title="CTInPathVehicle" }

  ``` swift

   ```

---


The steps to use the SDK are:

1. Initialise the SDK in App Delegate
2. Initialise the CTContext object with the parameters required
3. Set the InPath context on the SDK, to trigger the initial requests
3. Present the SDK

<h5>Initialisation of the SDK</h5>

<dl>
<dt>style</dt><dd>An optional style object, used to set the fonts and primary, secondary and accent colors in the SDK. Please ensure any custom fonts used are included in your main bundle.</dd>
<dt>customParameters</dt><dd>A dictionary of parameters, custom to a particular partner, see below for options.</dd>
<dt>production</dt><dd>A boolean to switch between endpoints, true is production, false is test.</dd>
</dl>

<h5>Initialising CTContext for Inpath</h5>

Allows a customer to reserve a car rental product to accompany their flight, this product forms part of the customer’s  flight itenary. 
See code to right for available methods and callbacks for in path.

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
</dl>

<h5>Set the InPath context on the SDK</h5>
```swift
  // Create a context for inPath flow
  let context = CTContext(clientID: "105614", flow: .inPath)
  context.countryCode = "IE"
  context.currencyCode = "EUR"
  context.languageCode = "EN"
  context.pickupLocation = "DUB"
  context.pickupDate = Date(timeIntervalSinceNow: 86400)
  context.flightNumber = "FL1234"
  context.delegate = self

  // Will trigger a first automatically bestDailyRate request
  CarTrawlerSDK.sharedInstance().setContext(context)
```

<h5>Presenting Inpath</h5>

After the initialisation of inPath flow using and setting a CTContext object filled with your parameters, is needed to use the presentation method:

```swift
let viewController = UIViewController() // Your view controller from which the SDK will be presented.
self.carTrawlerSDK.presentInPath(from: viewController)
```
