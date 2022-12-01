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

This widget shows a list of suppliers and can act as an entry point into one of our flows by conforming to the CTWidgetContainerDelegate and overriding the `didTapView` function.

---

![](/uploads/Simple_Loaded_Generic_style.png)

```java
let widgetStyle = CTWidgetStyle()
let widgetContainer = CarTrawlerSDK.sharedInstance().getWidget(status: .simple,
                                                                   style: widgetStyle,
                                                                   delegate: self)
self.stackWidgetView.insertArrangedSubview(widgetContainer, at: 0)

...

func didTapView(_ container: CTWidgetContainer) {
  // Widget view tapped
}
```
