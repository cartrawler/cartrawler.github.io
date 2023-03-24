---
layout: default
title: Migrate to 14.0.0
parent: Android Integration
nav_order: 5
---

# Migrate from 13.x.x to 14.0.0
{: .no_toc }

<details open markdown="block">
  <summary>
    Steps
  </summary>
  {: .text-delta }
1. TOC
{:toc}
</details>

## Common 
This common steps needs to be done independent of which flow you are going to use In Path or Standalone.<br/>
On previous versions of CarTrawlerSDK everything was done using `CartrawlerSDK.Builder()`, now we've
broken down into 2 main components:

* CartrawlerSDK
* CTSdkData

We need to call `CartrawlerSDK.init` passing an implementationID and the environment you want to initialise the SDK, we recommend adding
this into your application since this won't change much;<br/>

```kotlin
val partnerImplementationID = "your-implementation-id-here"
val environment = CTSdkEnvironment.DEVELOPMENT // CTSdkEnvironment.PRODUCTION

CartrawlerSDK.init(partnerImplementationID, environment)
```

We then need to setup some data needed to start the sdk using `CTSdkData.Builder` it requires a clientId to be passed;<br/>

```kotlin
val sdkDataClientIdXYZ = CTSdkData.Builder(clientId = clientId)
    .accountId("CZ638817950")
    .country(twoLetterISOCountry = "IE")
    .currency(currency = "EUR")
    .flightNumberRequired(false)
    .logging(true)
    .orderId("123")
    .visitorId("123")
    .theme(R.style.SampleTheme)
//.<any other options you need to initialise the builder here>
```

## Start Standalone

### 13.x.x or less

```kotlin
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

---

### 14.0.0

To present the SDK we need to call `CartrawlerSDK.start` passing the following parameters:<br/>

* `activity` - this is used to start our SDK internal activity using `startActivityForResult`;<br/>
* `requestCode` - also used to start our SDK internal activity using `startActivityForResult` and later on by the consumer app in the `onActivityResult` method;<br/>
* `ctSdkData` - builder with some data to initialise the SDK like clientID, currency and country;<br/>
* `flow` - for standalone flow is Standalone();<br/>

```kotlin
CartrawlerSDK.start(
    activity = this,
    requestCode = YOUR_REQUEST_CODE_HERE,
    ctSdkData = sdkDataClientIdXYZ.build(),
    flow = CTSdkFlow.Standalone()
)
```

`CTSdkFlow.Standalone()` accepts a parameter called `navigateTo` of type `CTStandaloneNavigation` you can check <a href="/docs/android/standalone/implementation-steps/#starting-the-flow-" target="_blank">Standalone Navigation Types</a> 
for more details and learn how to change the first screen being shown in the SDK.

---

## Start In Path

### 13.x.x or less

```kotlin
CartrawlerSDK.Builder()
    .setRentalInPathClientId(clientId = "your clientID")
    .setEnvironment(environment)
    .setCurrency(currency)
    .setCountry(countryISO)
    .setFlightNumberRequired(true)
    .setPassenger(passenger)
    .setAccountId("123")
    .setLogging(true)
    .setPickupTime(getPickUpDate())
    .setPickupLocation("DUB")
    .setDropOffLocationId(11)
    .setDropOffTime(getDropOffDate())
    .setTheme(getSelectedTheme(palette))
    .startRentalInPath(activity, REQUEST_CODE_IN_PATH)
```

---

### 14.0.0

To present the SDK we need to call `CartrawlerSDK.start` passing the following parameters:<br/>

* `activity` - this is used to start our SDK internal activity using `startActivityForResult`;<br/>
* `requestCode` - also used to start our SDK internal activity using `startActivityForResult` and later on by the consumer app in the `onActivityResult` method;<br/>
* `ctSdkData` - builder with some data to initialise the SDK like clientID, currency and country;<br/>
* `flow` - for standalone flow is Standalone();<br/>

```kotlin
// updating object previously defined in the Common section
sdkDataClientIdXYZ
    .pickupDateTime(LocalDateTime.of(2023, 5, 10, 10, 0))
    .dropOffDateTime(LocalDateTime.of(2023, 5, 15, 10, 0))
    .pickupLocationIATA("DUB")

CartrawlerSDK.start(
    activity = this,
    requestCode = YOUR_REQUEST_CODE_HERE,
    ctSdkData = sdkDataClientIdXYZ.build(),
    flow = CTSdkFlow.InPath()
)
```

`CTSdkFlow.InPath()` accepts a parameter called `navigateTo` of type `CTInPathNavigation` you can check <a href="/docs/android/inpath/implementation-steps/#starting-the-flow" target="_blank">In Path Navigation Types</a>
for more details and learn how to change the first screen being shown in the SDK.
