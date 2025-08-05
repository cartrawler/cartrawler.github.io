---
layout: default
title: Implementation Steps
parent: In Path with Grid View
grand_parent: iOS Integration
nav_order: 1
permalink: /docs/ios/inpath-gridview/implementation-steps
---

# Implementation Steps
{: .no_toc }

{: .important}
The SDK must be initialised in your AppDelegate

To implement the SDK’s In Path flow within your app, please use the following steps:

<details open markdown="block">
  <summary>
    Steps
  </summary>
  {: .text-delta }
1. TOC
{:toc}
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

---
## Initialise the CTContext Object with the Required Parameters
This can be done in the view controller the SDK will be presented from.

{: .important }
The `implementationID` is needed by the SDK to fetch partner specific configuration.<br/>

```java
import CarTrawlerSDK

// Create a context for in Path flow
let context = CTContext(implementationID: "your implementation ID", 
                              clientID: "your client ID", 
                              flow: .inPath)
context.countryCode = "IE" // The country code associated with the device’s system region is used by default.
context.currencyCode = "EUR" // The currency associated with the device’s system region is used by default.
context.languageCode = "EN" // The language associated with the device’s system region is used by default.

// Create CTFLightDetails 
let flightDetails = CTFlightDetails()
flightDetails.flightOrigin = "DUB|LHR|2025-01-16T06:05:00|2025-01-16T07:25:00|XX8719"
flightDetails.flightReturn = "LHR|DUB|2025-01-22T21:20:00|2025-01-23T22:45:00|XX8720"
flightDetails.passengerBreakdown = CTFlightPassengerBreakdown(adults: 2, teens: 0, children: 0, infants: 0)
flightDetails.firstName = "John"
flightDetails.surName	 = "Doe"
flightDetails.customerEmail	 = "john.doe@example.com"
flightDetails.fareClass	 = "business"
flightDetails.flightFare = 175.95
flightDetails.bags = 1
flightDetails.loyaltyNumber = "ABC123456"
flightDetails.loyaltyTier = "gold"

// Set flight details on context
context.flightDetails = flightDetails
```

{: .note}
The `countryCode` property refers to the country of residency.

### Object Description:
{: .no_toc }

```swift
class CTContext: NSObject {
    let implementationID: String
    let clientID: String
    let flowType: CTFlowType
    let countryCode: String
    let currencyCode: String
    let pinnedVehicleID: String
    let flightDetails: CTFlightDetails
}

class CTFlightDetails: NSObject {
    let flightOrigin: String
    let flightReturn: String
    let passengerBreakdown: CTFlightPassengerBreakdown
    let firstName: String
    let surName: String
    let customerEmail: String
    let flightFare: Double
    let bags: UInt
    let loyaltyNumber: String
    let loyaltyTier: String
}

class CTFlightPassengerBreakdown: NSObject {
    let adults: UInt
    let teens: UInt
    let children: UInt
    let infants: UInt
}
```
<small>Click <a href="/docs/ios/inpath-gridview/property-descriptions#initialising-ctcontext-for-in-path">here</a> for an in depth explanation of CTContext's properties.</small>

## Prewarm Grid View

After initialisation and setup of your `CTContext` object for In Path, you can pre warm the grid view. 
Prewarm mode allows the Grid View to preload all necessary API data before the user reaches the car rental offer page, resulting in significantly faster load times. 
To use prewarm, you must request the CTGridView object to the SDK and add as a subview to a UIView, the component doesn't to appear on the screen, can be hidden, at this point the grid view won't display any UI, instead, it will fetch and cache API responses. These cached responses are then used when the user navigates to the main offer page, making the experience seamless and quick.

It is recommended to prewarm the grid view in a previous step or ViewController before you reach the ViewController where it will effectively be presented. Example:

```java
class Step1ViewController: UIViewController {
  
    var context: CTContext!

    override func viewDidLoad() {
        super.viewDidLoad()

        // Request prewarmed grid view  
        let gridView = CarTrawlerSDK.sharedInstance().preWarmGridView(with: context)

        // Set hidden
        gridView.isHidden = true

        // Add to subview
        self.view.addSubview(gridView)
    }
}
```

## Present Grid View
The grid view is a component returned by the SDK where a number of recommended vehicles will be displayed as a grid.

When the user taps on a vehicle block, the grid view will work internally to present the SDK in a modal from the UIViewContoller sent as a parameter.

Request grid view to the CarTrawlerSDK:
```java
func getGridView(
                from: UIViewController, // The UIViewController from where the SDK modal will be presented
                context: CTContext // The CTContext with all parameters needed
                )
```


Add the grid view to your own view controller:
```java
class Step2ViewController: UIViewController {
  
    var context: CTContext! // Previously configured context

    override func viewDidLoad() {
        super.viewDidLoad()

        // Request grid view  
        let gridView = CarTrawlerSDK.sharedInstance().getGridView(from: self, context: context)

        // Add to subview
        self.view.addSubview(gridView)
    }
}
```

---

## Collect the booking data after flow completion

After a user has gone through the entire In Path flow and selected a vehicle, the SDK will use a delegate callback to send all of the booking data.

The payload JSON to be used on the reservation process, it will be returned as `request["ota"]`

### CarTrawler SDK Delegate:
{: .no_toc }

```java
// Required. Called when the user wants to add an In Path booking to their flight booking.
func didProduce(inPathPaymentRequest request: [AnyHashable : Any], vehicle: CTInPathVehicle, payment: Payment) {
  print("\(request)")

  print("\(request["ota"])") // Payload JSON to make the booking

  print("Total \(vehicle.totalCost)")
  print("Insurance \(vehicle.insuranceCost)")
  print("Vehicle Name \(vehicle.vehicleName)")

  print("*** PAYNOW: \(vehicle.payNowPrice)\n" ,
  "*** PAYLATER: \(vehicle.payLaterPrice)\n" ,
  "*** PAYDESK: \(vehicle.payAtDeskPrice)\n" ,
  "*** BOOKINGFEE: \(vehicle.bookingFeePrice)\n")

  print("*** Payment ***")
  print("authTotal: \(payment.authTotal)")
  print("authCurrency: \(payment.authCurrency)")

}
```

### In Path Vehicle Object:
{: .no_toc }


```java
class CTInPathVehicle: NSObject {
  let firstName: String // First name
  let lastName: String // Last name
  let vehicleName: String // Vehicle name
  let vendorName: String // Vendor name
  let vehicleImageURL: URL // vehicle image url
  let vendorImageURL: URL // vendorImageURL
  let pickUpLocationName: String // pick-up location name
  let dropOffLocationName: String // drop-off location name
  let pickupDate: Date // pick-up date
  let dropoffDate: Date // drop-off date
  let isBuyingInsurance: Bool // is buying insurance
  let insuranceCost: Number // Insurance cost
  let totalCost: Number // total cost
  let refId: String // vehicle reference id
  let payNowPrice: Number // pay now price
  let payAtDeskPrice: Number // pay at the desk price
  let payLaterPrice: Number // pay later price
  let bookingFeePrice: Number // just the vehicle rental price
```

---