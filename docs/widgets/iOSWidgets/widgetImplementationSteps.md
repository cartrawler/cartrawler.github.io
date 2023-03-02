---
layout: default
title: Implementation Steps
parent: iOS SDK Widgets
grand_parent: Widgets
nav_order: 0
# permalink: /docs/widgets/iOSWidgets//widget-implementation-steps
---

# Implementation Steps
{: .no_toc }

Each widget is independent of the Standalone and In Path flows, and can be used to launch either of them via your implementation of the `CTWidgetContainerDelegate`’s `didTapView` function.<br />

To implement the SDK's widgets within your app, please use the following steps:

<details open markdown="block">
  <summary>
    Steps
  </summary>
  {: .text-delta }
1. TOC
{:toc}
</details>

---

## Initialise the SDK in your App Delegate <br/>

```java
import CarTrawlerSDK

// In application(_:didFinishLaunchingWithOptions:)
CarTrawlerSDK.sharedInstance().initialiseSDK(with: nil,
                               customParameters: nil,
                               production: false)
```

{: .important }
The production parameter must be set to true when submitting your app to the AppStore.

## Create a CTWidgetStyle Object 

To implement the widgets, you must first create an instance of `CTWidgetStyle` and set the relevant properties, as shown in each widget type's diagram. 

```java
let widgetStyle = CTWidgetStyle()
```

## Create a CTWidgetContainer 
The next step is to create an instance of `CTWidgetContainer` and pass in the following: status (`.simple`, `.simpleAdded`, `.bestPrice`, or `.vehicle`), style (your `CTWidgetStyle` object), and a delegate. 
We recommend adding the widget container to a stackView. 

Here is an example of using the simple widget: 

```java
let widgetContainer = CarTrawlerSDK.sharedInstance().getWidget(status: .simple,
                                                                   style: widgetStyle,
                                                                   delegate: self)
self.stackWidgetView.insertArrangedSubview(widgetContainer, at: 0)
```

{: .note}
Your widget’s status can be changed at any time by calling setStatus on the container:
```java
self.widgetContainer?.setStatus(.simpleAdded)
```

--- 

## Handling User Interaction
To handle user interaction of the widgets, conform to the CTWidgetContainerDelegate and override any of the following functions that you require:

### Did Tap View
```java
func didTapView(_ container: CTWidgetContainer) {
  // Widget view tapped
}
``` 

### Did Tap Add Car Hire
```java
func didTapAddCarHire(_ container: CTWidgetContainer) {
  // Widget 'Car Hire' button tapped
}
``` 

### Did Tap Remove Button
```java
func didTapRemoveButton(_ container: CTWidgetContainer) {
  // Widget remove button tapped
}
``` 

### Vehicle Selected
```java
func vehicleSelected(_ vehicle: CTVehicleDetails) {
  // Vehicle selected by user
}
``` 

