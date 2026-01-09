---
layout: default
title: Receipt View
parent: In Path with Grid View
grand_parent: iOS Integration
nav_order: 4
permalink: /docs/ios/inpath-gridview/receiptView
---

## Receipt Lookup / Booking Details
<br />

The **Receipt View** allows users to retrieve and display their car hire reservation by providing a **Booking ID** and the **email used at booking**.  
Once set, the SDK loads the booking details — including pick-up and drop-off information, supplier and vehicle details, pricing, and current booking status — and renders the experience directly inside the Grid View flow.

--- 

### Implementation

Set the booking lookup fields in the `context`, configure the `flightDetails.context`, then request the Grid View:

```swift
// Previous configured context with lang, currency
context.bookingID = "ES845524110"
context.bookingEmail = "test@test.com"

let flightDetails = CTFlightDetails()
flightDetails.context = "MMB" // MMB or CONFIRM
context.flightDetails = flightDetails

// Request grid view
let gridView = CarTrawlerSDK.sharedInstance().getGridView(from: self, context: context)
```