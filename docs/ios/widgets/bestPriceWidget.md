---
layout: default
title: Best Price
parent: iOS SDK Widgets
grand_parent: iOS
nav_order: 1
permalink: /docs/ios/widgets/best-price
---

# Best Price Widget

{: .no_toc }

---

![](/uploads/Pricing_Loaded_Generic_iOS.png)

## Setting the CTBestPriceWidget price

When you make a `bestDailyRates` request, the price is returned to you in the delegate callback `didReceiveBestDailyRate`. You can set the widget’s price in your function body: 

```java
func didReceiveBestDailyRate(_ price: NSNumber, currency: String) {
    let price = String(format: "%@ %.2f", currency, price.floatValue)
    self.widgetContainer?.setPrice(price)
}
```