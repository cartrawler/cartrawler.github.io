---
layout: default
title: Vehicles
parent: iOS SDK APIs
grand_parent: APIs
nav_order: 2
permalink: /docs/api/ios/vehicles
---

# Vehicles
{: .no_toc }

The Vehicles API is responsible for returning a wrapper response object of the list of vehicles returned from our backend.

<details open markdown="block">
  <summary>
    Table of Contents
  </summary>
  {: .text-delta }
1. TOC
{:toc}
</details>

---

## Request Vehicles

We expose a method to retrieve the vehicle list based on a sort type and limit you specify.

Calling the `requestVehicles` function will trigger a vehicles request based on the provided IATA or PickupLocationCode, as well as the pick-up and drop-off dates. 

```java
import CarTrawlerSDK
// This will trigger a new vehicles fetch, and the subsequent delegate callbacks
// The SDK must be initialised, and a CTAPIQueryParams object with the necessary parameters must be set before callingthis method
  
let params = CTAPIQueryParams()  
params.delegate = self
params.clientID = "your clientID"
params.countryCode = "IE" // The country code associated with the device’s system region is used by default.
params.currencyCode = "EUR" // The currency associated with the device’s system region is used by default.
params.languageCode = "EN" // The language associated with the device’s system region is used by default.
params.iataCode = "DUB"
params.pickupDate = Date(timeIntervalSinceNow: 86400) // next day
params.dropOffDate = Date(timeIntervalSinceNow: 86400 * 3) // next day + 3 days
params.numberOfVehicles = 20 // Default 0, it will return all vehicles
params.sortType = .recommended // .recommended (Default) or .bestPrice

CarTrawlerSDK.sharedInstance().requestVehicles(params)
```

{: .note }
The number of vehicles returned can be limited by setting the `numberOfVehicles` property. <br /><br />
The sort type can be either: <br /> `.bestPrice`, which returns the cheapest cars in the list <br /> or `.recommended`, which returns the CarTrawler recommended cars.

---

## Delegate Functions

A list of vehicle objects (`CTVehicleDetails`) will be returned in the `CarTrawlerSDKDelegate` function `didReceiveVehicles`. If an error occurs, this will be returned via the `didFailToReceiveVehicles` function. 

```java
// Called when best daily rate received
func didReceiveVehicles(_ vehicles: [CTVehicleDetails]) {
}
// Called when best daily rate fails
func didFailToReceiveVehicles(error: Error) {
}
```

---

### Vehicle Details class
{: .no_toc }

```java
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
  let ctCashDetails: CTCashDetails // CTCashDetails object
}
```

### CTCash Details class
{: .no_toc }

```java
class CTCashDetails: NSObject {
  let offerTitle: String // title of the special offer, displayed on the carblock chip on the vehicle list 
  let originalPrice: NSNumber // The original price of the vehicle
  let discountedPrice: NSNumber // The discounted price of the vehicle
  let discountAmount: NSNumber // The discount amount 
  let discountPercentage: String // The percentage discount
}
```