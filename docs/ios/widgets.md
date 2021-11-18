---
layout: default
title: iOS SDK Widgets
parent: iOS
nav_order: 5
has_children: true
---

# Widgets

{: .no_toc }

The CarTrawler iOS SDK comes prepackaged with four widgets.<br /><br /> When considering the addition of a new touchpoint into the CarTrawler flow from within your app, you can choose the one that best suits your needs.<br /><br /> Each widget is independent of the Standalone and InPath flows, and can be used to launch either of them via your implementation of the CTWidgetContainerDelegate’s didTapView function.<br />

---

To implement the widgets, you must first create an instance of CTWidgetStyle and set the relevant properties, as shown in the above diagrams. <br /><br />
The next step is to create an instance of CTWidgetContainer and pass in the following: status (.simple, .simpleAdded, .bestPrice, or .vehicle), style (your CTWidgetStyle object), and a delegate. 
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
