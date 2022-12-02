---
layout: default
title: Implementation Steps
parent: Standalone
grand_parent: Android
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
    Optional Extras
  </summary>
  {: .text-delta }
1. <a href="/docs/ios/standalone/implementation-steps#start-the-standalone-flow-on-another-screen">Start the Standalone Flow on Another Screen</a>
- <a href="/docs/android/standalone/implementation-steps#start-standalone-flow-on-the-search-screen-bypass-landing-screen">Start Standalone flow on the Search Screen (bypass Landing Screen)</a>
- <a href="/docs/android/standalone/implementation-steps#start-standalone-flow-on-the-vehicle-list-via-pickup--drop-off-bypass-the-landing-and-search-screens">Start Standalone flow on the Vehicle List via Pickup & Drop Off (bypass the landing and search screens)</a>
<!-- - <a href="/docs/ios/standalone/implementation-steps#start-standalone-flow-on-the-vehicle-list-via-recent-search-bypass-the-landing-and-search-screens">Start Standalone Flow on the Vehicle List via Recent Search (bypass the landing and search screens)</a> -->
</details>

---

### Initialise the SDK by implementing the SDK Builder <br/>

{: .note}
.setEnvironment(CartrawlerSDK.Environment.PRODUCTION) must be set when submitting your app to the Play Store.

```java
CartrawlerSDK.Builder()
       .setRentalStandAloneClientId(clientId = "your clientID") // Ask your partner manager for your client id
       .setAccountId("CZ638817950")
       .setCountry(countryISO)
       .setCurrency(currency)
       .setEnvironment(environment)
       .setFlightNumberRequired(true)
       .setLogging(true)
       .setOrderId("123")
       .setPassenger(passenger(countryISO))
       .setVisitorId("123")
       .setTheme(R.style.SampleTheme)
       .startRentalStandalone(activity, requestCode = REQUEST_CODE_STANDALONE)
```

{: .note}
The SDK builder also has some optional properties that can be passed in during initialisation to use and/or display certain features:

```java
       .enableCustomCashTreatment()
       .setUSPDisplayType(USPDisplayType.CHECK_STYLE)
       .addPromotionCode(PromotionCodeType.WithCodeType("your-promotion-code-here")
       .setClientUserIdentifier("your-client-user-identifier-here")
```       

<small>Click <a href="/docs/android/standalone/property-descriptions">here</a> for an in depth explanation of the SDK builder's properties.</small>


---

## Start the Standalone Flow on Another Screen
{: .no_toc }

When launching the Standalone flow, it is possible to bypass the landing and search screens by setting certain properties of the SDK builder.   
This is entirely optional. 
<br/>

### Start Standalone flow on the Search Screen (bypass Landing Screen)
{: .no_toc }

```java
CartrawlerSDK.Builder()
       //... 
       .setRentalStandAloneClientId(clientId)
       .startSearchFlow(activity, requestCode)
```

### Start Standalone flow on the vehicle list <b>via Pickup & Drop Off</b> (bypass the landing and search screens)
{: .no_toc }

{: .important }
For this alternate starting point in the flow, pick-up and drop-off locations and dates must be provided.

{: .note }
A vehicle can be pinned to the top of the list by setting a pinned vehicle (adding a vehicle reference ID). To get a reference ID, you can use our <a href="/docs/android/apis/vehicles">Vehicles API</a>.

<!-- To support navigation to the car block screen you need to add pinned veh ref along with the drop-off time, pick-up time and the pick-up and drop-off locations as follows: --> 
```java
CartrawlerSDK.Builder()
       .setPickupTime(pickupDateTime = GregorianCalendar())
       .setDropOffTime(dropOffDateTime = GregorianCalendar())
       .setPickupLocation(iataAirportCode = "YXJ")
       .setPinnedVehicle(abandonmentRefId = "123456789") 
       //.. Add your other config properties as normal
       .startAvailabilityFlow(activity, YOUR_REQUEST_CODE)
```

### Start Standalone Flow on the Vehicle List via Recent Search (bypass the landing and search screens)
{: .no_toc }

{: .important }
For this alternate starting point in the flow, a recent search (`RecentSearch` object) must be provided.

```java
CartrawlerSDK.Builder()
       //...
       .withRecentSearch(recentSearch) // the recent search object fetched from the recent searches api. 
       .startAvailabilityFlow(activity, YOUR_REQUEST_CODE)
```

{: .note}
To get a recent search, you can use our <a href="/docs/android/apis/recent-searches">Recent Searches API</a>.