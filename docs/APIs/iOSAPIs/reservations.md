---
layout: default
title: Booking Reservations
parent: iOS SDK APIs
grand_parent: APIs
nav_order: 6
permalink: /docs/api/ios/reservations
---

# Booking Reservations

{: .no_toc }

Car rental booking reservations can be fetched using the `requestReservationDetails` function. 

---

Calling the `requestReservationDetails` function will trigger a vehicles request based on a `resID` and email (hashed) or `resUid`. This will return a `CTReservationDetails` object, or an error if the reservation does not exist. 

```java
import CarTrawlerSDK
  
// This will trigger a booking reservation fetch.
// The SDK must be initialised, and a CTAPIQueryParams object with the necessary parameters 
// must be set before calling this method.

let params = CTAPIQueryParams()  
params.delegate = self
params.clientID = "your clientID"
params.languageCode = "EN" // The language associated with the device’s system region is used by default.
params.resId = "IE3453453"
params.resUid = "12424345446466464"

CarTrawlerSDK.sharedInstance.requestReservationDetails(params { (reservation, error) in
    if let reservation = reservation {
        // reservation fetched successfully
    } else if let error = error {
        // error occured
    }
}
```

`CTReservationDetails` objects are also returned upon successful completion of the Standalone flow: 
```java
didReceive(_ reservationDetails: CTReservationDetails)
```