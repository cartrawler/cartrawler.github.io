---
layout: default
title: Implementation Steps
parent: Standalone
grand_parent: Android Integration
nav_order: 1
permalink: /docs/android/standalone/implementation-steps/
---

# Implementation Steps
{: .no_toc }

To implement the SDK's Standalone flow within your app, please use the following steps:

<details open markdown="block">
  <summary>
    Steps
  </summary>
  {: .text-delta }
1. TOC
{:toc}
</details>

<details markdown="block">
  <summary>
    Alternatives Standalone flows
  </summary>
  {: .text-delta }
- <a href="/docs/android/standalone/implementation-steps#start-standalone-flow-in-the-vehicle-list-screen-bypass-the-landing-and-search-screens-">Start Standalone Flow in the vehicle list screen</a>
- <a href="/docs/android/standalone/implementation-steps#start-standalone-flow-on-the-vehicle-list-via-recent-search-bypass-the-landing-and-search-screens-">Start Standalone Flow on the Vehicle List via Recent Search</a>
- <a href="/docs/android/standalone/implementation-steps#start-with-a-vehicle-pinned-to-the-top-of-the-list-bypass-the-landing-and-search-screens-">Start with a vehicle pinned to the top of the list</a>
- <a href="/docs/android/standalone/implementation-steps#start-the-standalone-flow-via-url-deeplink">Start the Standalone Flow via URL Deeplink</a>
</details>

---

## Initialise the SDK <br/>

Initialise the SDK by calling the init method in the CartrawlerSDK class as follow:

```kotlin
val partnerImplementationID = "your-implementation-id-here"
val environment = CTSdkEnvironment.DEVELOPMENT // CTSdkEnvironment.PRODUCTION

CartrawlerSDK.init(partnerImplementationID, environment)
```

{: .important }
The `implementationID` is needed by the SDK to fetch partner specific configuration. It's recommended to call CartrawlerSDK.init in your application class.<br/>

{: .warning }
Don't forget to use `CTSdkEnvironment.PRODUCTION` when submitting your app to the Play Store.

---

## Initialise CTSdkData <br/>

```kotlin
val sdkDataClientIdXYZ = CTSdkData.Builder(clientId = clientId)
    .country(twoLetterISOCountry = "IE")
    .currency(currency = "EUR")
    // check Property Descriptions link down below to see all available properties
```

{: .note-title }
> Optional properties
>
> The `CTSdkData` builder also has some optional properties that can be passed in during initialisation to use and/or display certain features, you can check
<a href="/docs/android/standalone/property-descriptions/" target="_blank">Property Descriptions</a> section for all properties available.

---

## Starting the flow <br/>

```kotlin
CartrawlerSDK.start(
    activity = this, 
    requestCode = YOUR_REQUEST_CODE_HERE, 
    ctSdkData = sdkDataClientIdXYZ.build(), 
    flow = CTSdkFlow.Standalone()
)
``` 

{: .note-title }
> Standalone Navigation types
> 
> Standalone flow accepts a parameter `navigateTo` of type `CTStandaloneNavigation`. If you're using Kotlin you can omit it since its default value is `CTStandaloneNavigation.CTNavigateToLanding`.
> 
> The following types are allowed:
>
> `CTStandaloneNavigation.CTNavigateToAvailability`<br/>
> `CTStandaloneNavigation.CTNavigateToAvailabilityWithPinnedVehicle("<vehicle_ref_id_here>")`<br/>
> `CTStandaloneNavigation.CTNavigateToAvailabilityWithRecentSearch("<ct_recent_search_data_here>")`<br/>
> `CTStandaloneNavigation.CTNavigateToLanding`<br/>
> `CTStandaloneNavigation.CTNavigateWithDeepLink`<br/>

{: .important }
The `requestCode` is defined by the consumer app, since it will need to use it inside its `onActivityResult` conditional to capture the booking payload that the SDK sends it back to the consumer app.

---

