---
layout: default
title: Property Descriptions
parent: Standalone
grand_parent: Android Integration
nav_order: 2
permalink: /docs/android/standalone/property-descriptions/
---

# Standalone Property Descriptions
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

### Pick Up & Drop Off Location parameters
Pick up location can be passed as a IATA, coordinates or OTA Location ID. 
Priority is IATA > OTA location id > coordinates

- 'pickupLocationIATA' and 'dropOffLocationIATA':
  It takes a three-letter code that represents airports worldwide. E.g 'DUB' for Dublin.

- 'pickUpLocationCoordinate's and 'dropOffLocationCoordinates':
  It takes a CTCoordinates object:
  ```java 
  data class CtCoordinates(
      val latitude: Double,
      val longitude: Double
  )
  ```
- 'pickupLocationId' and 'dropOffLocationId':
  It takes a string OTA Location ID for pickup location, e.g “11” for Dublin.


### addPromotionCode(promotionCodeType: CTPromotionCodeType)
This allows Partners to pass a promotion code type to the SDK as the main toggle to display promotion code field on the search form or not. The following types can be used:
```kotlin
/**
 * This will display an input field that will allow the user to input/paste a promotion code that will be sent to Availability Request.
 */
CTPromotionCodeType.UseInAppInputType

/**
 * This means that a hardcoded promotion code will be injected by the partner in the
 * SDK initialization and also a field will be displayed to the user that can change the
 * code
 */
CTPromotionCodeType.WithCodeType(promotionCode: String)
```

{: .note-title }
> Note
>
> If `addPromotionCode` method is not called with any of the options described above the promotion code field will not be shown in the search form screen.

---

### accountId
A string value that represents the Account ID.

---
### clientId
A string identifier provided by CarTrawler required to use the CarTrawler API.

---
### clientUserIdentifier
A string token of a logged in user that allows the SDK to fetch the user loyalty details and points.

---
### country
An optional two letter country code string, based on the ISO standard e.g "US", "IE" used on the CarTrawler API. The country code associated with the device’s system region is used by default.

---
### currency
An optional currency code string, based on the ISO standard currency codes e.g "USD". The currency associated with the device’s system region is used by default.

---
### darkModeConfig
An optional int that can be passed to the SDK to control whether or not Dark Mode should be used, for more details check <a href="/docs/android/customisation/dark-mode##set-up">Dark Mode</a>.
The following options can be used:
```kotlin
AppCompatDelegate.MODE_NIGHT_FOLLOW_SYSTEM
AppCompatDelegate.MODE_NIGHT_YES
AppCompatDelegate.MODE_NIGHT_NO
```
---
### enableCustomCashTreatment
Call this method to display enhanced cash voucher merchandising throughout the booking flow.

---
### enableSupplierBenefitAutoApplyCodes
A boolean that allows Partners to initialise the SDK and opt in to apply ALL automatic codes that can be applied for suppliers.

---
### flightNumberRequired
A boolean key to enable Flight Number as a required field in the Payment Form. Flight number required is enabled by default.

---
### logging
A boolean value for additional logging while debugging.

---
### loyaltyRegex
A string regex that if passed will be used to validate the loyalty membership id in the payment form.

---
### orderId
A String value that represents the Order ID for a Flight PNR or Booking Reference, Example: IE1234 (limited to 32 characters)

---
### passenger
An optional CTSdkPassenger, passing this will pre-populate the driver details form during the booking flow.

The CTSdkPassenger object has the following optional parameters:

| Parameter          | Description                                  | Usage                                                                                                                                      | Type      | 
|:-------------------|:---------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------|-----------|
| firstName          | Customer's name                              | First name displayed on the driver details screen                                                                                          | String    |
| lastName           | Customer's last name                         | Last name displayed on the driver details screen                                                                                           | String    |
| email              | Customer's email                             | Email displayed on the driver details screen                                                                                               | String    |
| phoneCountryCode   | Customer's 3 digit phone country code        | Phone code displayed on the driver details screen. If not set, it is retrieved from user's country code                                    | String    |
| phoneNumber        | Customer's phone number in national format   | Phone number displayed on the driver details screen                                                                                        | String    |
| address            | Customer's Address                           | Address displayed on the driver details screen                                                                                             | String    |
| city               | Customer's city                              | City displayed on the driver details screen                                                                                                | String    |
| postcode           | Customer's postcode                          | Postcode displayed on the driver details screen                                                                                            | String    |
| country            | Customer's country                           | Country displayed on the driver details screen. If not set, it fallbacks to the country set in the main CTSdkData or the device's country  | String    |
| flightNumber       | Customer's flight number                     | Flight number displayed on the driver details screen                                                                                       | String    |
| age                | Customer's age                               | Age displayed on the driver details screen                                                                                                 | String    |
| membershipId       | Customer's loyalty program id                | Loyalty number displayed on the driver details screen and also used to retrieve loyalty points to be displayed during the booking flow     | String    |

```java
//Example CTSdkPassenger initialisation
val passenger = CTSdkPassenger.Builder()
        .firstName("John")
        .lastName("Murphy")
        .email("mail@mail.com")
        .phoneCountryCallingCode("353")
        .phoneNumber("08666666666")
        .address("Pearse Stree 738")
        .city("Cork")
        .postCode("W99NH99")
        .country("IE")
        .flightNumber("IE 123")
        .age("29")
        .membershipId("123456")
        .build()

//Use CTSdkPassenger when initialising CTSdkBuilder
val sdkData = CTSdkData.Builder(clientId = clientId).passenger(passenger).build()
```

