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

The Vehicle Widget provides a UI for displaying a CTVehicleDetails object. It can be used after a user has completed the In Path flow. 

---

![](/uploads/Pricing_Added_Generic_iOS.png)

```java
let widgetStyle = CTWidgetStyle()
let widgetContainer = CarTrawlerSDK.sharedInstance().getWidget(status: .vehicle,
                                                                   style: widgetStyle,
                                                                   delegate: self)
self.stackWidgetView.insertArrangedSubview(widgetContainer, at: 0)
```

## Setting the CTVehicleWidget vehicle

When a user has completed the CarTrawler In Path flow and added a vehicle, the vehicle will be returned to you in the `vehicleSelected` delegate callback. Using the code below, you could switch a best price widget into a populated vehicle widget: 

```java  
func vehicleSelected(_ vehicle: CTVehicleDetails) {
    self.widgetContainer?.setVehicle(vehicle)
    self.widgetContainer?.setStatus(.vehicle)
}
```