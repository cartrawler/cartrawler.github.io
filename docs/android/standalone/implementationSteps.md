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

---

### Initialise the SDK by implementing the SDK Builder <br/>
<b>Note that the `production` parameter must be set to true when submitting your app to the AppStore.

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

#### Optional Properties
The SDK has some optional properties that can be passed in initialisation to use and/or display certain features

```java
       .enableCustomCashTreatment()
       .setUSPDisplayType(USPDisplayType.CHECK_STYLE)
       .addPromotionCode(PromotionCodeType.WithCodeType("your-promotion-code-here")
```       

---
### Navigation to the Vehicle List

To support navigation to the car block screen you need to add pinned veh ref along with the drop-off time, pick-up time and the pick-up and drop-off locations as follows:
```java
CartrawlerSDK.Builder()
       .setPickupTime(pickupDateTime = GregorianCalendar())
       .setDropOffTime(dropOffDateTime = GregorianCalendar())
       .setPickupLocation(iataAirportCode = "YXJ")
       .setPinnedVehicle(abandonmentRefId = "123456789") 
       //.. Add your other config properties as normal
       .startAvailabilityFlow(activity, YOUR_REQUEST_CODE)
```
---

### Navigation to search screen 

```java
CartrawlerSDK.Builder()
       //... 
       .setRentalStandAloneClientId(clientId)
       .startSearchFlow(activity, requestCode)
```