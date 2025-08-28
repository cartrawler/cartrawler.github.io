---
layout: default
title: Property Descriptions
parent: In Path with Grid View
grand_parent: Android Integration
nav_order: 2
permalink: /docs/android/inpath-gridview/property-descriptions/
---

# In Path Property Descriptions
{: .no_toc }

<details open markdown="block">
  <summary>
    Properties
  </summary>
  {: .text-delta }
1. TOC
{:toc}
</details>

---

## CTSdkData Builder
{: .no_toc }

---
### clientId
A string identifier provided by CarTrawler required to use the CarTrawler API.

---
### country
An optional two letter country code string, based on the ISO standard e.g "US", "IE" used on the CarTrawler API. The country code associated with the device’s system region is used by default.

---
### currency
An optional currency code string, based on the ISO standard currency codes e.g "USD". The currency associated with the device’s system region is used by default.

---
### theme
An `@StyleRes` int that is used to setup the theme for the SDK, for more details check <a href="/docs/android/customisation/themes#styling-the-sdk-and-initialising-your-theme">Themes</a>.

---

## flightDetails
{: .no_toc }
To initialise the In Path Grid View flow, it is necessary to build and set a CTFlighDetails object to the CTSdkData.

| Parameter                | Description                                                                                                 | Example                                                                  | Type       | 
|:-------------------------|:------------------------------------------------------------------------------------------------------------|:-------------------------------------------------------------------------|------------|
| flightOrigin             | <b>[Required]</b> Origin flight details separated by pipes: IATA departure, IATA arrival, datetime departure, datetime arrival, flight number.                                                                                                        | DUB\|LHR\|2025-01-16T06:05:00\|2025-01-16T07:25:00\|XX8719               | String     |
| flightReturn             | <b>[Required] (return flights only)</b> Return flight details separated by pipes: IATA departure, IATA arrival, datetime departure, datetime arrival, flight number.                   | LHR\|DUB\|2025-01-22T21:20:00\|2025-01-23T22:45:00\|XX8720                                                                        | String    |
| passengerBreakdown       | Breakdown of Adults, Teens, Children and Infants                                                            | CTFlightPassengerBreakdown(adults = 2, teens = 0, children = 0, infants = 0) | CT Object  |
| firstName                | First name of the customer.                                                                                 | John                                             | String     |
| surName                  | Surname of the customer.                                                                         | Doe                                                      | String     |
| customerEmail            | Email address of the customer.                                                                                                | john.doe@example.com                                                                  | String     |
| fareClass                | Flight class selected by the customer. Map to values like economy, business, etc.                           | business                                                                 | String     |
| flightFare               | Total price of the flight fare in the selected currency.                                                    | 175.95                                                                   | Float     |
| bags                     | Number of extra bags added by the customer.                                                                | 1                                                                        | Integer       |
| loyaltyNumber            | Loyalty program membership ID, if applicable.                                                              | ABC123456                                                                 | String     |
| loyaltyTier              | Loyalty program tier name, if applicable.                                                                  | gold                                                                      | String     |
| context                  | Where in the flow the grid view is loaded. INPATH, CONFIRM or MMB                                          | INPATH                                                                    | String     |


```java
//Example CTFLightDetails initialisation
val flightDetails = CTFlightDetails.Builder()
        .flightOrigin("DUB|LHR|2025-01-16T06:05:00|2025-01-16T07:25:00|XX8719")
        .flightReturn("LHR|DUB|2025-01-22T21:20:00|2025-01-23T22:45:00|XX8720")
        .passengerBreakdown(CTFlightPassengerBreakdown(adults = 2, teens = 0, children = 0, infants = 0))
        .firstName("John")
        .surName("Doe")
        .customerEmail("john.doe@example.com")
        .fareClass("business")
        .flightFare(175.95)
        .bags(1)
        .loyaltyNumber("ABC123456")
        .loyaltyTier("gold")
        .context("INPATH")
        .build()

//Use CTFlightDetails when initialising CTSdkBuilder
val sdkData = CTSdkData.Builder(clientId = clientId).flightDetails(flightDetails).build()
```
