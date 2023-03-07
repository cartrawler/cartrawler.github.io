---
layout: default
title: Implementation Steps
parent: Standalone
grand_parent: iOS Integration
nav_order: 1
permalink: /docs/ios/standalone/implementation-steps
---

# Implementation Steps
{: .no_toc }

{: .important}
The SDK must be initialised in your AppDelegate

To implement the SDK's Standalone flow within your app, please use the following steps:

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
1. <a href="/docs/ios/standalone/implementation-steps#start-the-standalone-flow-on-another-screen">Start the Standalone Flow on Another Screen</a>
- <a href="/docs/ios/standalone/implementation-steps#start-standalone-flow-on-the-search-screen-bypass-landing-screen">Start Standalone flow on the Search Screen (bypass Landing Screen)</a>
- <a href="/docs/ios/standalone/implementation-steps#start-standalone-flow-on-the-vehicle-list-via-pickup--drop-off-bypass-the-landing-and-search-screens">Start Standalone flow on the Vehicle List via Pickup & Drop Off (bypass the landing and search screens)</a>
- <a href="/docs/ios/standalone/implementation-steps#start-standalone-flow-on-the-vehicle-list-via-recent-search-bypass-the-landing-and-search-screens">Start Standalone Flow on the Vehicle List via Recent Search (bypass the landing and search screens)</a>
2. <a href="/docs/ios/standalone/implementation-steps#prepopulate-driver-details">Prepopulate Driver Details</a>
</details>
---

## Initialise the SDK in your App Delegate <br/>

```java
import CarTrawlerSDK

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
For a full list of property descriptions, please click <a href="/docs/ios/standalone/property-descriptions">here</a>

---
## Initialise the CTContext object with the required parameters 

This can be done in the view controller the SDK will be presented from.

<!-- ### Required parameters for Initialisation:
{: .no_toc } -->

```java
import CarTrawlerSDK
let context = CTContext(clientID: "your client ID", flow: .standAlone)
context.countryCode = "IE" // The country code associated with the device’s system region is used by default.
context.currencyCode = "EUR" // The currency associated with the device’s system region is used by default.
context.languageCode = "EN" // The language associated with the device’s system region is used by default.
````

{: .note }
The above properties are required, and the `countryCode` property refers to the country of residency. This is used to make search requests.

### Object Description
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
  let settingsIconType: CTSettingsIconType 
}
```
<small>Click <a href="/docs/ios/standalone/property-descriptions#initialising-ctcontext-for-standalone">here</a> for an in depth explanation of CTContext's properties.</small>

---
## Present the SDK via Modal or Push Presentation

In the view controller you wish to present the SDK from, add the following code after configuring your `CTContext`: 

```java
let viewController = UIViewController()
CarTrawlerSDK.sharedInstance().present(from: viewController, context: context)
```

Alternatively, in the navigation controller you wish to push the SDK from, add the following code after configuring your `CTContext`: 

```java
let navigationController = UINavigationController()
CarTrawlerSDK.sharedInstance().push(fromNavigationViewController: navigationController, context: context)
```

---

## Start the Standalone Flow on Another Screen
{: .no_toc }

When launching the Standalone flow, it is possible to bypass the landing and search screens by setting certain properties of your CTContext object.    
This is entirely optional. 
<br/>

### Start Standalone flow on the Search Screen (bypass Landing Screen)
{: .no_toc }

```java
// Create a context for standAlone flow
let context = CTContext(clientID: "your clientID", flow: .standAlone)
context.countryCode = "IE" // The country code associated with the device’s system region is used by default.
context.currencyCode = "EUR" // The currency associated with the device’s system region is used by default.
context.languageCode = "EN" // The language associated with the device’s system region is used by default.
context.deeplink = .searchForm
context.delegate = self
```

<br />
### Start Standalone flow on the vehicle list <b>via Pickup & Drop Off</b> (bypass the landing and search screens)
{: .no_toc }

{: .important }
For this alternate starting point in the flow, pick-up and drop-off locations and dates must be provided.

```java
// Create a context for the Standalone flow
let context = CTContext(clientID: "your clientID", flow: .standAlone)

// Set required properties
context.countryCode = "IE" // The country code associated with the device’s system region is used by default.
context.currencyCode = "EUR" // The currency associated with the device’s system region is used by default.
context.languageCode = "EN" // The language associated with the device’s system region is used by default.

// Set pick-up and drop-off locations using location or IATA codes
// Pick-up at Dublin Airport
context.pickupLocationID = "11" // or context.pickupLocation = "DUB"
// Drop-off at Cork Airport
context.dropOffLocationID = "1316" // or context.dropOffLocation = "ORK"

// Sample dates
var dateComponent = DateComponents()
dateComponent.month = 1
let nextMonth = Calendar.current.date(byAdding: dateComponent, to: Date())
dateComponent.day = 3
let nextMonthPlusThreeDays = Calendar.current.date(byAdding: dateComponent, to: Date())

// Set pick-up and drop-off dates
context.pickupDate = nextMonth! // next month sample date
context.dropOffDate = nextMonthPlusThreeDays! // next month + 3 days sample date

// Set your viewController as the CarTrawlerSDK delegate 
context.delegate = self
```

{: .note }
A vehicle can be pinned to the top of the list by setting `pinnedVehicleID` (adding a vehicle reference ID). To get a reference ID, you can use our <a href="/docs/ios/apis/vehicles">Vehicles API</a>.


```java
context.pinnedVehicleID = "1892038" // Vehicle RefID
```
<small>Click <a href="/docs/ios/standalone/property-descriptions#initialising-ctcontext-for-the-standalone-flow-with-deep-linking">here</a> for an in depth explanation of the relevant CTContext properties</small>

<br/>
### Start Standalone Flow on the Vehicle List <b>via Recent Search</b> (bypass the landing and search screens)
{: .no_toc }

{: .important }
For this alternate starting point in the flow, a recent search (`CTRecentSearch` object) must be provided.

```java
// Create a context for the Standalone flow
let context = CTContext(clientID: "your clientID", flow: .standAlone)
context.countryCode = "IE" // The country code associated with the device’s system region is used by default.
context.currencyCode = "EUR" // The currency associated with the device’s system region is used by default.
context.languageCode = "EN" // The language associated with the device’s system region is used by default.
context.recentSearch = recentSearch // your recent search object fetched from the recent searches api. 
context.delegate = self
```

{: .note}
To get a recent search, you can use our <a href="/docs/api/ios/recent-searches">Recent Searches API</a>.

<small> Click <a href="/docs/ios/standalone/property-descriptions#initialising-ctcontext-for-standalone-with-a-recent-search">here</a> for an in depth explanation of the relevant CTContext properties </small>
<br/>


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

{: .note }
`CTPassenger` `countryCode` takes priority over `CTContext`'s `countryCode` property when we make search requests.
