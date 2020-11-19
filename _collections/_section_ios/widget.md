---
title: Widgets
position: 5
type: iOS
description:
right_code: >-

---


The CarTrawler iOS SDK comes prepackaged with four widgets.<br /> When considering the addition of a new touchpoint into the CarTrawler flow from within your app, you can choose the one that best suits your needs.<br /> Each widget is independent of the Standalone and inPath flows, and can be used to launch either of them via your implementation of the CTWidgetContainerDelegate’s didTapView function.<br />

<h4>CTSimpleWidget</h4>
<picture>
  <source media="(max-width: 799px)" srcset="/uploads/Simple_Loaded_Generic_iOS.png">
  <source media="(min-width: 800px)" srcset="/uploads/Simple_Loaded_Generic_iOS.png">
  <img src="/uploads/Simple_Loaded_Generic_iOS.png">
</picture><br />

<h4>CTSimpleAddedWidget</h4>
<picture>
  <source media="(max-width: 799px)" srcset="/uploads/Simple_Added_Generic_iOS.png">
  <source media="(min-width: 800px)" srcset="/uploads/Simple_Added_Generic_iOS.png">
  <img src="/uploads/Simple_Added_Generic_iOS.png">
</picture><br />

<h4>CTBestPriceWidget</h4>
<picture>
  <source media="(max-width: 799px)" srcset="/uploads/Pricing_Loaded_Generic_iOS.png">
  <source media="(min-width: 800px)" srcset="/uploads/Pricing_Loaded_Generic_iOS.png">
  <img src="/uploads/Pricing_Loaded_Generic_iOS.png_iOS">
</picture><br />

<h4>CTVehicleWidget</h4>
<picture>
  <source media="(max-width: 799px)" srcset="/uploads/Pricing_Added_Generic_iOS.png">
  <source media="(min-width: 800px)" srcset="/uploads/Pricing_Added_Generic_iOS.png">
  <img src="/uploads/Pricing_Added_Generic_IOS.png_iOS">
</picture><br />

***Setup and Usage***

<h5>UI</h5>

To implement the widgets, you must first create an instance of CTWidgetStyle and set the relevant properties, as shown in the above diagrams. <br /><br />
The next step is to create an instance of CTWidgetContainer and pass in the following: status (.simple, .simpleAdded, .bestPrice, or .vehicle), style (your CTWidgetStyle object), and a delegate. 
We recommend adding the widget container to a stackView, like so: 

```swift
    let widgetStyle = CTWidgetStyle()
    let widgetContainer = CarTrawlerSDK.sharedInstance().getWidget(status: .simple,
                                                                       style: widgetStyle,
                                                                       delegate: self)
    self.stackWidgetView.insertArrangedSubview(widgetContainer, at: 0)
```

Your widget’s status can be changed at any time by calling setStatus on the container:

    self.widgetContainer?.setStatus(.simpleAdded)

<br />
<h5>Setting the CTBestPriceWidget price</h5>

When you make a best bestDailyRates request, the price is returned to you in the delegate callback didRecieveBestDailyRate. You can set the widget’s price in your function body: 

```swift
    func didReceiveBestDailyRate(_ price: NSNumber, currency: String) {
        let price = String(format: "%@ %.2f", currency, price.floatValue)
        self.widgetContainer?.setPrice(price)
    }
```
<br />
<h5>Setting the CTVehicleWidget vehicle</h5>

When a user has completed the CarTrawler inPath flow and added a vehicle, the vehicle will be returned to you in the vehicleSelected delegate callback. Using the below code, you could switch a best price widget into a populated vehicle widget: 
```swift  
    func vehicleSelected(_ vehicle: CTVehicleDetails) {
        self.widgetContainer?.setVehicle(vehicle)
        self.widgetContainer?.setStatus(.vehicle)
    }
```
<br />
<h5>Handling user interaction</h5> 
  
Conform to the CTWidgetContainerDelegate and override any of the following functions that you require: 
<br />
didTapView

  ```swift
    func didTapView(_ container: CTWidgetContainer) {
      // Widget view tapped
    }
  ```

didTapAddCarHire

  ```swift    
    func didTapAddCarHire(_ container: CTWidgetContainer) {
      // Widget 'Car Hire' button tapped
    }
  ```
didTapRemoveButton

  ```swift    
    func didTapRemoveButton(_ container: CTWidgetContainer) {
      // Widget remove button tapped
    }
  ```

vehicleSelected

  ```swift
    func vehicleSelected(_ vehicle: CTVehicleDetails) {
      // Vehicle selected by user
    }
  ```

