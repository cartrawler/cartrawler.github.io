---
layout: default
title: Property Descriptions
parent: In Path with Grid View
grand_parent: iOS Integration
nav_order: 2
permalink: /docs/ios/inpath-gridview/property-descriptions
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
<dt><b>style</b></dt><dd>An optional <a href="/docs/ios/customisation/themes#creating-a-ctstyle">CTStyle</a> object, used to set the fonts as well as the primary and secondary colors in the SDK. </dd>
</dl>

{: .important}
Please ensure any custom fonts used are included in your main bundle.

## Initialising CTContext for In Path Grid View
To initialise the In Path flow, it is necessary to instantiate a CTContext and CTFlighDetails objects.

| Parameter                | Description                                                                                                                                      | Example                                | Type            | 
|:-------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------ |:---------------------------------------|-----------------|
| implementationID         |  <b>[Required]</b> An implementation ID, provided by CarTrawler and required to fetch the partner configuration.                                 | 9f3e1d7c-3e44-49f2-b1c9-ra4e7c2f59b    | String          |
| clientID                 |  <b>[Required]</b> A client ID, required to use the CarTrawler API.                                                                              | 564488                                 | Integer         |
| flow                     |  <b>[Required]</b> The flow to be launched. Must be <b>.inPath</b>                                                                               | .inPath                                | Enum            |
| countryCode              |  A country code, such as "US". Default is the device location if not provided.                                                                   | IE                                     | String          |
| currencyCode             | A currency code, based on the ISO standard currency codes e.g "USD". The currency associated with the device’s system region is used by default. | EUR                                    | String          |
| languageCode             | A language code to switch between languages. Default is "EN" if not provided.                                                                    | Economy                                | String          |
| flightDetails            |  <b>[Required]</b> The flight details                                                                                                            | CTFlightDetails()                      | CT Object.      |


CTFlightDetails object:

| Parameter                | Description                                                                                                 | Example                                                                  | Type       | 
|:-------------------------|:------------------------------------------------------------------------------------------------------------|:-------------------------------------------------------------------------|------------|
| flightOrigin             | <b>[Required]</b> Origin flight details separated by pipes: IATA departure, IATA arrival, datetime departure, datetime arrival, flight number.                                                                                                        | DUB\|LHR\|2025-01-16T06:05:00\|2025-01-16T07:25:00\|XX8719               | String     |
| flightReturn             | <b>[Required] (return flights only)</b> Return flight details separated by pipes: IATA departure, IATA arrival, datetime departure, datetime arrival, flight number.                   | LHR\|DUB\|2025-01-22T21:20:00\|2025-01-23T22:45:00\|XX8720                                                                        | String    |
| passengerBreakdown       | Breakdown of Adults, Teens, Children and Infants                                                            | CTFlightPassengerBreakdown(adults: 2, teens: 0, children: 0, infants: 0) | CT Object  |
| firstName                | First name of the customer.                                                                                 | John                                             | String     |
| surName                  | Surname of the customer.                                                                         | Doe                                                      | String     |
| customerEmail            | Email address of the customer.                                                                                                | john.doe@example.com                                                                  | String     |
| fareClass                | Flight class selected by the customer. Map to values like economy, business, etc.                           | business                                                                 | String     |
| flightFare               | Total price of the flight fare in the selected currency.                                                    | 175.95                                                                   | Double     |
| bags                     | Number of extra bags added by the customer.                                                                | 1                                                                        | UInt       |
| loyaltyNumber            | Loyalty program membership ID, if applicable.                                                              | ABC123456                                                                 | String     |
| loyaltyTier              | Loyalty program tier name, if applicable.                                                                  | gold                                                                      | String     |

