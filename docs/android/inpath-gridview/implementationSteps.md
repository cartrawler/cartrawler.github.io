---
layout: default
title: Implementation Steps
parent: In Path with Grid View
grand_parent: Android Integration
nav_order: 1
permalink: /docs/android/inpath-gridview/implementation-steps/
---

# Implementation Steps
{: .no_toc }

To implement the SDK's In Path flow within your app, please use the following steps:

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
    Notes
  </summary>
  {: .text-delta }
1. <a href="/docs/android/inpath-gridview/implementation-steps/#insurance">Insurance</a>
3. <a href="/docs/android/inpath-gridview/implementation-steps/#insurance">Totals</a>
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
The `implementationID` is needed by the SDK to fetch partner specific configuration. It's recommended to call CartrawlerSDK.init in your application class.<br/>

{: .warning }
Don't forget to use `CTSdkEnvironment.PRODUCTION` when submitting your app to the Play Store.

---

### Initialise CTSdkData <br/>

```kotlin
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
        .context("IN_PATH")
        .build()

val sdkData = CTSdkData.Builder(clientId = clientId)
    .country(twoLetterISOCountry = "IE")
    .currency(currency = "EUR")
    .flightDetails(flightDetails)
    .build()
    
    // check Property Descriptions link down below to see all available properties
```

{: .note-title }
> Optional properties
>
> The `CTSdkData` builder also has some optional properties that can be passed in during initialisation to use and/or display certain features, you can check
<a href="/docs/android/inpath-gridview/property-descriptions/" target="_blank">Property Descriptions</a> section for all properties available.

---

## Prewarm Grid View

After initialisation and setup of your `CTSdkData` object for In Path, you can pre warm the grid view. 
Prewarm mode allows the Grid View to preload all necessary API data before the user reaches the car rental offer page, resulting in significantly faster load times. 
To use prewarm, you must request a SmartBlockWidgetWebview object to the SDK, at this point the grid view won't display any UI, instead, it will fetch and cache API responses. These cached responses are then used when the user navigates to the main offer page, making the experience seamless and quick.

It is recommended to prewarm the grid view in a previous step before you reach where it will effectively be presented. Example:

```kotlin
CartrawlerSDK.getInPathGridView(
    activity = this,
    requestCode = YOUR_REQUEST_CODE_HERE,
    ctSdkData = sdkData,
    flow = CTSdkFlow.InPath(),
    preLoad = true
)
```

---

## Present Grid View
The `SmartBlockWidgetWebview` grid view is a component returned by the SDK where a number of recommended vehicles will be displayed as a grid.

When the user taps on a vehicle block, the grid view will work internally to present the SDK in a modal.

You can request a preloaded grid view or a new one. Below some implementation examples. 

Example of layout:
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout
        xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:tools="http://schemas.android.com/tools"
        android:id="@+id/widgets"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="vertical"
        tools:visibility="visible">

        <FrameLayout
            android:id="@+id/webViewContainer"
            android:layout_width="match_parent"
            android:layout_height="1000dp"
            />
</LinearLayout>
```

Request pre loaded grid view and plugin it to your own view:
```kotlin
val webview = CartrawlerSDK.getPreLoadedInPathGridView()
binding.webViewContainer.addView(webview)
```

Request a new grid view with parameters previously configured and plugin it to your own view:
```kotlin
CartrawlerSDK.getInPathGridView(
    activity = this,
    requestCode = YOUR_REQUEST_CODE_HERE,
    ctSdkData = sdkData,
    flow = CTSdkFlow.InPath()
)
binding.webViewContainer.addView(webview)
```

---

## Present Grid View with alternative flow (FBE)
The default behaviour of the grid view is `flightDetails.context = "IN_PATH"`, which means it will assume it has been presented during the flight selection, it will open the InPath flow and return the booking JSON when the user selects the vehicle.

Alternatively, you can use the grid view in the end of your flow, for example as a post booking option, after the user has paid for the flight but didn't rent a car.

On that case, the `flightDetails.context` can be set to `CONFIRMATION` and when the user taps on any vehicle, it will open a FBE flow, which is the complete flow (vehicle and insurance selection, payment and confirmation) provided by CarTrawler.

Example of request grid view with alternative flow (FBE):
```kotlin
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
        .context("CONFIRMATION") // Needed to define the flow
        .build()

val sdkData = CTSdkData.Builder(clientId = clientId)
    .country(twoLetterISOCountry = "IE")
    .currency(currency = "EUR")
    .flightDetails(flightDetails)
    .build()

CartrawlerSDK.getInPathGridView(
    activity = this,
    requestCode = YOUR_REQUEST_CODE_HERE,
    ctSdkData = sdkData,
    flow = CTSdkFlow.InPath()
)
binding.webViewContainer.addView(webview)
```

---

## Retrieval of objects from the In Path Process

If a user selected a car during the In Path process, the `onActivityForResult` will be fired. Objects can be retrieved at this point, namely the payload, the fees object and the vehicle details object

These objects are accessed via the return intent by `onActivityForResult`

```kotlin

    override fun onActivityForResult(requestCode: Int, resultCode: Int, data: Intent?) {
        if (resultCode == Activity.RESULT_OK) {
            if (requestCode == YOUR_REQUEST_CODE_HERE) {
                /*
                get json or objects returned by the SDK to your activity
                
                getStringExtra(CartrawlerSDK.PAYLOAD) // Returns a JSON String  
                getParcelableExtra(CartrawlerSDK.VEHICLE_DETAILS) // Returns a VehicleDetails Object
                getSerializableExtra(CartrawlerSDK.FEES) // Returns a Payment Object
                getParcelableExtra(CartrawlerSDK.TRIP_DETAILS) // Returns a Trip Object with extras included
                 */
            }
        }
    }  
```    
    
The JSON payload object is returned so that the partner can process the payment/reservation with a CarTrawler payment end point at a different time and put it in the partners basket flow. This JSON payload object is passed to this endpoint. 
<!-- Further details can be found in our OTA developer docs. (Also see In Path reservation section) -->
    
The ``CartrawlerSDK.TRIP_DETAILS`` object:

```kotlin
data class TripDetails(
   val pickUpDateTime: String? = null,
   val returnDateTime: String? = null,
   val pickupLocation: LocationDetails? = null,
   val returnLocation: LocationDetails? = null
) 
```
          
The total amount to be authorized against the customers credit card, is the authTotal attribute above. This is calculated by CarTrawler using pay now, insurance, and booking fee amounts when applicable.

---
 
## Notes on other attributes:
{: .no_toc}

### Insurance
{: .no_toc}

* If insurance is selected, `insuranceAmount` will be non-zero.
* The `insuranceName` will be non-null when `insuranceAmount` is non-zero.
* The `insuranceName` is the title of the insurance.

### Totals
{: .no_toc}

* `payNowAmount` is the total amount for only the car element (excludes extras,  booking fee and insurance)
* `bookingFeeAmount` is a CarTrawler fee which is separate to the `payNowAmount`.
* The authTotal is calculated by the CarTrawler system by adding the `payNowAmount`, `bookingFeeAmount` (if present) and `insuranceFee` (if present).