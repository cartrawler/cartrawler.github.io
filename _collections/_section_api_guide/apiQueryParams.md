---
title: API Query Params
position: 1
type: iOS
description:
right_code: >-
---


<h5>iOS API Query Params</h5>

An object to use for request on the API methods.

After initializing the SDK, please create a CTAPIQueryParams object and initialize it with the following parameters:

  ``` swift
  
    import CarTrawlerSDK

    let params = CTAPIQueryParams()  
    params.delegate = self
    params.clientID = "12345"
    params.countryCode = "IE"
    params.currencyCode = "EUR"
    params.languageCode = "EN"
    params.iataCode = "DUB"
    params.pickupDate = Date(timeIntervalSinceNow: 86400) // next day
    params.dropOffDate = Date(timeIntervalSinceNow: 86400 * 3) // next day + 3 days
    params.numberOfVehicles = 20
    params.sortType = .recommended
  
  ```

Description of each parameter as follows:
<br/>
<dl>
<dt>delegate</dt>
<dd>a CarTrawlerSDKDelegate to receive the API response.</dd>
<dt>clientID</dt>
<dd>Your client ID, required to use the CarTrawler API.</dd>
<dt>countryCode</dt>
<dd>A country code, such as "US". Default is the device location if not provided.</dd>
<dt>currencyCode</dt>
<dd>An optional currency code, based on the ISO standard currency codes e.g "USD". The currency associated with the device’s system region is used by default.</dd>
<dt>languageCode</dt>
<dd>A language code. Default is "EN" if not provided.</dd>
<dt>pickupDate</dt>
<dd>A required Pickup Date.</dd>
<dt>dropOffDate</dt>
<dd>A required Drop-off Date.</dd>
<dt>IATACode </dt>
<dd>An optional IATA code for pickup location.</dd>
<dt>pickupLocationID</dt>
<dd>An optional OTA Location ID for pickup location.</dd>
<dt>dropOffLocationID</dt>
<dd>An optional OTA Location ID for drop off location.</dd>
<dt>passengers</dt>
<dd>An optional Array of Passengers, the first one will be the main passenger.</dd>
<dt>numberOfVehicles</dt>
<dd>An optional number of vehicles to return, used in <a href="https://cartrawler.github.io/#section_iosgetVehicles">Vehicles API</a></dd>
<dt>sortType</dt>
<dd>An optional sort type, used in <a href="https://cartrawler.github.io/#section_iosgetVehicles">Vehicles API</a></dd>
</dl>