---
### theme
An `@StyleRes` int that is used to setup the theme for the SDK, for more details check <a href="/docs/android/customisation/themes#styling-the-sdk-and-initialising-your-theme">Themes</a>.

---
### uspDisplayType
This allows Partners to choose which homepage USP style they want to use in the app. The following types can be used:
```kotlin
CTUSPDisplayType.DEFAULT_STYLE
CTUSPDisplayType.CHECK_STYlE
```

{: .note-title }
> Note
>
> If you don't call this method `CTUSPDisplayType.DEFAULT_STYLE` will be used.

---
### visitorId
An optional String value that represents the Visitor ID.

---
### withSettingsMenuIconType(settingsMenuIcon: CTSettingsMenuIcon)
Used to set the menu icon on the toolbar on the landing and search screens, for more details check <a href="/docs/android/customisation/settings/#selecting-an-icon">Settings</a>. The following types can be used:
```kotlin
CTSettingsMenuIcon.COG
CTSettingsMenuIcon.HAMBURGER
CTSettingsMenuIcon.USER
```

---
## Adding Flight Information
{: .no_toc }
For enhanced reporting partners can optionally add flight data when initialising the SDK. Each of these parameters is optional and user functionality will not be impacted whether they are added or not.

A new CTFlightDetails object is added with the following parameters.

| Parameter                | Description                                                                                                 | Example                                                                  | Type      | 
|:-------------------------|:------------------------------------------------------------------------------------------------------------|:-------------------------------------------------------------------------|-----------|
| age                      | Customer age                                                                                                | 29                                                                       | Integer   |
| bags                     | Total number of bags. Both Cabin and Hold should be included                                                | 3                                                                        | Integer   |
| basketAmount             | Total basket amount including bags, seats, etc.                                                             | 130.99                                                                   | Float     |
| campaignID               | Unique identifier associated with a specific marketing campaign.                                            | Google-EN-Destination-France                                             | String    |
| context                  | Where in the flow the SDK is loaded                                                                         | Confirmation Screen                                                      | String    |
| fareClass                | Type of fare                                                                                                | Economy                                                                  | String    |
| flightFare               | Price of flight(s) only                                                                                     | 101.99                                                                   | Float     |
| loyaltyNumber            | Customer loyalty number                                                                                     | 123222121                                                                | String    |
| loyaltyTier              | Customer loyalty tier                                                                                       | Platinum                                                                 | String    |
| marketingPreference      | Flag to indicate if the current user has opted in for third-party marketing i.e car rental                  | true                                                                     | Boolean   |
| marketingSegment         | The customers segment as determined by the airline marketing team.                                          | Weekend Breaker                                                          | String    |
| membershipID             | Only required if different from loyaltyNumber. This value represents the logged in users unique identifier. | 789                                                                      | String    |
| passengerBreakdown       | Breakdown of Adults, Teens, Children and Infants                                                            | CTFlightPassengerBreakdown(adults: 2, teens: 0, children: 0, infants: 0) | CT Object |
| pnr                      | Flight PNR                                                                                                  | CT123456                                                                 | String    |
| sessionID                | Current user session identifier                                                                             | 0idfw78jsnkoo                                                            | String    |
| sportsEquipment          | Total number of sports equipment items a customer is travelling with                                        | 1                                                                        | Integer   |
| sportsEquipmentBreakdown | A breakdown of the individual equipment a customer is travelling with                                       | ["golf": (1), "ski": (1), "surf": (1)]                                   | Map       |
| tripDuration             | Total length of trip in days                                                                                | 4                                                                        | Integer   |
| tripType                 | An identification of whether the trip type is business, leisure, or other                                   | Business                                                                 | String    |

```java
//Example CTFLightDetails initialisation
val flightDetails = CTFlightDetails.Builder()
    .age(32)
    .bags(3)
    .basketAmount(99.90)
    .campaignID("Google-EN-Destination-Ireland")
    .context("confirmation")
    .fareClass("regular")
    .flightFare(101.99)
    .loyaltyNumber("12345")
    .loyaltyTier("Platinum")
    .marketingPreference(true)
    .marketingSegment("Budget Traveller")
    .membershipID("12343")
    .passengerBreakdown(CTFlightPassengerBreakdown(adults = 2, teens = 1, children = 1, infants=1))
    .pnr("12345")
    .sessionID("0idfw78jsnkoo")
    .sportsEquipment(3)
    .sportsEquipmentBreakdown(mapOf("Golf" to 1, "Ski" to 1, "Surf" to 1))
    .tripDuration(2)
    .tripType("leisure")
    .build()

//Use CTFlightDetails when initialising CTSdkBuilder
val sdkData = CTSdkData.Builder(clientId = clientId).flightDetails(flightDetails).build()

```        
---
## Adding UTM tracking data
{: .no_toc }
For enhanced reporting partners can optionally add tracking data in the form of UTM parameters. The meaning and usage of these parameters are <a href="https://en.wikipedia.org/wiki/UTM_parameters">as standard</a>.

A new CTUTMParameters object is added with the following parameters.

```java
//Example CTUTMParameters usage
val utmParameters = CTUTMParameters(
    .utmSource = "partner_utm_source"
    .utmMedium = "partner_utm_medium"
    .utmCampaign = "partner_utm_campaign"
    .utmTerm = "partner_utm_term"
    .utmContent = "partner_utm_content"

//Use CTUTMParameters when initialising CTSdkBuilder
val sdkData = CTSdkData.Builder(clientId = clientId).utmParameters(utmParameters).build()
```