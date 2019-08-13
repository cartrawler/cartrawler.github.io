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
                     .setLoyalty(loyaltyProgramId = "LoyaltyId", membershipNumber = "123")
                     .setOrderId(orderId = "123")
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
<dt>setCurrency</dt><dd>An optional currency code, such as "USD". Default is "EUR" if not provided.</dd>
<dt>setEnvironment</dt><dd>Switch between CarTrawlers endpoints for STAGING and PRODUCTION environments.</dd>
<dt>setFlightNumberRe...</dt><dd>A boolean key to enable Flight Number as a required field in the Payment Form.</dd>
<dt>setLogging</dt><dd>Boolean value for additional logging while debugging.</dd>
<dt>setLoyalty</dt><dd>loyaltyProgramId: A String value that represents the Loyalty Program ID , Example: "HAWAIIAN_MILES". membershipNumber: A String value that is used to pre-populate the loyalty field.</dd>
<dt>setOrderId</dt><dd>A String value that represents the Order ID</dd>
<dt>setPassenger</dt><dd>An optional Array of Passengers, the first one will be the main passenger.</dd>
<dt>setPickupLocation</dt><dd>An optional IATA code.</dd>
<dt>setPickupTime</dt><dd>Pick up date time for required service.</dd>
<dt>setVisitorId</dt><dd>A String value that represents the Visitor ID.</dd>
<dt>startRentalInPath</dt><dd>Start Rental InPath activity.</dd></dl>


<h5>Retrieval of objects from Inpath Process</h5>


If a user selected a car during the in path process, the onActivityForResult will be fired. Objects can be retrieved at this point, namely the payload, the fees object and the vehicle details object

These objects are accessed via the return intent by onActivityForResult
    
    getStringExtra(CartrawlerSDK.PAYLOAD) // Returns a JSON String
    
    getParcelableExtra(CartrawlerSDK.VEHICLE_DETAILS) // Returns a VehicleDetails Object
    
    getSerializableExtra(CartrawlerSDK.FEES) // Returns a Payment Object
        
        
    override fun onActivityForResult(requestCode: Int, resultCode: Int, data: Intent?) {
       if (resultCode == Activity.RESULT_OK) {
           if (requestCode == 123) {
                openCreditCardProcessor(data!!.getStringExtra(CartrawlerSDK.PAYLOAD))
            }      
    }
    
    
The json payload object is returned so that the partner can process the payment/reservation with a cartrawler payment end point at a different time and point in the partners basket flow. This JSON playload object is passed to this endpoint. 
Further details can be found in our OTA developer docs. (Also see inpath reservation section)

The CartrawlerSDK.FEES object:

    cartrawler.core.data.external.Payment {
          
          public String currency; // The currency of the fees

          public Double payNowAmount; // The pay now amount
          public Double payAtDeskAmount; // The pay at desk amount
          public Double bookingFeeAmount; // The booking fee amount
      
          public String insuranceName; // The insurance name
          public Double insuranceAmount; The insurance amount
          
          public String authTotal; The total amount to be authorized against the customers credit card.
     }
     
     
     
     
The total amount to be authorized against the customers credit card, is the authTotal attribute above. This is calcuated by cartrawler using paynow, insurance, and bookingfee amounts when applicable.
 
**Notes on other attributes:**

*Insurance*
* If insurance is selected, insuranceAmount will be non-zero.
* The insuranceName will non-null when insuranceAmount is non-zero.
* The insuranceName is the title of the insurance.

*Extras*
* If the user has selected extras, the total amount of extra fees will set in payAtDeskAmount. <br>

*Totals*
* payNowAmount is the total amount for only the car element (excludes extras,  bookingfee and insurance)
* bookingFeeAmount is a cartrawler fee which is separate to the payNowAmount.
* The authTotal is calculated by the cartrawler system by adding the payNowAmount, bookingFeeAmount (if present) and insuranceFee (if present).
