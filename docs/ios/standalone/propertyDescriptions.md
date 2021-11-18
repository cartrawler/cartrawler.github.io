---
layout: default
title: Property Descriptions
parent: Standalone
grand_parent: iOS
nav_order: 3
permalink: /docs/ios/standalone/property-descriptions
---

# Property Descriptions

{: .no_toc }

---

<dl>
<dt>style</dt><dd>An optional style object, used to set the fonts and primary, secondary and accent colors in the SDK. Please ensure any custom fonts used are included in your main bundle.</dd>
<dt>customParameters</dt><dd>A dictionary of parameters, custom to a particular partner, see below for options.</dd>
<dt>production</dt><dd>A boolean to switch between endpoints, true is production, false is test.</dd>
</dl>

---
### Custom Parameters

<dl>
  <dt>orderID</dt>
  <dd>A String value that represents the Order ID for a Flight PNR or Booking Reference, Example: IE1234</dd>
  <dt>flightNumberRequired</dt>
  <dd>A boolean key to enable Flight Number as a required field in the Payment Form. Default: 0 (optional field)</dd>
</dl>
---

#### Initialising CTContext for Standalone 
<br />
To initialise the standalone flow, it is necessary to instantiate a CTContext object and set the context in the SDK.

<dl>
  <dt>clientID</dt><dd><b>[Required]</b>Your client ID, required to use the CarTrawler API.</dd>
  <dt>flow</dt><dd><b>[Required]</b> The flow to be launched. Must be <b>.standAlone</b>.</dd>
  <dt>countryCode</dt><dd>[Optional] A country code, such as "US". Default is the device location if not provided.</dd>
  <dt>currencyCode</dt><dd>[Optional] A currency code, based on the ISO standard currency codes e.g "USD". The currency associated with the device’s system region is used by default.</dd>
  <dt>languageCode</dt><dd>[Optional] A code for setting the language. This defaults to "EN" if not provided.</dd>
  <dt>passengers</dt><dd>[Optional] An array of passengers, the first one will be the main passenger.</dd>
  <dt>delegate</dt><dd>[Optional] A delegate to receive reservation details after the payment, as well as other callbacks</dd>
  <dt>loyaltyRegex</dt><dd>[Optional] A regular expression used to validate the loyalty number field</dd>
  <dt>customCashTreatment</dt><dd>[Optional] A boolean used in the SDK as the main toggle to display enhanced cash voucher merchandising throughout the booking flow.</dd>
  <dt>promotionCode</dt><dd>[Optional] A string used in the SDK as the main toggle for displaying the promotion code field on the search form or not. Use an empty string to show the field or a string with a promotion code to show the field and pre populate it.</dd>
</dl>

---
#### Initialising CTContext for the Standalone with Deeplinking
<br />
This is a variant on the standalone flow whereby the vehicle list is shown based on the pickup and drop off properties, rather than the regular initial search screen.
Optionally, if a vehicle refId is provided, the vehicle it belongs to will be pinned to the top of the vehicle list.
If the user navigates back from this screen in this flow variant, list, it will return the user to the CarTrawler search.

* If the pickup and drop off dates are invalid, out of date, or not present the SDK will fallback to regular standalone search.
* If the vehicle refId is invalid (or out of date), the list will be shown without the vehicle being pinned.
* If the pickup and drop off properties are valid but no search results are returned by the CarTrawler system, the SDK will fallback to the regular standalone search.

