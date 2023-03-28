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
- <a href="/docs/android/standalone/implementation-steps#start-standalone-flow-in-the-search-screen-bypass-the-landing-screen-">Start Standalone Flow in the Search screen</a>
- <a href="/docs/android/standalone/implementation-steps#start-standalone-flow-in-the-vehicle-list-screen-bypass-the-landing-and-search-screens-">Start Standalone Flow in the vehicle list screen</a>
- <a href="/docs/android/standalone/implementation-steps#start-standalone-flow-on-the-vehicle-list-via-recent-search-bypass-the-landing-and-search-screens-">Start Standalone Flow on the Vehicle List via Recent Search</a>
- <a href="/docs/android/standalone/implementation-steps#start-with-a-vehicle-pinned-to-the-top-of-the-list-bypass-the-landing-and-search-screens-">Start with a vehicle pinned to the top of the list</a>
</details>

---

### Initialise the SDK <br/>

Initialise the SDK by calling the init method in the CartrawlerSDK class as follow:

```kotlin
val partnerImplementationID = "your-implementation-id-here"
val environment = CTSdkEnvironment.DEVELOPMENT // CTSdkEnvironment.PRODUCTION

CartrawlerSDK.init(partnerImplementationID, environment)
```

{: .important }
The `implementationID` is needed by the SDK since it's used to fetch some configuration. It's recommended to call CartrawlerSDK.init in your application class.<br/>

{: .warning }
Don't forget to use `CTSdkEnvironment.PRODUCTION` when submitting your app to the Play Store.

---

### Initialise CTSdkData <br/>

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

### Starting the flow <br/>

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
> `CTStandaloneNavigation.CTNavigateToSearch`

{: .important }
The `requestCode` is defined by the consumer app, since it will need to use it inside its `onActivityResult` conditional to capture the booking payload that the SDK sends it back to the consumer app.

---

### Start Standalone Flow in the Search screen (bypass the landing screen) <br/>
{: .no_toc }

This flow will bypass the landing screen and set the start screen as Search, you only need to set the `flow` in CartrawlerSDK.start to:

```kotlin
CartrawlerSDK.start(
    activity = this,
    requestCode = YOUR_REQUEST_CODE_HERE,
    ctSdkData = sdkDataClientIdXYZ.build(),
    flow = CTSdkFlow.Standalone(navigateTo = CTStandaloneNavigation.CTNavigateToSearch)
)
```

---

### Start Standalone Flow in the vehicle list screen (bypass the landing and search screens) <br/>
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

### Start Standalone Flow on the Vehicle List via Recent Search (bypass the landing and search screens) <br/>
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

### Start with a vehicle pinned to the top of the list (bypass the landing and search screens) <br/>
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
