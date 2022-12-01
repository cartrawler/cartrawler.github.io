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

The Best Price Widget is used to display the cheapest available vehicle price for a given location based on the pick up and drop off dates. To get this price, use our <a href="/docs/ios/apis/best-daily-rates#best-daily-rates">Best Daily Rates API</a>

---

![](/uploads/Pricing_Loaded_Generic_iOS.png)

```java
let widgetStyle = CTWidgetStyle()
let widgetContainer = CarTrawlerSDK.sharedInstance().getWidget(status: .bestPrice,
                                                                   style: widgetStyle,
                                                                   delegate: self)
self.stackWidgetView.insertArrangedSubview(widgetContainer, at: 0)
```

## Setting the CTBestPriceWidget Price

When you make a `bestDailyRates` request, the price is returned to you in the delegate callback `didReceiveBestDailyRate`. You can set the widget’s price in your function body: 

```java
func didReceiveBestDailyRate(_ price: NSNumber, currency: String) {
    let price = String(format: "%@ %.2f", currency, price.floatValue)
    self.widgetContainer?.setPrice(price)
}
```