<dl>
  <dt>clientID</dt><dd><b>[Required]</b>client ID, required to use the CarTrawler API.</dd>
  <dt>flow</dt><dd><b>[Required]</b>The flow to be launched. Must be <b>.standAlone</b>.</dd>
  <dt>countryCode</dt><dd>[Optional] A country code, such as "US". Default is the device location if not provided.</dd>
  <dt>currencyCode</dt><dd>[Optional] A currency code, based on the ISO standard currency codes e.g "USD".</dd>
  <dd>The currency associated with the device’s system region is used by default.</dd>
  <dt>languageCode</dt><dd>[Optional] A language code to switch between languages. Default is "EN" if not provided.</dd>
  <dt>pickupDate</dt><dd><b>[Required]</b> The Pickup Date.</dd>
  <dt>dropOffDate</dt><dd><b>[Required]</b> The Drop-off Date.</dd>
  <dt>pickupLocation</dt><dd>[Optional] An IATA code for pickup location.</dd>
  <dd><b>Note: If this is set, the pickupLocationID and dropOffLocationID properties will not be used.</b></dd>
  <dt>pickupLocationID</dt><dd>[Required (if pickupLocation is not set)]</b> An OTA Location ID for pickup location.</dd>
  <dt>dropOffLocationID</dt><dd>[Optional] An OTA Location ID for drop off location.</dd>
  <dt>pinnedVehicleID</dt><dd>[Optional] A refId to highlight and pin a vehicle to the top of the list. Returned by the abandonment deeplink.</dd>
  <dt>passengers</dt><dd>[Optional] An Array of Passengers, the first one will be the main passenger.</dd>
  <dt>delegate</dt><dd>[Optional] A delegate to receive reservation details after the payment, as well as other callbacks</dd>
  <dt>loyaltyRegex</dt><dd>[Optional] A regular expression to validate loyalty number field. Example: ^[A-Za-z0-9]{6, }$</dd>
  <dt>customCashTreatment</dt><dd>[Optional] A boolean used in the SDK as the main toggle to display enhanced cash voucher merchandising throughout the booking flow.</dd>
  <dt>promotionCode</dt><dd>[Optional] A string used by the SDK to toggle the display of and prepopulate the promotion code field on the search form.</dd>
  <dd><b>Note: To display the field and prepopulate it, please provide a string. To display the field without a prepopulated code please provide an empty string.</b></dd>
</dl>


---
#### Initialising CTContext for Standalone with a Recent Search
<br />
This is a variant on the standalone flow whereby the vehicle list is shown based on the recent search parameter, rather than the regular initial search screen.
If a user backs out of the list, it will exit the standalone flow.

* Note: If the recent search is unavailable or invalid, the SDK will fallback to the regular standalone search.

<dl>
  <dt>clientID</dt><dd><b>[Required]</b> client ID, required to use the CarTrawler API.</dd>
  <dt>flow</dt><dd><b>[Required]</b> The flow to be launched. Must be <b>.standAlone</b>.</dd>
  <dt>countryCode</dt><dd>[Optional] A country code, such as "US". Default is the device location if not provided.</dd>
  <dt>currencyCode</dt><dd>[Optional] A currency code, based on the ISO standard currency codes e.g "USD". The currency associated with the device’s system region is used by default.</dd>
  <dt>languageCode</dt><dd>[Optional] A language code to switch between languages. Default is "EN" if not provided.</dd>
  <dt>passengers</dt><dd>[Optional] An array of Passengers, the first one will be the main passenger.</dd>
  <dt>delegate</dt><dd>[Optional] A delegate to receive reservation details after the payment</dd>
  <dt>loyaltyRegex</dt><dd>[Optional] A regular expression to validate loyalty number field. Example: ^[A-Za-z0-9]{6, }$</dd>
  <dt>customCashTreatment</dt><dd>[Optional] A boolean used in the SDK as the main toggle to display enhanced cash voucher merchandising throughout the booking flow.</dd>
  <dt>promotionCode</dt><dd>[Optional] A string used in the SDK as the main toggle to display promotion code field on the search form or not. Use empty string to show the field or a promotion code string to show the field and pre populate it.</dd>
  <dt>recentSearch</dt><dd>[Optional] A CTRecentSearch object used to open the SDK on the vehicle list page and bypass the search screen.</dd>
</dl>