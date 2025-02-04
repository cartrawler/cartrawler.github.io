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
- <a href="/docs/ios/standalone/implementation-steps#start-standalone-flow-on-the-vehicle-list-via-pickup--drop-off-bypass-the-landing-and-search-screens">Start Standalone flow on the Vehicle List via Pickup & Drop Off (bypass the landing and search screens)</a>
- <a href="/docs/ios/standalone/implementation-steps#start-standalone-flow-on-the-vehicle-list-via-recent-search-bypass-the-landing-and-search-screens">Start Standalone Flow on the Vehicle List via Recent Search (bypass the landing and search screens)</a>
2. <a href="/docs/ios/standalone/implementation-steps#start-the-standalone-flow-via-url-deeplink">Start the Standalone Flow via URL Deeplink</a>
3. <a href="/docs/ios/standalone/implementation-steps#prepopulate-driver-details">Prepopulate Driver Details</a>
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
`customParameters`: A dictionary of parameters, custom to a particular partner, see below for options.<br/> <small>- orderID: A String value that represents the Order ID for a Flight PNR or Booking Reference, example: IE1234 (limited to 32 characters)<br/> - flightNumberRequired: A boolean key to enable Flight Number as a required field in the Payment Form. Default: 0 (optional field)</small><br/><br/>
For a full list of property descriptions, please click <a href="/docs/ios/standalone/property-descriptions">here</a>

---
## Initialise the CTContext object with the required parameters 

This can be done in the view controller the SDK will be presented from.

{: .important }
The `implementationID` is needed by the SDK to fetch partner specific configuration.<br/>

<!-- ### Required parameters for Initialisation:
{: .no_toc } -->

```java
import CarTrawlerSDK
let context = CTContext(implementationID: "your implementation ID", 
                        clientID: "your client ID", 
                        flow: .standAlone)
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
  let implementationID: String
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

<br />
### Start Standalone flow on the vehicle list <b>via Pickup & Drop Off</b> (bypass the landing and search screens)
{: .no_toc }

{: .important }
For this alternate starting point in the flow, pick-up and drop-off locations and dates must be provided.

```java
// Create a context for the Standalone flow
let context = CTContext(implementationID: "your implementation ID", 
                        clientID: "your client ID", 
                        flow: .standAlone)

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
A vehicle can be pinned to the top of the list by setting `pinnedVehicleID` (adding a vehicle reference ID). To get a reference ID, you can use our <a href="/docs/api/ios/vehicles">Vehicles API</a>.


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
let context = CTContext(implementationID: "your implementation ID", 
                        clientID: "your client ID", 
                        flow: .standAlone)
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
## Start the Standalone Flow via URL Deeplink
{: .no_toc }

It is possible to launch the Standalone flow using a URL, which may come from a push notification or native app deep link for example.
This is entirely optional. 

{: .note}
The URL should have the following pattern: <br/>
`schema://host?param1=123&param2=abcd&paramN=xyz`

This example will open the vehicle list / search results page: <br />
`schema://ct-car-rental?type=search-result&client_id=123456&pt=2023-09-08T09:44:35+0100&dt=2023-09-10T10:44:35+0100&pkIATA=DUB`

{: .warning}
A client ID <small>(client_id)</small> <b>must</b> be provided as part of your URL in order for the SDK to function properly. 

Similar to the methods listed previously for launching the Standalone flow, the URL is used by CTContext. A separate initialiser exists for this purpose: 

```java
let url = "airline://ct-car-rental?type=search-result&client_id=123456&pt=2023-09-08T09:44:35+0100&dt=2023-09-10T10:44:35+0100&pkIATA=DUB"
let context = CTContext(implementationID: "your implementation ID", appDeeplinkURL: url)
```

You can then present the SDK:

```java
CarTrawlerSDK.sharedInstance().present(from: self, context: context)
```
<small> See <a href="#present-the-sdk-via-modal-or-push-presentation">here</a> for more details</small>

<br/>

### Start Standalone flow on the Landing Page

To open the SDK on the landing page, simply set the type to landing: `type=landing`

### Start Standalone flow on the Vehicle List (bypass Landing Screen and Search Screens)

To open the SDK on the vehicle list page, set the type to search-result: `type=search-result` 

{: .note}
Make sure to provide pickup and drop off dates, and a pickup location ID, or IATA code. If any of these are missing, the SDK will instead open on the landing page. 

### Start Standalone flow on the Manage Booking screen
{: .no_toc }

To open the SDK on the manage booking screen, simply set the type to booking-details: `type=booking-details` and make sure to provide the booking id and email. Email is only required to import the booking if it was made through a different platform (e.g Android, Web)

### List of Parameters

Below are all the available parameters for use in the URL. 



##### Landing
{: .no_toc }

| Parameter           | Example | Required | 
|:--------------------|:--------|:---------|
| type                | landing | yes      |
| client_id           | 123456  | yes      |
| ctyCode (residency) | IE      | no       |
| ccy (currency)      | EUR     | no       |

##### Manage Booking
{: .no_toc }

| Parameter           | Example          | Required                                                              | 
|:--------------------|:-----------------|:----------------------------------------------------------------------|
| type                | booking -details | yes                                                                   |
| client_id           | 123456           | yes                                                                   |
| ctyCode (residency) | IE               | no                                                                    |
| ccy (currency)      | EUR              | no                                                                    |
| booking             | IE123456789      | yes                                                                   |
| email               | mail@mail.com    | no (only required if importing the booking from a different platform) |

##### Search Results
{: .no_toc }

