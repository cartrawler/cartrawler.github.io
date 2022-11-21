---
layout: default
title: Vehicle
parent: Android SDK Widgets
grand_parent: Android
nav_order: 4
permalink: /docs/android/widgets/vehicle/
---

# Vehicle Widget

{: .no_toc }

---

![](/uploads/Pricing_Added_Generic_style.png)

In order to use the `CTVehicleWidget`, you will need pass in a Vehicle Object.

```java
override fun onActivityForResult(requestCode: Int, resultCode: Int, data: Intent?) {
    if (resultCode == Activity.RESULT_OK) {
        if (requestCode == 123) {
            // Get Vehicle Details
            val vehicleDetails = data.getParcelableExtra(CartrawlerSDK.VEHICLE_DETAILS)
            //Transform it to View Object
            val vehicleDetailsVO = VehicleDetailsFromSession(vehicleDetails)
            // Set the widget to the added state
            ctVehicleWidget.setVehicle(vehicleDetailsVO)
        }
    }
}
````