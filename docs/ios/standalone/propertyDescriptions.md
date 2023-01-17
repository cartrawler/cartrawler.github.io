---
layout: default
title: Property Descriptions
parent: Standalone
grand_parent: iOS Integration
nav_order: 2
permalink: /docs/ios/standalone/property-descriptions
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

## Initialising CTContext for Standalone 
<br />
To initialise the standalone flow, it is necessary to instantiate a CTContext object and set the context in the SDK.

{: .note } 
The `countryCode` property refers to the country of residency, and this is used when we make search requests.

<dl>
  <dt>clientID</dt><dd><b>[Required]</b> Your client ID, required to use the CarTrawler API.</dd>
  <dt>flow</dt><dd><b>[Required]</b> The flow to be launched. Must be <b>.standAlone</b>.</dd>
  <dt>countryCode</dt><dd> A country code, such as "US". Default is the device location if not provided.</dd>
  <dt>currencyCode</dt><dd> A currency code, based on the ISO standard currency codes e.g "USD". The currency associated with the device’s system region is used by default.</dd>
  <dt>languageCode</dt><dd> A code for setting the language. The language associated with the device’s system region is used by default.</dd>
  <dt>flightNumber</dt><dd> A flight number string to pre populate the equivalent field on driver details screen.</dd>
  <dt>passengers</dt><dd> An array of passengers, the first one will be the main passenger.</dd>
  <dt>delegate</dt><dd> A delegate to receive reservation details after the payment, as well as other callbacks.</dd>
  <dt>loyaltyRegex</dt><dd> A regular expression used to validate the loyalty number field.</dd>
  <dt>customCashTreatment</dt><dd> A boolean used in the SDK as the main toggle to display enhanced cash voucher merchandising throughout the booking flow.</dd>
  <dt>promotionCode</dt><dd> A string used by the SDK to toggle the display of and prepopulate the promotion code field on the search form.</dd>
  <dt><span style="font-size:0.7em">supplierBenefitAutoApplied</span></dt><dd>A boolean that allows Partners to initialise the SDK and opt in to apply ALL automatic codes that can be applied for suppliers.</dd>
  <dt>clientUserIdentifier</dt><dd>A string token of a logged in user that allows the SDK to fetch the user loyalty details and points.</dd>
  <dt>settingsIconType</dt><dd>An enum used to set the icon used for the landing and search page settings buttons.</dd>
</dl>

{: .note } 
For the `promotionCode` property: To display the field and prepopulate it, please provide a string. To display the field without a prepopulated code please provide an empty string.

---
### Initialising CTContext for the Standalone flow with Pickup & Drop Off
{: .no_toc }

<br />
This is a variant on the standalone flow whereby the vehicle list is shown based on the pick-up and drop-off properties, rather than the regular initial search screen.
Optionally, if a vehicle refId is provided, the vehicle it belongs to will be pinned to the top of the vehicle list.
If the user navigates back from the vehicle list in this flow variant, they will be brought to the search screen.

{: .note } 
<small>If the pick-up and drop-off dates are invalid, out-of-date, or not present the SDK will fallback to regular standalone search.
<br/>If the vehicle refId is invalid or out-of-date, the list will be shown without the vehicle being pinned.
<br/>If the pick-up and drop-off properties are valid but no search results are returned by the CarTrawler system, the SDK will fallback to the regular standalone search.
<br/>The countryCode property refers to the country of residency, and this is used when we make search requests.</small>

