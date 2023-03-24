---
layout: default
title: Implementation Steps
parent: In Path
grand_parent: Android Integration
nav_order: 1
permalink: /docs/android/inpath/implementation-steps/
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
1. <a href="/docs/android/inpath/implementation-steps/#insurance">Insurance</a>
2. <a href="/docs/android/inpath/implementation-steps/#insurance">Extras</a>
3. <a href="/docs/android/inpath/implementation-steps/#insurance">Totals</a>
</details>

---

### Initialise the SDK <br/>

First we need to initialise the SDK by calling the init method in the CartrawlerSDK class as follow:

```kotlin
val partnerImplementationID = "your-implementation-id-here"
val environment = CTSdkEnvironment.DEVELOPMENT // CTSdkEnvironment.PRODUCTION

CartrawlerSDK.init(partnerImplementationID, environment)
```

{: .important }
The `implementationID` is needed by the SDK since it's used to fetch some configuration, it's good to call `CartrawlerSDK.init` in your application class;<br/>
Don't forget to use `CTSdkEnvironment.PRODUCTION` when submitting your app to the Play Store;

---

### Initialise the CTSdkData <br/>

```kotlin
// CTSdkPassenger is optional
val optionalPassenger = CTSdkPassenger(
    firstName = "John",
    lastName = "Smith",
    email = "john@example.com",
    phoneCountryCode = "353",
    phoneNumber = "81234567",
    address = "Dundrum Business Park",
    city = "Dublin",
    postcode = "D14 R7V2",
    country = "IE",
    flightNumber = "EZY130",
    age = "26", //Default age is 30
    membershipId = "123456"
)

val sdkDataClientIdXYZ = CTSdkData.Builder(clientId = clientId)
    .country(twoLetterISOCountry = "IE")
    .currency(currency = "EUR")
    .logging(isLogEnabled = true) // disable/remove this when publishing to Play Store
    .pickupDateTime(LocalDateTime.of(2023, 5, 10, 10, 0))
    .dropOffDateTime(LocalDateTime.of(2023, 5, 15, 10, 0))
    .pickupLocationIATA("DUB")
    .passenger(optionalPassenger) // you can omit this, it's optional 
    //.<any other options you need to initialise the builder here>
```

{: .note-title }
> Optional properties
>
> The `CTSdkData` builder also has some optional properties that can be passed in during initialisation to use and/or display certain features, you can check
<a href="/docs/android/inpath/property-descriptions/" target="_blank">Property Descriptions</a> section for all properties available.

---

### Starting the flow<br/>

```kotlin
CartrawlerSDK.start(
    activity = this,
    requestCode = YOUR_REQUEST_CODE_HERE,
    ctSdkData = sdkDataClientIdXYZ.build(),
    flow = CTSdkFlow.InPath()
)
```

{: .note-title }
> In Path Navigation types
>
> InPath flow accepts a parameter `navigateTo` of type `CTInPathNavigation`. If you're using Kotlin you can omit it since its default value is `CTInPathNavigation.CTNavigateToAvailability`.
>
> The following types are allowed:<br/>
>
> `CTInPathNavigation.CTNavigateToAvailability`
> `CTInPathNavigation.CTNavigateToAvailabilityWithPinnedVehicle("<vehicle_ref_if_here>")`

---

### Start with a vehicle pinned to the top of the list<br/>

{: .important }
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
    flow = Standalone(navigateTo = 
        CTInPathNavigation.CTNavigateToAvailabilityWithPinnedVehicle(
            vehicleRefId = "vehicle_ref_if_here"
        )
    )
)
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
   val returnLocation: LocationDetails? = null,
   val vehicleCharges: List<VehicleCharge>, // The list of Vehicle Charges
   val extras: List<Extra>, // The list of extras
   val isPrePaidExtra: Boolean? = null //Determines if the extra requires payment
   val loyalty: Loyalty? = null
) 

data class Extra (
   var amount: Double? = null,
   var currencyCode: String? = null,
   var name: String? = null,
   var description: String? = null,
   var type: String? = null, // the CT Code we use
   var selected: Int? = null, // the number of selected extras or qty
   var includedInRate: Boolean? = null // Is this an extra selected by the user or already part of rate
)

 class VehicleCharge (
   var chargeDescription: String? = null, // The localized description
   var taxInclusive: String? = null, // Is tax included?
   var includedInRate: String? = null, // In this In Path payload use case this is always 'true'
   var purpose: String? = null, // Internal Purpose code
   var calculation: String? = null, // Reserved as "BeforePickup"
   var amount: Double? = null, // The amount of charge
   var currencyCode: String? = null // The currencyCode of the charge
)

class Loyalty(
   val programID: String? = null,
   val points: Int? = 0
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

### Extras
{: .no_toc}

* If the user has selected extras, the total amount of extra fees will set in `payAtDeskAmount`. <br>

Some extras are free. 
We expose a free extra with the `IncludedInRate` set to true. Here is an example of a free additional driver:

```java  
data class Extra (
   var amount: Double? = 20.00,
   var currencyCode: String? = "EUR",
   var name: String? = "Additional Driver",
   var description: String? = "Additional Driver",
   var type: String? = "101.EQP",
   var selected: Int? = 1, 
   var includedInRate: Boolean? = true
)
```

To display this as a free additional driver in your UI, you can simply check that `includedInRate` is true. 

### Totals
{: .no_toc}

* `payNowAmount` is the total amount for only the car element (excludes extras,  booking fee and insurance)
* `bookingFeeAmount` is a CarTrawler fee which is separate to the `payNowAmount`.
* The authTotal is calculated by the CarTrawler system by adding the `payNowAmount`, `bookingFeeAmount` (if present) and `insuranceFee` (if present).