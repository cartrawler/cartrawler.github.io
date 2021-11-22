---
layout: default
title: Simple Added
parent: iOS SDK Widgets
grand_parent: iOS
nav_order: 2
permalink: /docs/ios/widgets/simple-added
---

# Simple Added Widget

{: .no_toc }

---

![](/uploads/Simple_Added_Generic_iOS.png)

#### Simple Added Widget
```java
let widgetStyle = CTWidgetStyle()
let widgetContainer = CarTrawlerSDK.sharedInstance().getWidget(status: .simpleAdded,
                                                                   style: widgetStyle,
                                                                   delegate: self)
self.stackWidgetView.insertArrangedSubview(widgetContainer, at: 0)
```
