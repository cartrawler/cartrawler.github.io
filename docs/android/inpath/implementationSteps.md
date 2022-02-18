---
layout: default
title: Implementation Steps
parent: In Path
grand_parent: Android
nav_order: 1
permalink: /docs/android/inpath/implementation-steps/
---

# Implementation Steps

{: .no_toc }

To implement the SDK's In Path flow within your app, please use the following steps:

---

### Initialise the In Path flow with the SDK Builder <br/>
Please make sure to set the following properties: 

```java
CartrawlerSDK.Builder()
           .setRentalInPathClientId(clientId(activity, palette))
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
           
//Setup your lead passenger object
val passenger = CartrawlerSDKPassenger(
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
             membershipId = "123456") 
```

#### Optional Builder Properties
The SDK has some optional properties that can be passed in initialisation to use and/or display certain features

```java
       .enableCustomCashTreatment()
       .setUSPDisplayType(USPDisplayType.CHECK_STYLE)
       .addPromotionCode(PromotionCodeType.WithCodeType("your-promotion-code-here")
```  

---

### Retrieval of objects from the In Path Process

If a user selected a car during the In Path process, the `onActivityForResult` will be fired. Objects can be retrieved at this point, namely the payload, the fees object and the vehicle details object

These objects are accessed via the return intent by `onActivityForResult`

```java   
getStringExtra(CartrawlerSDK.PAYLOAD) // Returns a JSON String
    
getParcelableExtra(CartrawlerSDK.VEHICLE_DETAILS) // Returns a VehicleDetails Object
    
getSerializableExtra(CartrawlerSDK.FEES) // Returns a Payment Object
    
getParcelableExtra(CartrawlerSDK.TRIP_DETAILS) // Returns a Trip Object with extras included
        
        
override fun onActivityForResult(requestCode: Int, resultCode: Int, data: Intent?) {
   if (resultCode == Activity.RESULT_OK) {
      if (requestCode == 123) {
         openCreditCardProcessor(data!!.getStringExtra(CartrawlerSDK.PAYLOAD))
      }
   }      
}
```    
    
The JSON payload object is returned so that the partner can process the payment/reservation with a CarTrawler payment end point at a different time and point in the partners basket flow. This JSON payload object is passed to this endpoint. 
Further details can be found in our OTA developer docs. (Also see In Path reservation section)
    
The ``CartrawlerSDK.TRIP_DETAILS`` object:

```java
@Parcelize
data class TripDetails(
   val pickUpDateTime: String? = null,
   val returnDateTime: String? = null,
   val pickupLocation: @RawValue LocationDetails? = null,
   val returnLocation: @RawValue LocationDetails? = null,
   val vehicleCharges: List<VehicleCharge>, // The list of Vehicle Charges
   val extras: List<Extra>, // The list of extras
   val isPrePaidExtra: Boolean? = null //Determines if the extra requires payment
   val loyalty: Loyalty? = null): Parceable 
        
@Parcelize
data class Extra (
   var amount: Double? = null,
   var currencyCode: String? = null,
   var name: String? = null,
   var description: String? = null,
   var type: String? = null, // the CT Code we use
   var selected: Int? = null, // the number of selected extras or qty
   var includedInRate: Boolean? = null // Is this an extra selected by the user or already part of rate
): Parcelable

@Parcelize
 class VehicleCharge (
   var chargeDescription: String? = null, // The localized description
   var taxInclusive: String? = null, // Is tax included?
   var includedInRate: String? = null, // In this In Path payload use case this is always 'true'
   var purpose: String? = null, // Internal Purpose code
   var calculation: String? = null, // Reserved as "BeforePickup"
   var amount: Double? = null, // The amount of charge
   var currencyCode: String? = null // The currencyCode of the charge
)  : Parcelable
@Parcelize
class Loyalty(
   val programID: String? = null,
   val points: Int? = 0
)  : Parcelable
```
          
The total amount to be authorized against the customers credit card, is the authTotal attribute above. This is calculated by CarTrawler using pay now, insurance, and booking fee amounts when applicable.
 
### Notes on other attributes:

##### Insurance
* If insurance is selected, `insuranceAmount` will be non-zero.
* The `insuranceName` will non-null when `insuranceAmount` is non-zero.
* The `insuranceName` is the title of the insurance.

##### Extras
* If the user has selected extras, the total amount of extra fees will set in `payAtDeskAmount`. <br>

##### Extract Free Extras

We expose a free extra with the `IncludedInRate` set to true. In order to display this in the UI you need to do the following logic:

A free additional driver

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

To display this as a free additional driver in your UI, you can simple check the `IncludedInRate` returns true. 

There will be an improvement to extract free extras and standard extras in future releases.


##### Totals
* `payNowAmount` is the total amount for only the car element (excludes extras,  booking fee and insurance)
* `bookingFeeAmount` is a CarTrawler fee which is separate to the `payNowAmount`.
* The authTotal is calculated by the CarTrawler system by adding the `payNowAmount`, `bookingFeeAmount` (if present) and `insuranceFee` (if present).