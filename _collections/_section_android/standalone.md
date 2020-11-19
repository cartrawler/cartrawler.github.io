---
title: Standalone
position: 2
type: Android
description:
right_code: >
---

<h5>To initialise the SDK with standalone flow you need to implement the SDK builder as follows:</h5>

  ```java
 CartrawlerSDK.Builder()
            .setRentalStandAloneClientId(clientId = "12345") // Ask your partner manager for your client id
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

<h5>To support a deeplink to the availability screen you need to add pinned veh ref along with the drop off time, pick up time and the pickup and drop off locations as follows:</h5>

 ```java
 CartrawlerSDK.Builder()
            .setPickupTime(pickupDateTime = GregorianCalendar())
            .setDropOffTime(dropOffDateTime = GregorianCalendar()
            .setPickupLocation(iataAirportCode = "YXJ")
            //.. Add your other config properties as normal
  ```

<h5>Standalone builder properties descriptions</h5>

<dl>
<dt>requestCode</dt><dd>Partner provided unique integer value used to send back reservation details from Systems API.</dd>
<dt>setClientId</dt><dd>Your client ID, required to use the CarTrawler API.</dd>
<dt>setAccountId</dt><dd>A String value that represents the Account ID.</dd>
<dt>setCountry</dt><dd>An optional country code to used switch between languages</dd>
<dt>setCurrency</dt><dd>An optional currency code, based on the ISO standard currency codes e.g "USD". The currency associated with the device’s system region is used by default.</dd>
<dt>setEnvironment</dt><dd>Switch between CarTrawlers endpoints for STAGING and PRODUCTION environments.</dd>
<dt>setFlightNumberRe...</dt><dd>A boolean key to enable Flight Number as a required field in the Payment Form.</dd>
<dt>setLogging</dt><dd>Boolean value for additional logging while debugging.</dd>
<dt>setOrderId</dt><dd>A String value that represents the Order ID for a Flight PNR or Booking Reference, Example: IE1234</dd>
<dt>setPassenger</dt><dd>An optional Array of Passengers, the first one will be the main passenger.</dd>
<dt>setVisitorId</dt><dd>A String value that represents the Visitor ID.</dd>
<dt>startRentalStandalone</dt><dd>Start Rental standalone activity.</dd></dl>


<h5>Reservation Retrieval from Standalone Process</h5>

If a user booked a car during the standalone process, the onActivityForResult will be fired.
The reservation object is accessed via the return intent by onActivityForResult:

```kotlin
returnIntent.getStringExtra(CartrawlerSDK.RESERVATION)
    
    override fun onActivityForResult(requestCode: Int, resultCode: Int, data: Intent?) {
       if (resultCode == Activity.RESULT_OK) {
           if (requestCode == 123) {
                yourMethod(data!!.getStringExtra(CartrawlerSDK.RESERVATION))
            }      
    }
    
    // The Reservation Object is defined as the following
    
    data class ReservationDetails (
       // In this scernario it will be confirmed
       val status: String,
       // first name
       val givenName: String, 
       // Surname
       val surname: String, 
       // Reservation ID
       val resId: String, 
       // resuid, use this along with the resId to retrieve the booking later
       val resuid: String, 
       //The date & time of pickup
       val pickUpDateTime: GregorianCalendar, 
       //The date & time of pickup 
       val returnDateTime: GregorianCalendar,  
       //Location details of pickup
       val pickupLocation: LocationDetails, 
       //Location details of pickup
       val returnLocation: LocationDetails, 
       // Insurance, null if none attached
       val insurance: Insurance, 
       // Information on reservation costs
       val rentalInfo: RentalInfo, 
       //Information on selected vehicle
       val vehicle: VehicleDetails,
       //Loyalty membership id and program name
       var custLoyalty: CustLoyalty? = null) 
      
    data class LocationDetails (
       // Determines if the location is at airport
       val atAirport: Boolean, 
       // IATA Code
       val iataCode: String,  
       // Unique Location Code (code type is internal to Cartrawler)
       val code: Int,  
       // Text description of location
       val name: String, 
       // Postal address of location
       val address: Address, 
       // Vendor contact number
       val phoneNumber: String)
    
    data class Insurance (
       // Insurance company name
       val company: String, 
       // Code of offered insurance product
       val id: String, 
       // base cost
       val cost: Double, 
       // base currency
       val currency: String, 
       // date insurance was purchased
       val createDate: String, 
       // Cost converted into charged currency (presented currency)
       val costCharge: Double, 
       // the presented currency to the customer
       val currencyCharge: String,
       // a link to the company logo
       val companyLogo: String, 
       // a link to the policy terms and conditions
       val companyPolicyURL: String, 
       // A marketing description of the insurance (markup)
       val text: String)
    
    data class RentalInfo (
       // base cost
       val cost: Double, 
       // currency (base currency is EUR)
       val currency: String, 
       // cost in the currency of the customer
       val customerCost: Double, 
       // the presented currency to the customer
       val customerCurrency: String )
    
    data class Address (
       // Post adddress of location
       val addressLine: String, 
       // 2 letter country code.
       val countryNameCode: String)

       
    @Parcelize
    data class VehicleDetails(
       val referenceId: String,
       val name: String,
       val orSimilar: String,
       val code: String,
       val vehicleAssetNumber: String,
       val pictureURL: String,
       val passengerQuantity: Int,
       val doorCount: Int?,
       val baggageQuantity: Int
       val fuelType: String,
       val driveType: Stringl,
       val airConditionInd: Boolean,
       val transmissionType: String,
       val size: String,
       val supplier: String,
       val supplierRating: Double,
       val supplierImageURL: String,
       val passengersText: String,
       val baggageText: String?,
       val doorsCountText: String,
       val transmissionText: String,
       val categoryText: String,
       val sizeText: String
       val price: Double,
       val pricePerDay: Double,
       val currencyCode: String) : Parcelable

   @Parcelize
   data class CustLoyalty(
       val membershipID: String? = null, 
       val programID: String? = null,
       val pointsEarned: String? = null): Parcelable
    }
```    
    
<h5>Present Standalone with Deeplinking</h5>

This is a variant on the standalone flow whereby the vehicle list is shown based on the pickup and dropoff parameters, rather than the regular initial search screen.
Optionally, if a vehicle refId is provided, this will be become the pinned item in the list.
If a user backs out of the list, it will return the user to the Cartrawler search.

- If the pickup and drop off dates are invalid, out of date, or not present the SDK will fallback to regular standalone search.
- If the vehicle refId is invalid (or out of date), the list will be shown without the vehicle being pinned.
- If the parameters are valid but no search results are returned by the CarTrawler system, the SDK will fallback to the regular standalone search.

setPinnedVehicle call is optional, as ths is on dependent on whether the vehicle is known (by Partner App), if know this becomes the pinned item in the list.

setDropOffTime, setPickupLocation (or setDropOffLocationID/setPickupLocationId), setPickupTime, are required so that the list is displayed (rather then the search).
