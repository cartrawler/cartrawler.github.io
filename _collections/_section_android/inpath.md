---
title: Inpath
position: 3
type: Androidimage = "https://ota-cars.imgix.net/otaimages/renault/clio_3dr_nologo.jpg?w=180&dpr=2",
description:
right_code: >
  
  ~~~java      

     //Passenger
            new CartrawlerSDKPassenger(
              "firstname",
                             "lastName",
                             "email",
                             "phoneCountryCode",
                             "phoneNumber",
                             "Address",
                             "City",
                             "Postcode",
                             "Country",
                             "Flight Number",
                             "Age");

     //InPath
         try {
           CartrawlerSDK.Builder()
                     .setRentalInPathClientId(clientId = "1234")
                     .setAccountId(accountId = "123")
                     .setCountry(twoLetterISOCountry = "IE")
                     .setCurrency(currency = "EUR")
                     .setDropOffTime(dropOffDateTime = GregorianCalendar())
                     .setEnvironment(environment = CartrawlerSDK.Environment.STAGING)
                     .setFlightNumberRequired(required = true)
                     .setLogging(logging = true)
                     .setOrderId(orderId = "IE1234")
                     .setPassenger(ctPassenger = cartrawlerSDKPassenger)
                     .setPickupLocation(iataAirportCode = "YXJ")
                     .setPickupTime(pickupDateTime = GregorianCalendar())
                     .setVisitorId(visitorId = "123")
                     .startRentalInPath(activity = this, requestCode = 123)
  ~~~

  {: title="Inpath" }
  

---

Usage of the SDK is demonstrated to the right, the parameters are as follows:

<h5>Inpath</h5>

<dl><dt>CartrawlerSDKPass...</dt><dd>An optional Array of Passengers, the first one will be the main passenger.</dd>
<dt>setRentalInPathCl...</dt><dd>Your client ID, required to use the CarTrawler API.</dd>
<dt>setAccountId</dt><dd>A String value that represents the Account ID.</dd>
<dt>setCountry</dt><dd>An optional country code to used switch between languages</dd>
<dt>setCurrency</dt><dd>An optional currency code, based on the ISO standard currency codes e.g "USD". The currency associated with the device’s system region is used by default.</dd>
<dt>setEnvironment</dt><dd>Switch between CarTrawlers endpoints for STAGING and PRODUCTION environments.</dd>
<dt>setFlightNumberRe...</dt><dd>A boolean key to enable Flight Number as a required field in the Payment Form.</dd>
<dt>setLogging</dt><dd>Boolean value for additional logging while debugging.</dd>
<dt>setOrderId</dt><dd>A String value that represents the Order ID for a Flight PNR or Booking Reference, Example: IE1234</dd>
<dt>setPassenger</dt><dd>An optional Array of Passengers, the first one will be the main passenger.</dd>
<dt>setPickupLocation</dt><dd>An optional IATA code.</dd>
<dt>setPickupTime</dt><dd>Pick up date time for required service.</dd>
<dt>setVisitorId</dt><dd>A String value that represents the Visitor ID.</dd>
<dt>startRentalInPath</dt><dd>Start Rental InPath activity.</dd></dl>


<h5>Retrieval of objects from Inpath Process</h5>


If a user selected a car during the in path process, the onActivityForResult will be fired. Objects can be retrieved at this point, namely the payload, the fees object and the vehicle details object

These objects are accessed via the return intent by onActivityForResult

```kotlin   
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
```    
    
The json payload object is returned so that the partner can process the payment/reservation with a cartrawler payment end point at a different time and point in the partners basket flow. This JSON playload object is passed to this endpoint. 
Further details can be found in our OTA developer docs. (Also see inpath reservation section)

The CartrawlerSDK.FEES object:
```kotlin
    cartrawler.core.data.external.Payment {
          
          public String currency; // The currency of the fees

          public Double payNowAmount; // The pay now amount
          public Double payAtDeskAmount; // The pay at desk amount
          public Double bookingFeeAmount; // The booking fee amount
      
          public String insuranceName; // The insurance name
          public Double insuranceAmount; //The insurance amount
          
          public String authTotal; //The total amount to be authorized against the customers credit card.
          pubic  String authCurrency; // The currency code of the authTotal
     }
```
    
The CartrawlerSDK.TRIP_DETAILS object:
```kotlin
    @Parcelize
    data class TripDetails(
            var pickUpDateTime: String? = null,
            var returnDateTime: String? = null,
            var pickupLocation: @RawValue LocationDetails? = null,
            var returnLocation: @RawValue LocationDetails? = null,
            var vehicleCharges: List<VehicleCharge>, // The list of Vehicle Charges
            var extras: List<Extra>, // The list of extras
            var isPrePaidExtra: Boolean? = null //Determines if the extra requires payment
            ): Parceable 
            
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
        var includedInRate: String? = null, // In this inpath payload use case this is always 'true'
        var purpose: String? = null, // Internal Purpose code
        var calculation: String? = null, // Reserved as "BeforePickup"
        var amount: Double? = null, // The amount of charge
        var currencyCode: String? = null // The currencyCode of the charge
    )  : Parcelable
```
          
     
The total amount to be authorized against the customers credit card, is the authTotal attribute above. This is calcuated by cartrawler using paynow, insurance, and bookingfee amounts when applicable.
 
**Notes on other attributes:**

*Insurance*
* If insurance is selected, insuranceAmount will be non-zero.
* The insuranceName will non-null when insuranceAmount is non-zero.
* The insuranceName is the title of the insurance.

*Extras*
* If the user has selected extras, the total amount of extra fees will set in payAtDeskAmount. <br>

*Extract Free Extras*

We expose a free extra with the IncludedInRate set to true. In order to display this in the UI you need to do the following logic:

A free additional driver

```kotlin  
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

To display this as a free additional driver in your UI, you can simple check the IncludedInRate returns true. 

There will be an improvement to extract free extras and standard extras in future releases.


*Totals*
* payNowAmount is the total amount for only the car element (excludes extras,  bookingfee and insurance)
* bookingFeeAmount is a cartrawler fee which is separate to the payNowAmount.
* The authTotal is calculated by the cartrawler system by adding the payNowAmount, bookingFeeAmount (if present) and insuranceFee (if present).