## Start Standalone Flow in the vehicle list screen (bypass the landing and search screens) <br/>
{: .no_toc }

```kotlin
val sdkDataClientIdXYZ = CTSdkData.Builder(clientId = clientId)
    .pickupDateTime(LocalDateTime.of(2023, 5, 10, 10, 0))
    .dropOffDateTime(LocalDateTime.of(2023, 5, 15, 10, 0))
    .pickupLocationIATA("DUB")

CartrawlerSDK.start(
    activity = this,
    requestCode = YOUR_REQUEST_CODE_HERE,
    ctSdkData = sdkDataClientIdXYZ.build(),
    flow = CTSdkFlow.Standalone(navigateTo = CTStandaloneNavigation.CTNavigateToAvailability)
)
```

---

## Start Standalone Flow on the Vehicle List via Recent Search (bypass the landing and search screens) <br/>
{: .no_toc }

For this alternate starting point in the flow, a recent search (CTRecentSearchData object) must be provided. To get a CTRecentSearchData, you can use our <a href="/docs/api/android/recent-searches">Recent Searches API</a>.

```kotlin
val sdkDataClientIdXYZ = CTSdkData.Builder(clientId = clientId)
    .country(twoLetterISOCountry = "IE")
    .currency(currency = "EUR")

CartrawlerSDK.start(
    activity = this,
    requestCode = YOUR_REQUEST_CODE_HERE,
    ctSdkData = sdkDataClientIdXYZ.build(),
    flow = CTSdkFlow.Standalone(
        navigateTo = CTStandaloneNavigation.CTNavigateToAvailabilityWithRecentSearch(/*<ct_recent_search_data_here>*/)
))
```

---

## Start with a vehicle pinned to the top of the list (bypass the landing and search screens) <br/>
{: .no_toc }

A vehicle can be pinned to the top of the list by passing a vehicle reference ID. To get a vehicle reference ID, you can use our <a href="/docs/api/android/vehicles">Vehicles API</a>

```kotlin
val sdkDataClientIdXYZ = CTSdkData.Builder(clientId = clientId)
    .country(twoLetterISOCountry = "IE")
    .currency(currency = "EUR")
    .pickupDateTime(LocalDateTime.of(2023, 5, 10, 10, 0))
    .dropOffDateTime(LocalDateTime.of(2023, 5, 15, 10, 0))
    .pickupLocationIATA("DUB")

CartrawlerSDK.start(
    activity = this,
    requestCode = YOUR_REQUEST_CODE_HERE,
    ctSdkData = sdkDataClientIdXYZ.build(),
    flow = CTSdkFlow.Standalone(
        navigateTo = CTStandaloneNavigation.CTNavigateToAvailabilityWithPinnedVehicle(vehicleRefId = "vehicle_ref_id_here")
    )
)
```

---

## Start standalone flow on the manage my booking screen (bypass the landing) <br/>
{: .no_toc }

To start from this alternate point in the flow, a CTBooking object must be provided. If the booking is not already available in the app, it will be imported first, and the user will then be redirected.

You can provide a booking ID from any booking made on your platforms (web, iOS, Android, etc.). If the booking needs to be imported (e.g., it was not created using the Android SDK), ensure that you also supply the associated email address

```kotlin
val sdkDataClientIdXYZ = CTSdkData.Builder(clientId = clientId)
    .country(twoLetterISOCountry = "IE")
    .currency(currency = "EUR")

val booking = CTBooking(
    id = "IE123456789, 
    email = "mail@mail.com" //optional see description above
)    

CartrawlerSDK.start(
    activity = this,
    requestCode = YOUR_REQUEST_CODE_HERE,
    ctSdkData = sdkDataClientIdXYZ.build(),
    flow = CTSdkFlow.Standalone(
        navigateTo = CTStandaloneNavigation.CTNavigateToBookingDetails(bookingDetails = booking)
    )
)
```

---

## Start the Standalone Flow via URL Deeplink
{: .no_toc }

It is possible to launch the Standalone flow using an URL, which may come from a push notification or native app deep link for example.
This is entirely optional.

