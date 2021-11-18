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


### Initialise the SDK in your App Delegate <br/>
<b>Note that the production parameter must be set to true when submitting your app to the AppStore.

```java
// In application(_:didFinishLaunchingWithOptions:)
CarTrawlerSDK.sharedInstance().initialiseSDK(with: nil,
                               customParameters: nil,
                               production: false)
```

---
### Initialise the CTContext object with the parameters required

#### Object description
  
````swift
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
  let recentSearch: CTRecentSearch
}
````
<br/>
#### Required parameters for initialisation:

```java
import CarTrawlerSDK
let context = CTContext(clientID: "your client ID", flow: .standAlone)
context.countryCode = "IE"
context.currencyCode = "EUR"
context.languageCode = "en"
````

<br/>
#### Pre populating driver details:
- add a CTPassenger object

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

<br/>
#### Navigation to the vehicle list
- Optional (bypasses the landing and search screens)

```java
// Create a context for the Standalone flow
let context = CTContext(clientID: "your clientID", flow: .standAlone)
context.countryCode = "IE"
context.currencyCode = "EUR"
context.languageCode = "EN"
context.pickupLocationID = "11" // Dublin airport code
context.dropOffLocationID = "1316" // Cork airport code
context.pinnedVehicleID = "1892038" // Vehicle RefID
context.pickupDate = Date(timeIntervalSinceNow: 2629746) // next month
context.dropOffDate = Date(timeIntervalSinceNow: 2888946) // next month + 3 dayssz243
context.delegate = self
```
<br/>
#### Navigation to the vehicle list <b>via recent search</b> 
- Optional (bypasses the landing and search screens)

```java
// Create a context for the Standalone flow
let context = CTContext(clientID: "your clientID", flow: .standAlone)
context.countryCode = "IE"
context.currencyCode = "EUR"
context.languageCode = "EN"
context.recentSearch = recentSearch // your recent search object fetched from the recent searches api. 
context.delegate = self
```
<br/>
#### Navigation to the search screen 
- Optional (bypasses the landing screen)

```java
// Create a context for standAlone flow
let context = CTContext(clientID: "your clientID", flow: .standAlone)
context.countryCode = "IE"
context.currencyCode = "EUR"
context.languageCode = "EN"
context.deeplink = .searchForm
context.delegate = self
```

---
### Present the SDK

```java
import CarTrawlerSDK

let viewController = UIViewController() // Your view controller from which the SDK will be presented.

CarTrawlerSDK.sharedInstance().present(from: viewController, context: context)
```