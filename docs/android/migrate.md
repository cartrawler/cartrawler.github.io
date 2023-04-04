---
layout: default
title: Migrate to 14.0.2
parent: Android Integration
nav_order: 5
permalink: /docs/android/migrate/
---

# Migrate from 13.2 and below to 14.0.2
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
this into your Application class since it won't change often.<br/>

```kotlin
val partnerImplementationID = "your-implementation-id-here"
val environment = CTSdkEnvironment.DEVELOPMENT // CTSdkEnvironment.PRODUCTION

CartrawlerSDK.init(partnerImplementationID, environment)
```

{: .warning }
Don't forget to use `CTSdkEnvironment.PRODUCTION` when submitting your app to the Play Store.

We then need to setup some data needed to start the sdk using `CTSdkData.Builder` it requires a clientId to be passed.<br/>

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
    // check Property Descriptions link down below to see all available properties
```

{: .note-title }
> Optional properties
>
> The `CTSdkData` builder also has some optional properties that can be passed in during initialisation to use and/or display certain features, you can check the following for all properties available:
> 
> <a href="/docs/android/standalone/property-descriptions/" target="_blank">Property Descriptions (Standalone)</a>
> 
> <a href="/docs/android/inpath/property-descriptions/" target="_blank">Property Descriptions (In Path)</a>

---

## Start Standalone

On SDK 13.2 and below we used to initialise some properties using `CartrawlerSDK.Builder()` and then call one of its start methods like `startRentalStandalone` to
start the SDK flow and present its screen.
Now to present the SDK we only need to call `CartrawlerSDK.start` passing the following parameters:<br/>
> {: .new }
> > <b>activity</b><br/>Used to start our SDK's internal activity using `startActivityForResult`;<br/><br/>
> > <b>requestCode</b><br/>Used to start our SDK's internal activity using `startActivityForResult` and later on by the consumer app in the `onActivityResult` method;<br/><br/>
> > <b>ctSdkData</b><br/>Builder with some data to initialise the SDK like clientID, currency and country;<br/><br/>
> > <b>flow</b><br/>For standalone flow is CTSdkFlow.Standalone();

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

On SDK 13.2 and below we used to initialise some properties using `CartrawlerSDK.Builder()` and then call one of its start methods like `startRentalInPath` to
start the SDK flow and present its screen.
Now to present the SDK we only need to call `CartrawlerSDK.start` passing the following parameters:<br/>
> {: .new }
> > <b>activity</b><br/>Used to start our SDK's internal activity using `startActivityForResult`;<br/><br/>
> > <b>requestCode</b><br/>Used to start our SDK's internal activity using `startActivityForResult` and later on by the consumer app in the `onActivityResult` method;<br/><br/>
> > <b>ctSdkData</b><br/>Builder with some data to initialise the SDK like clientID, currency and country;<br/><br/>
> > <b>flow</b><br/>For In Path flow is CTSdkFlow.InPath();

---

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
