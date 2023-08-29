---
layout: default
title: Property Descriptions
parent: In Path
grand_parent: Android Integration
nav_order: 2
permalink: /docs/android/inpath/property-descriptions/
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

### CTSdkData Builder
{: .no_toc }

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
### dropOffDateTime(dropOffDateTime: LocalDateTime)
An optional LocalDateTime that indicates the rental drop off date.

{: .note-title }
> Note
>
> If this is not called the drop off date will be set to the same value set in the pickupDateTime + 3 days

---
### dropOffLocationId
A string OTA Location ID for drop off location, e.g "11" for Dublin.

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
A string value that represents the Order ID for a Flight PNR or Booking Reference, Example: IE1234 (limited to 32 characters)

---
### passenger
An optional CTSdkPassenger, passing this will pre-populate the driver details form during the booking flow.

---
### pickupDateTime
A <b>required</b> LocalDateTime that indicates the rental pickup date.

---
### pickupLocationIATA
A string IATA code for pickup location, e.g "DUB" for Dublin.

---
### pickupLocationId
A string OTA Location ID for pickup location, e.g "11" for Dublin.

{: .note-title }
> Note
>
> This is required if `pickupLocationIATA` is not set.

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
