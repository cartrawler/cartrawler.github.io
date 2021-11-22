---
layout: default
title: Simple 
parent: iOS SDK Widgets
grand_parent: iOS
nav_order: 3
permalink: /docs/ios/widgets/simple
---

# Simple Widget

{: .no_toc }

---

![](/uploads/Simple_Loaded_State_Generic.png)

#### Simple Widget
```java
let widgetStyle = CTWidgetStyle()
let widgetContainer = CarTrawlerSDK.sharedInstance().getWidget(status: .simple,
                                                                   style: widgetStyle,
                                                                   delegate: self)
self.stackWidgetView.insertArrangedSubview(widgetContainer, at: 0)
```
