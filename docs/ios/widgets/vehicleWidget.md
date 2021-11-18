---
layout: default
title: Vehicle
parent: iOS SDK Widgets
grand_parent: iOS
nav_order: 4
permalink: /docs/ios/widgets/vehicle
---

# Vehicle Widget

{: .no_toc }

---

<picture>
  <source media="(max-width: 799px)" srcset="/uploads/Pricing_Added_Generic_iOS.png">
  <source media="(min-width: 800px)" srcset="/uploads/Pricing_Added_Generic_iOS.png">
  <img src="/uploads/Pricing_Added_Generic_IOS.png_iOS">
</picture>

## Setting the CTVehicleWidget vehicle

When a user has completed the CarTrawler InPath flow and added a vehicle, the vehicle will be returned to you in the vehicleSelected delegate callback. Using the code below, you could switch a best price widget into a populated vehicle widget: 

```java  
    func vehicleSelected(_ vehicle: CTVehicleDetails) {
        self.widgetContainer?.setVehicle(vehicle)
        self.widgetContainer?.setStatus(.vehicle)
    }
```