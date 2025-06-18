---
layout: default
title: API Query Params
parent: iOS SDK APIs
grand_parent: APIs
nav_order: 1
permalink: /docs/api/ios/api-query-params
---

# API Query Params

{: .no_toc }

A `CTAPIQueryParams` object must be created and initialised after initialising the SDK to make requests using our API methods.

---

## Example CTAPIQueryParams

```java
import CarTrawlerSDK

let params = CTAPIQueryParams()  
params.delegate = self
params.clientID = "your clientID"
params.countryCode = "IE" // The country code associated with the device’s system region is used by default.
params.currencyCode = "EUR" // The currency associated with the device’s system region is used by default.
params.languageCode = "EN" // The language associated with the device’s system region is used by default.
params.iataCode = "DUB" // use IATACode for Objective-C
params.iataCodeDropoff = "DUB" // use IATACodeDropoff for Objective-C
params.pickupDate = Date(timeIntervalSinceNow: 86400) // next day
params.dropOffDate = Date(timeIntervalSinceNow: 86400 * 3) // next day + 3 days
params.numberOfVehicles = 20
params.sortType = .recommended
params.pickupLocationCoordinate = CLLocationCoordinate2DMake(53.3333, -6.2408989)
params.dropOffLocationCoordinate = CLLocationCoordinate2DMake(51.903614, -8.468399)
```

### Pick Up & Drop Off Location parameters
Pick up location can be passed as a IATA, coordinates or OTA Location ID. 
Priority is IATA > OTA location id > coordinates

- 'pickupLocation' and 'dropOffLocation':
  It takes a three-letter code that represents airports worldwide. E.g 'DUB' for Dublin.

- 'pickupLocationCoordinate's and 'dropOffLocationCoordinate':
  ```swift 
  context.pickupLocationCoordinate = CLLocationCoordinate2DMake(53.3333, -6.2408989)
  context.dropOffLocationCoordinate = CLLocationCoordinate2DMake(51.903614, -8.468399)
  ```
- 'pickupLocationId' and 'dropOffLocationId':
  It takes a string OTA Location ID for pickup location, e.g “11” for Dublin.

### Property Descriptions

<dl>
<dt>delegate</dt>
<dd>A CarTrawlerSDKDelegate to receive the API response.</dd>
<dt>clientID</dt>
<dd>Your client ID, required to use the CarTrawler API.</dd>
<dt>countryCode</dt>
<dd>A country code, such as "US". Default is the device location if not provided.</dd>
<dt>currencyCode</dt>
<dd>An optional currency code, based on the ISO standard currency codes e.g "USD". The currency associated with the device’s system region is used by default.</dd>
<dt>languageCode</dt>
<dd>A language code. The language associated with the device’s system region is used by default.</dd>
<dt>pickupDate</dt>
<dd><b>[Required]</b> A pick-up Date.</dd>
<dt>dropOffDate</dt>
<dd><b>[Required]</b> A Drop-off Date.</dd>
<dt>IATACode </dt>
<dd>An IATA code for pick-up location.</dd>
<dt>IATACodeDropoff</dt>
<dd>An IATA code for drop-off location.</dd>
<dt>pickupLocationID</dt>
<dd>An OTA Location ID for pick-up location.</dd>
<dt>dropOffLocationID</dt>
<dd>An OTA Location ID for drop-off location.</dd>
<dt><span style="font-size:0.8em">pickupLocationCoordinate</span></dt><dd>A CLLocationCoordinate2D for pick-up coordinates.</dd>
<dt><span style="font-size:0.8em">dropOffLocationCoordinate</span></dt><dd>A CLLocationCoordinate2D for drop-off coordinates. Fallback to pickupLocationCoordinate if is not set.</dd>
<dt>passenger</dt>
<dd>The main driver details.</dd>
<dt>numberOfVehicles</dt>
<dd>A number of vehicles to return, used in the <a href="/docs/ios/apis/vehicles">Vehicles API</a></dd>
<dt>sortType</dt>
<dd>The sort type, used in the <a href="/docs/ios/apis/vehicles">Vehicles API</a></dd>
</dl>