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

<picture>
  <source media="(max-width: 799px)" srcset="/uploads/Pricing_Added_Generic_style.png">
  <source media="(min-width: 800px)" srcset="/uploads/Pricing_Added_Generic_style.png">
  <img src="/uploads/Pricing_Added_Generic_style.png">
</picture>


In order to use the CTVehicleWidget, you will need pass it Vehicle Object, that is returned following the Cartrawler flow, see example below
on how you would achieve this:

```java
override fun onActivityForResult(requestCode: Int, resultCode: Int, data: Intent?) {
       if (resultCode == Activity.RESULT_OK) {
           if (requestCode == 123) {
               	// Set the widget to the added state
               ctVehicleWidget.setVehicle(data.getParcelableExtra(CartrawlerSDK.VEHICLE_DETAILS))
              
           }
       }
    }
````