<dl>
  <dt>clientID</dt><dd><b>[Required]</b> Your client ID, required to use the CarTrawler API.</dd>
  <dt>flow</dt><dd><b>[Required]</b> The flow to be launched. Must be <b>.standAlone</b>.</dd>
  <dt>countryCode</dt><dd> A country code, such as "US". Default is the device location if not provided.</dd>
  <dt>currencyCode</dt><dd> A currency code, based on the ISO standard currency codes e.g "USD".</dd>
  <dd>The currency associated with the device’s system region is used by default.</dd>
  <dt>languageCode</dt><dd> A language code to switch between languages. The language associated with the device’s system region is used by default.</dd>
  <dt>flightNumber</dt><dd> A flight number string to pre populate the equivalent field on driver details screen.</dd>
  <dt>pickupDate</dt><dd><b>[Required]</b> The pick-up Date.</dd>
  <dt>dropOffDate</dt><dd><b>[Required]</b> The Drop-off Date.</dd>
  <dt>pickupLocation</dt><dd> An IATA code for pick-up location.</dd>
  <dd><b>Note: If this is set, the pickupLocationID and dropOffLocationID properties will not be used.</b></dd>
  <dt>pickupLocationID</dt><dd><b>[Required (if pickupLocation is not set)]</b> An OTA Location ID for pick-up location.</dd>
  <dt>dropOffLocationID</dt><dd> An OTA Location ID for drop-off location.</dd>
  <dt>pinnedVehicleID</dt><dd> A refId to highlight and pin a vehicle to the top of the list. Returned by the abandonment deep link.</dd>
  <dt>passengers</dt><dd> An Array of Passengers, the first one will be the main passenger.</dd>
  <dt>delegate</dt><dd> A delegate to receive reservation details after the payment, as well as other callbacks.</dd>
  <dt>loyaltyRegex</dt><dd> A regular expression to validate loyalty number field. Example: ^[A-Za-z0-9]{6, }$</dd>
  <dt>customCashTreatment</dt><dd> A boolean used in the SDK as the main toggle to display enhanced cash voucher merchandising throughout the booking flow.</dd>
  <dt>promotionCode</dt><dd> A string used by the SDK to toggle the display of and prepopulate the promotion code field on the search form.</dd>
  <dt><span style="font-size:0.7em">supplierBenefitAutoApplied</span></dt><dd>A boolean that allows Partners to initialise the SDK and opt in to apply ALL automatic codes that can be applied for suppliers.</dd>
  <dd><b>Note: To display the field and prepopulate it, please provide a string. To display the field without a pre-populated code please provide an empty string.</b></dd>
  <dt>clientUserIdentifier</dt><dd>A string token of a logged in user that allows the SDK to fetch the user loyalty details and points.</dd>
  <dt>settingsIconType</dt><dd>An enum used to set the icon used for the landing and search page settings buttons.</dd>
</dl>

---
### Initialising CTContext for Standalone with a Recent Search
{: .no_toc }

<br />
This is a variant on the standalone flow whereby the vehicle list is shown based on the recent search parameter, rather than the regular initial search screen.
If a user backs out of the list, it will exit the standalone flow.

{: .note } 
If the recent search is unavailable or invalid, the SDK will fallback to the regular standalone search.<br/>
The `countryCode` property refers to the country of residency, and this is used when we make search requests.

<dl>
  <dt>clientID</dt><dd><b>[Required]</b> Your client ID, required to use the CarTrawler API.</dd>
  <dt>flow</dt><dd><b>[Required]</b> The flow to be launched. Must be <b>.standAlone</b>.</dd>
  <dt>countryCode</dt><dd> A country code, such as "US". Default is the device location if not provided.</dd>
  <dt>currencyCode</dt><dd> A currency code, based on the ISO standard currency codes e.g "USD". The currency associated with the device’s system region is used by default.</dd>
  <dt>languageCode</dt><dd> A language code to switch between languages. The language associated with the device’s system region is used by default.</dd>
  <dt>passengers</dt><dd> An array of Passengers, the first one will be the main passenger.</dd>
  <dt>delegate</dt><dd> A delegate to receive reservation details after the payment.</dd>
  <dt>loyaltyRegex</dt><dd> A regular expression to validate loyalty number field. Example: ^[A-Za-z0-9]{6, }$</dd>
  <dt>customCashTreatment</dt><dd> A boolean used in the SDK as the main toggle to display enhanced cash voucher merchandising throughout the booking flow.</dd>
  <dt>promotionCode</dt><dd> A string used by the SDK to toggle the display of and prepopulate the promotion code field on the search form.</dd>
  <dt>recentSearch</dt><dd> A CTRecentSearch object used to open the SDK on the vehicle list page and bypass the search screen.</dd>
</dl>