| Parameter                 | Example              | Required                | 
|:--------------------------|:---------------------|:------------------------|
| type                      | search-result        | yes                     |
| client_id                 | 123456               | yes                     |
| pt (pickup time)          | 2023-08-18T10:00:00Z | yes                     |
| dt (drop off time)        | 2023-08-20T10:00:00Z | yes                     |
| pkIATA (pickup IATA)      | DUB                  | yes (if pl not set)     |
| doIATA (drop off IATA)    | DUB                  | no                      |
| pl (pickup location ID)   | 11                   | yes (if pkIATA not set) |
| dl (drop off location ID) | 11                   | no                      |
| age                       | 30                   | no                      |
| ctyCode (residency)       | IE                   | no                      |
| ccy (currency)            | EUR                  | no                      |
| pinVeh (pinned vehicle)   | 123456789            | no                      |

--- 

## Adding Flight Information
{: .no_toc }
For enhanced reporting partners can optionally add flight data when initialising the SDK. Each of these parameters is optional and user functionality will not be impacted whether they are added or not.

A new CTFlightDetails object is added with the following parameters.

| Parameter                | Description                                                                                                 | Example                                                                  | Type       | 
|:-------------------------|:------------------------------------------------------------------------------------------------------------|:-------------------------------------------------------------------------|------------|
| age                      | Customer age                                                                                                | 29                                                                       | Integer    |
| bags                     | Total number of bags. Both Cabin and Hold should be included                                                | 3                                                                        | Integer    |
| basketAmount             | Total basket amount including bags, seats, etc.                                                             | 130.99                                                                   | Float      |
| campaignID               | Unique identifier associated with a specific marketing campaign.                                            | Google-EN-Destination-France                                             | String     |
| context                  | Where in the flow the SDK is loaded                                                                         | Confirmation Screen                                                      | String     |
| fareClass                | Type of fare                                                                                                | Economy                                                                  | String     |
| flightFare               | Price of flight(s) only                                                                                     | 101.99                                                                   | Float      |
| loyaltyNumber            | Customer loyalty number                                                                                     | 123222121                                                                | String     |
| loyaltyTier              | Customer loyalty tier                                                                                       | Platinum                                                                 | String     |
| marketingPreference      | Flag to indicate if the current user has opted in for third-party marketing i.e car rental                  | true                                                                     | Boolean    |
| marketingSegment         | The customers segment as determined by the airline marketing team.                                          | Weekend Breaker                                                          | String     |
| membershipID             | Only required if different from loyaltyNumber. This value represents the logged in users unique identifier. | 789                                                                      | String        |
| passengerBreakdown       | Breakdown of Adults, Teens, Children and Infants                                                            | CTFlightPassengerBreakdown(adults: 2, teens: 0, children: 0, infants: 0) | CT Object  |
| pnr                      | Flight PNR                                                                                                  | CT123456                                                                 | String     |
| sessionID                | Current user session identifier                                                                             | 0idfw78jsnkoo                                                            | String     |
| sportsEquipment          | Total number of sports equipment items a customer is travelling with                                        | 1                                                                        | Integer    |
| sportsEquipmentBreakdown | A breakdown of the individual equipment a customer is travelling with                                       | ["golf": (1), "ski": (1), "surf": (1)]                                   | Dictionary |
| tripDuration             | Total length of trip in days                                                                                | 4                                                                        | Integer    |
| tripType                 | An identification of whether the trip type is business, leisure or other                                    | Business                                                                 | String     |

```java
//Example CTFLightDetails initialisation

let flightDetails = CTFlightDetails()
flightDetails.age = 32
flightDetails.bags = 2
flightDetails.basketAmount = 130.99
flightDetails.campaignID = "Google-EN-Destination-France"
flightDetails.context = "confirmation"
flightDetails.fareClass = "regular"
flightDetails.flightFare = 101.99
flightDetails.loyaltyNumber = "WZ123456789"
flightDetails.loyaltyTier = "platinum"
flightDetails.marketingPreference = true
flightDetails.marketingSegment = "Budget Conscious"
flightDetails.membershipID = "123222121"
flightDetails.passengerBreakdown = CTFlightPassengerBreakdown(adults: 2, teens: 0, children: 0, infants: 0)
flightDetails.pnr = "TEYI89"
flightDetails.sessionID = "0idfw78jsnkoo"
flightDetails.sportsEquipment = 2
flightDetails.sportsEquipmentBreakdown = ["golf": (1), "ski": (1), "surf": (1)]
flightDetails.tripDuration = 4
flightDetails.tripType = "business"
        
//Use CTFLightDetails as part of the context
let context = CTContext(implementationID: "your implementation ID",
clientID: "your client ID",
flow: .standAlone)
    
context.flightDetails = flightDetails

```        
---
## Adding UTM tracking data
{: .no_toc }
For enhanced reporting partners can optionally add tracking data in the form of UTM parameters. The meaning and usage of these parameters are <a href="https://en.wikipedia.org/wiki/UTM_parameters">as standard</a>.

A new CTUTMParameters object is added with the following parameters.

```java
//Example CTUTMParameters initialisation
let utmParameters = CTUTMParameters()
utmParameters.source = "partner_utm_source"
utmParameters.medium = "partner_utm_medium"
utmParameters.campaign = "partner_utm_campaign"
utmParameters.term = "partner_utm_term"
utmParameters.content = "partner_utm_content"
        
context.utmParameters = utmParameters
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
                            loyaltyProgramNumber: "1234")
context.passengers = [passenger]
```

{: .note }
`CTPassenger` `countryCode` takes priority over `CTContext`'s `countryCode` property when we make search requests.
