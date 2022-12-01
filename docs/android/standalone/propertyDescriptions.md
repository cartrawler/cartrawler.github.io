---
layout: default
title: Property Descriptions
parent: Standalone
grand_parent: Android
nav_order: 2
permalink: /docs/android/standalone/property-descriptions/
---

# Property Descriptions

{: .no_toc }

---

Standalone Builder

<dl>
<dt>requestCode</dt><dd>Partner provided unique integer value used to send back reservation details from Systems API.</dd>
<dt>setClientId</dt><dd>Your client ID, required to use the CarTrawler API.</dd>
<dt>setAccountId</dt><dd>A String value that represents the Account ID.</dd>
<dt>setCountry</dt><dd>An optional country code to used switch between languages.</dd>
<dt>setCurrency</dt><dd>An optional currency code, based on the ISO standard currency codes e.g "USD". The currency associated with the device’s system region is used by default.</dd>
<dt>setEnvironment</dt><dd>Switch between CarTrawlers endpoints for STAGING and PRODUCTION environments.</dd>
<dt><small>setFlightNumberRequired</small></dt><dd>A boolean key to enable Flight Number as a required field in the Payment Form.</dd>
<dt>setLogging</dt><dd>Boolean value for additional logging while debugging.</dd>
<dt>setOrderId</dt><dd>A String value that represents the Order ID for a Flight PNR or Booking Reference, Example: IE1234</dd>
<dt>setPassenger</dt><dd>An optional Array of Passengers, the first one will be the main passenger.</dd>
<dt>setVisitorId</dt><dd>A String value that represents the Visitor ID.</dd>
<dt><small>startRentalStandalone</small></dt><dd>Start Rental standalone activity.</dd>
<dt><span style="font-size:0.7em">enableCustomCashTreatment</span></dt><dd>This is used in the SDK in order to enable enhanced cash voucher merchandising throughout the booking flow. (Cash voucher banner, Cash voucher widgets etc)</dd>
<dt>setUSPDisplayType</dt><dd>This allows Partners to choose which homepage USP style (icons (DEFAULT_STYLE) or checks (CHECK_STYlE)) they want to use in the app. Note: If you don't call this method DEFAULT_STYLE will be used. </dd>
<dt>addPromotionCode</dt><dd>This allows Partners to pass a promotion code type to the SDK as the main toggle to display promotion code field on the search form or not.  </dd>
<dt>setClientUserIdentifier</dt><dd>A string token of a logged in user that allows the SDK to fetch the user loyalty details and points.</dd>
</dl>

---

### PromotionCode Types ###

<dl>
<dt>UseInAppInputType</dt><dd>This will display an input field that will allow the user to input/paste a promotion code that will be sent to Availability Request.
</dd>
<dt><span style="font-size:0.9em">WithCodeType("code")</span></dt><dd>This will send the promotion code provided in the Availability Request. The input field will also be shown to the user in the search form.
</dd></dl>

Note: If ```addPromotionCode``` method is not called with any of the options described above the promotion code field will not be shown in the search form screen.




