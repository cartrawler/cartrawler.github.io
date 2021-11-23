---
layout: default
title: Property Descriptions
parent: Standalone
grand_parent: Android
nav_order: 3
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
<dt>setFlightNumberRe...</dt><dd>A boolean key to enable Flight Number as a required field in the Payment Form.</dd>
<dt>setLogging</dt><dd>Boolean value for additional logging while debugging.</dd>
<dt>setOrderId</dt><dd>A String value that represents the Order ID for a Flight PNR or Booking Reference, Example: IE1234</dd>
<dt>setPassenger</dt><dd>An optional Array of Passengers, the first one will be the main passenger.</dd>
<dt>setVisitorId</dt><dd>A String value that represents the Visitor ID.</dd>
<dt>startRentalStandalone</dt><dd>Start Rental standalone activity.</dd></dl>
