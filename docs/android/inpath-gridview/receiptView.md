---
layout: default
title: Receipt View
parent: In Path with Grid View
grand_parent: Android Integration
nav_order: 4
permalink: /docs/android/inpath-gridview/receiptView
---


## Receipt Lookup / Booking Details
<br />

The **Receipt View** allows users to retrieve and display their car hire reservation by providing a **Booking ID** and the **email used at booking**.  
Once set, the SDK loads the booking details — including pick-up and drop-off information, supplier and vehicle details, pricing, and current booking status — and renders the experience directly inside the Grid View flow.

--- 

### Implementation

Set the booking lookup fields in the `context`, configure the `flightDetails.context`, then request the Grid View:

```java
val flightDetails = CTFlightDetails.Builder()
        .context("MMB") // MMB or CONFIRM
        .build()

val sdkData = CTSdkData.Builder(clientId = clientId)
    .country(twoLetterISOCountry = "IE")
    .currency(currency = "EUR")
    .flightDetails(flightDetails)
    .booking(CTBooking("ES845524110", "test@test.com"))
    .build()

val receiptView = CartrawlerSDK.getInPathGridView(
    activity = this,
    requestCode = YOUR_REQUEST_CODE_HERE,
    ctSdkData = sdkData,
    flow = CTSdkFlow.InPath()
)
```