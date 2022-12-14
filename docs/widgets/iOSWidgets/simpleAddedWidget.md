---
layout: default
title: Simple Added
parent: iOS SDK Widgets
grand_parent: Widgets
nav_order: 2
permalink: /docs/widgets/iOSWidgets/simple-added
---

# Simple Added Widget
{: .no_toc }

This widget shows that a car has been added, and provides a button to remove the car. 

---

![](/uploads/Simple_Added_Generic_iOS.png)

```java
let widgetStyle = CTWidgetStyle()
let widgetContainer = CarTrawlerSDK.sharedInstance().getWidget(status: .simpleAdded,
                                                                   style: widgetStyle,
                                                                   delegate: self)
self.stackWidgetView.insertArrangedSubview(widgetContainer, at: 0)
```