{: .note}
The URL should have the following pattern: <br/>
`schema://ct-car-rental?param1=123&param2=abcd&paramN=xyz`<br/>

This example will open the vehicle list / search results screen: <br />
`schema://ct-car-rental?type=search-result&client_id=your-client-id&pt=2023-08-18T10:00:00Z&dt=2023-08-20T10:00:00Z&pkIATA=DUB&doIATA=ORK`

{: .warning}
A client ID <small>(client_id)</small> <b>must</b> be provided as part of your URL in order for the SDK to function properly.<br/>

Once you have the URL you can call the following method to launch the SDK:

```kotlin

val deepLinkURL = "schema://ct-car-rental?type=landing&client_id=your-client-id"
val isLoggingHttpRequests = BuildConfig.DEBUG
val ctSdkData = CTSdkData.Builder("")
    .theme(R.style.your-theme-here)
    .darkModeConfig(AppCompatDelegate.MODE_NIGHT_FOLLOW_SYSTEM)
    .logging(isLoggingHttpRequests)
        
CartrawlerSDK.start(
    activity = this,
    requestCode = YOUR_REQUEST_CODE_HERE,
    ctSdkData = ctSdkData.build(),
    CTStandaloneNavigation.CTNavigateWithDeepLink(deepLinkURL)
)
```

### Start Standalone flow on the Landing screen
{: .no_toc }

To open the SDK on the landing screen, simply set the type to landing: `type=landing`

### Start Standalone flow on the Vehicle List (bypass Landing and Search screens)
{: .no_toc }

To open the SDK on the vehicle list screen, simply set the type to search-result: `type=search-result`, make sure to provide pickup and drop off dates, and a pickup location ID, or IATA code. If any of these are missing, the SDK will instead open on the landing page.

### Start Standalone flow on the Manage Booking screen
{: .no_toc }

To open the SDK on the manage booking screen, simply set the type to booking-details: `type=booking-details` and make sure to provide the booking id and email. Email is only required to import the booking if it was made through a different platform (e.g iOS, Web)

### List of Parameters
{: .no_toc }

Below are all the available parameters for use in the URL. As you will see, some of them are required in order to deep link to the vehicle list. If these are missing, the SDK will open on the landing screen.

##### Landing
{: .no_toc }

| Parameter           | Example | Required | 
|:--------------------|:--------|:---------|
| type                | landing | yes      |
| client_id           | 123456  | yes      |
| ctyCode (residency) | IE      | no       |
| ccy (currency)      | EUR     | no       |


##### Manage Booking
{: .no_toc }

| Parameter           | Example          | Required                                                              | 
|:--------------------|:-----------------|:----------------------------------------------------------------------|
| type                | booking -details | yes                                                                   |
| client_id           | 123456           | yes                                                                   |
| ctyCode (residency) | IE               | no                                                                    |
| ccy (currency)      | EUR              | no                                                                    |
| booking             | IE123456789      | yes                                                                   |
| email               | mail@mail.com    | no (only required if importing the booking from a different platform) |


##### Search Results
{: .no_toc }

| Parameter                 | Example              | Required                | 
|:--------------------------|:---------------------|:------------------------|
| type                      | search-result        | yes                     |
| client_id                 | 123456               | yes                     |
| pt (pickup time)          | 2023-08-18T10:00:00Z | yes                     |
| dt (drop off time)        | 2023-08-20T10:00:00Z | yes                     |
| pkIATA (pickup IATA)      | DUB                  | yes (if pl not set)     |
| doIATA (drop off IATA)    | DUB                  | no                      |
| pl (pickup location ID)   | 11                   | yes (if pkIATA not set) |
| dl (drop off location ID) | 11                   | no                      |
| age                       | 30                   | no                      |
| ctyCode (residency)       | IE                   | no                      |
| ccy (currency)            | EUR                  | no                      |
| pinVeh (pinned vehicle)   | 123456789            | no                      |
