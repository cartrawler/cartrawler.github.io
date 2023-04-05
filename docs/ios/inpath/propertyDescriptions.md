---
layout: default
title: Property Descriptions
parent: In Path
grand_parent: iOS Integration
nav_order: 2
permalink: /docs/ios/inpath/property-descriptions
---

# Property Descriptions
{: .no_toc }

<details open markdown="block">
  <summary>
    Table of Contents
  </summary>
  {: .text-delta }
1. TOC
{:toc}
</details>

---

## SDK Initialisation Parameters

<dl>
<dt><b>style</b></dt><dd>An optional <a href="/docs/ios/customisation/themes#creating-a-ctstyle">CTStyle</a> object, used to set the fonts as well as the primary, secondary, and accent colors in the SDK. </dd>
<dt><b>customParameters</b></dt><dd>A dictionary of custom parameters, it may contain:</dd>
<dd><b>orderID</b>: A String value that represents the Order ID for a Flight PNR or Booking Reference, Example: IE1234</dd>
<dd><b>flightNumberRequired</b>: A boolean key to enable Flight Number as a required field in the Payment Form. Default: 0 (optional field)</dd>
<dt><b>production</b></dt><dd>A boolean for switching between endpoints. Set to true for production and false for dev.</dd>
</dl>

{: .important}
Please ensure any custom fonts used are included in your main bundle.

--- 
## Initialising CTContext for In Path
<br />
To initialise the In Path flow, it is necessary to instantiate a CTContext object and set the context in the SDK.

{: .note } 
The `countryCode` property refers to the country of residency, and this is used when we make search requests.

<dl>
  <dt>implementationID</dt><dd><b>[Required]</b> An implementation ID, provided by CarTrawler and required to fetch the partner configuration.</dd>
  <dt>clientID</dt><dd><b>[Required]</b> A client ID, required to use the CarTrawler API.</dd>
  <dt>flow</dt><dd><b>[Required]</b> The flow to be launched. Must be <b>.inPath</b>.</dd>
  <dt>countryCode</dt><dd> A country code, such as "US". Default is the device location if not provided.</dd>
  <dt>currencyCode</dt><dd> A currency code, based on the ISO standard currency codes e.g "USD". The currency associated with the device’s system region is used by default.</dd>
  <dt>languageCode</dt><dd> A language code to switch between languages. Default is "EN" if not provided.</dd>
  <dt>pickupDate</dt><dd><b>[Required]</b> Pick-up Date.</dd>
  <dt>dropOffDate</dt><dd><b>[Required]</b> Drop-off Date.</dd>
  <dt>pickupLocation</dt><dd><b>[Required]</b> An IATA code for pick-up location.</dd>
  <dt>dropOffLocation</dt><dd> An IATA code for drop-off location.</dd>
  <dt>pinnedVehicleID</dt><dd> A refId to highlight and pin a vehicle to the top of the list. Returned by the abandonment deep link.</dd>
  <dt>passengers</dt><dd> An Array of Passengers, the first one will be the main passenger.</dd>
  <dt>delegate</dt><dd> A delegate that will handle callback methods.</dd>
  <dt>customCashTreatment</dt><dd> A boolean used in the SDK as the main toggle to display enhanced cash voucher merchandising throughout the booking flow.</dd>
  <dt>promotionCode</dt><dd> A string used by the SDK to toggle the display of and prepopulate the promotion code field on the search form.</dd>
  <dt>clientUserIdentifier</dt><dd>A string token of a logged in user that allows the SDK to fetch the user loyalty details and points.</dd>
</dl>

{: .note } 
For the `promotionCode` property: To display the field and prepopulate it, please provide a string. To display the field without a prepopulated code please provide an empty string.