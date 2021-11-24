---
layout: default
title: iOS SDK Widgets
parent: iOS
nav_order: 5
has_children: true
---

# Widgets

{: .no_toc }

The CarTrawler iOS SDK comes prepackaged with four widgets.<br /><br /> When considering the addition of a new touchpoint into the CarTrawler flow from within your app, you can choose the one that best suits your needs.<br /><br /> Each widget is independent of the Standalone and In Path flows, and can be used to launch either of them via your implementation of the `CTWidgetContainerDelegate`’s `didTapView` function.<br />

---

### Implementation 

To implement the widgets, you must first create an instance of `CTWidgetStyle` and set the relevant properties, as shown in each widget's diagram. <br /><br />
The next step is to create an instance of `CTWidgetContainer` and pass in the following: status (`.simple`, `.simpleAdded`, `.bestPrice`, or `.vehicle`), style (your `CTWidgetStyle` object), and a delegate. 
We recommend adding the widget container to a stackView, like so: 

```java
let widgetStyle = CTWidgetStyle()
let widgetContainer = CarTrawlerSDK.sharedInstance().getWidget(status: .simple,
                                                                   style: widgetStyle,
                                                                   delegate: self)
self.stackWidgetView.insertArrangedSubview(widgetContainer, at: 0)
```

Your widget’s status can be changed at any time by calling setStatus on the container:
```java
self.widgetContainer?.setStatus(.simpleAdded)
```

--- 

### Handling User Interaction
Conform to the CTWidgetContainerDelegate and override any of the following functions that you require:

#### did Tap View
```java
func didTapView(_ container: CTWidgetContainer) {
  // Widget view tapped
}
``` 

#### did Tap Add Car Hire
```java
func didTapAddCarHire(_ container: CTWidgetContainer) {
  // Widget 'Car Hire' button tapped
}
``` 

#### did Tap Remove Button
```java
func didTapRemoveButton(_ container: CTWidgetContainer) {
  // Widget remove button tapped
}
``` 

#### vehicle Selected
```java
func vehicleSelected(_ vehicle: CTVehicleDetails) {
  // Vehicle selected by user
}
``` 