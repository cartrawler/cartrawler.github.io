---
layout: default
title: API Query Params
parent: iOS SDK APIs
grand_parent: iOS
nav_order: 1
permalink: /docs/ios/apis/api-query-params
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
params.countryCode = "IE"
params.currencyCode = "EUR"
params.languageCode = "EN"
params.iataCode = "DUB"
params.pickupDate = Date(timeIntervalSinceNow: 86400) // next day
params.dropOffDate = Date(timeIntervalSinceNow: 86400 * 3) // next day + 3 days
params.numberOfVehicles = 20
params.sortType = .recommended
```

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
<dd>A language code. Default is "EN" if not provided.</dd>
<dt>pickupDate</dt>
<dd><b>[Required]</b> A pick-up Date.</dd>
<dt>dropOffDate</dt>
<dd><b>[Required]</b> A Drop-off Date.</dd>
<dt>IATACode </dt>
<dd>An IATA code for pick-up location.</dd>
<dt>pickupLocationID</dt>
<dd>An OTA Location ID for pick-up location.</dd>
<dt>dropOffLocationID</dt>
<dd>An OTA Location ID for drop-off location.</dd>
<dt>passengers</dt>
<dd>An Array of Passengers, the first one will be the main passenger.</dd>
<dt>numberOfVehicles</dt>
<dd>A number of vehicles to return, used in the <a href="/docs/ios/apis/vehicles">Vehicles API</a></dd>
<dt>sortType</dt>
<dd>The sort type, used in the <a href="/docs/ios/apis/vehicles">Vehicles API</a></dd>
</dl>