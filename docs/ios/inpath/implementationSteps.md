---
layout: default
title: Implementation Steps
parent: In Path
grand_parent: iOS
nav_order: 1
permalink: /docs/ios/inpath/implementation-steps
---

# Implementation Steps
{: .no_toc }

<details open markdown="block">
  <summary>
    Steps
  </summary>
  {: .text-delta }
1. TOC
{:toc}
</details>

<details markdown="block">
  <summary>
    Optional Extras
  </summary>
  {: .text-delta }
1. <a href="/docs/ios/inpath/implementation-steps#prepopulate-driver-details">Prepopulate Driver Details</a>
</details>
---

## Initialise the SDK in your App Delegate <br/>
<b>Note that the `production` parameter must be set to true when submitting your app to the AppStore.</b>

```java
// In application(_:didFinishLaunchingWithOptions:)
CarTrawlerSDK.sharedInstance().initialiseSDK(with: nil,
                                customParameters: nil,
                                production: false)
```

{: .important }
The production parameter must be set to true when submitting your app to the AppStore.

{: .note }
`style`: An optional <a href="/docs/ios/customisation/themes#creating-a-ctstyle">CTStyle</a> object, used to set the fonts as well as the primary, secondary, and accent colors in the SDK. Please ensure any custom fonts used are included in your main bundle. <br/><br/>
`customParameters`: A dictionary of parameters, custom to a particular partner, see below for options.<br/> <small>- orderID: A String value that represents the Order ID for a Flight PNR or Booking Reference, example: IE1234 <br/> - flightNumberRequired: A boolean key to enable Flight Number as a required field in the Payment Form. Default: 0 (optional field)</small><br/><br/>
For a full list of property descriptions, please click <a href="/docs/ios/inpath/property-descriptions#sdk-initialisation-parameters">here</a>

---
## Initialise the CTContext Object with the Required Parameters
This can be done in the view controller the SDK will be presented from.

```java
import CarTrawlerSDK

// Create a context for in Path flow
let context = CTContext(clientID: "12345", flow: .inPath)
context.countryCode = "IE" // The country code associated with the device’s system region is used by default.
context.currencyCode = "EUR" // The currency associated with the device’s system region is used by default.
context.languageCode = "EN" // The language associated with the device’s system region is used by default.
context.pickupLocation = "DUB"
context.pickupDate = Date(timeIntervalSinceNow: 86400)
context.flightNumber = "FL1234"
context.delegate = self
```

{: .note}
The above properties are required, and the `countryCode` property refers to the country of residency. This is used to make search requests.

### Object Description:
{: .no_toc }

```swift
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
    let loyaltyRegex: String
    let customCashTreatment: Bool
    let promotionCode: String
    let recentSearch: CTRecentSearch
    let supplierBenefitAutoApplied: Bool
    let clientUserIdentifier: String
}
```
<small>Click <a href="/docs/ios//inpath/property-descriptions#initialising-ctcontext-for-in-path">here</a> for an in depth explanation of CTContext's properties.</small>

---
## Set the In Path context on the SDK, to trigger the Initial Request
```java
// This will automatically trigger a bestDailyRate request
CarTrawlerSDK.sharedInstance().setContext(context)
```

---

## Present the SDK

After initialisation and setup of your `CTContext` object for In Path, you must use the following presentation method:

```java
let viewController = UIViewController() // Your view controller from which the SDK will be presented.
self.carTrawlerSDK.presentInPath(from: viewController)
```

---

## Collect the booking data after flow completion

After a user has gone through the entire In Path flow and selected a vehicle, the SDK will use a delegate callback to send all of the booking data:

### CarTrawler SDK Delegate:
{: .no_toc }

```java
// Required. Called when the user wants to add an In Path booking to their flight booking.
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

// Called when the best daily rate is received, the setContext: method will trigger this request automatically
func didReceiveBestDailyRate(_ price: NSNumber, currency: String) {
}

// Called when the best daily rate request fails
func didFailToReceiveBestDailyRate(error: Error) {
}
```

### In Path Vehicle Object:
{: .no_toc }


```java
class CTInPathVehicle: NSObject {
  let firstName: String // First name
  let lastName: String // Last name
  let vehicleName: String // Vehicle name
  let vehicleOrSimilar: String // localized "or similar" text
  let vendorName: String // Vendor name
  let vehicleImageURL: URL // vehicle image url
  let vendorImageURL: URL // vendorImageURL
  let pickUpLocationName: String // pick-up location name
  let dropOffLocationName: String // drop-off location name
  let pickupDate: Date // pick-up date
  let dropoffDate: Date // drop-off date
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

---

## Prepopulate Driver Details:
{: .no_toc }

The Driver Details screen can by prepopulated by creating a `CTPassenger` object and setting the `passengers` property on your `CTContext`. 

```java
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
<b>Note: `CTPassenger` `countryCode` takes priority over `CTContext`'s `countryCode` property when we make search requests.</b>