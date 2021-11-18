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

<!-- A CTAPIQueryParams object must be created and initialised after initialising the SDK to make requests using our API methods. -->

---

<picture>
  <source media="(max-width: 799px)" srcset="/uploads/Pricing_Loaded_Generic_iOS.png">
  <source media="(min-width: 800px)" srcset="/uploads/Pricing_Loaded_Generic_iOS.png">
  <img src="/uploads/Pricing_Loaded_Generic_iOS.png_iOS">
</picture><br />

## Setting the CTBestPriceWidget price

When you make a best bestDailyRates request, the price is returned to you in the delegate callback didRecieveBestDailyRate. You can set the widget’s price in your function body: 

```java
func didReceiveBestDailyRate(_ price: NSNumber, currency: String) {
    let price = String(format: "%@ %.2f", currency, price.floatValue)
    self.widgetContainer?.setPrice(price)
}
```