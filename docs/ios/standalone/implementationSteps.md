---
layout: default
title: Implementation Steps
parent: Standalone
grand_parent: iOS
nav_order: 1
permalink: /docs/ios/standalone/implementation-steps
---

# Implementation Steps

{: .no_toc }

To implement the SDK's Standalone flow within your app, please use the following steps:

---

## Step 1
### Initialise the SDK in your App Delegate <br/>
<b>Note that the production parameter must be set to true when submitting your app to the AppStore.

```java
import CarTrawlerSDK

// In application(_:didFinishLaunchingWithOptions:)
CarTrawlerSDK.sharedInstance().initialiseSDK(with: nil,
                               customParameters: nil,
                               production: false)
```

For a full list of property descriptions, please click <a href="/docs/ios/standalone/property-descriptions">here</a>

---
## Step 2
### Initialise the CTContext object with the parameters required

This can be done in the view controller the SDK will be presented from.

#### Object description
  
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
  let loyaltyRegex: String,
  let customCashTreatment: Bool,
  let promotionCode: String,
  let recentSearch: CTRecentSearch,
  let supplierBenefitAutoApplied: Bool
}
```
<br/>
#### Required parameters for initialisation:

```java
import CarTrawlerSDK
let context = CTContext(clientID: "your client ID", flow: .standAlone)
context.countryCode = "IE" // The country code associated with the device’s system region is used by default.
context.currencyCode = "EUR" // The currency associated with the device’s system region is used by default.
context.languageCode = "EN" // The language associated with the device’s system region is used by default.
````
<b>Note: the `countryCode` property refers to the country of residency, and this is used when we make search requests.</b>

<br/>
#### Start Standalone flow on the vehicle list
- Optional (bypasses the landing and search screens).

As well as the required parameters mentioned above, you must provide pick-up and drop-off locations and dates. 

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

To pin a specific vehicle to the top of the list, add a vehicle RefID:

```java
context.pinnedVehicleID = "1892038" // Vehicle RefID
```


<br/>
#### Start Standalone flow on the vehicle list <b>via recent search</b> 
- Optional (bypasses the landing and search screens)

```java
// Create a context for the Standalone flow
let context = CTContext(clientID: "your clientID", flow: .standAlone)
context.countryCode = "IE" // The country code associated with the device’s system region is used by default.
context.currencyCode = "EUR" // The currency associated with the device’s system region is used by default.
context.languageCode = "EN" // The language associated with the device’s system region is used by default.
context.recentSearch = recentSearch // your recent search object fetched from the recent searches api. 
context.delegate = self
```
<br/>
#### Start Standalone flow on the search screen 
- Optional (bypasses the landing screen)

```java
// Create a context for standAlone flow
let context = CTContext(clientID: "your clientID", flow: .standAlone)
context.countryCode = "IE" // The country code associated with the device’s system region is used by default.
context.currencyCode = "EUR" // The currency associated with the device’s system region is used by default.
context.languageCode = "EN" // The language associated with the device’s system region is used by default.
context.deeplink = .searchForm
context.delegate = self
```

<br/>
#### Pre populating driver details:
- add a `CTPassenger` object

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

---
## Step 3
### Present the SDK via Modal 
In the view controller you wish to present the SDK from, add the following code after configuring your `CTContext`: 

```java
let viewController = UIViewController()
CarTrawlerSDK.sharedInstance().present(from: viewController, context: context)
```

### Present the SDK via Push Presentation
In the navigation controller you wish to push the SDK from, add the following code after configuring your `CTContext`: 

```java
let navigationController = UINavigationController()
CarTrawlerSDK.sharedInstance().push(fromNavigationViewController: navigationController, context: context)
```
