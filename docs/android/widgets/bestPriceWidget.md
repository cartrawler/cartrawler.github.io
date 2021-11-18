---
layout: default
title: Best Price
parent: Android SDK Widgets
grand_parent: Android
nav_order: 3
permalink: /docs/android/widgets/best-price/
---

# Best Price Widget

{: .no_toc }

---

<picture>
  <source media="(max-width: 799px)" srcset="/uploads/Pricing_Loaded_Generic_style.png">
  <source media="(min-width: 800px)" srcset="/uploads/Pricing_Loaded_Generic_style.png">
  <img src="/uploads/Pricing_Loaded_Generic_style.png">
</picture><br />

In order to use the CTBestPriceWidget, you will need to set the price on the widget, 
for example this can be done in onReceiveBestDailyRate (when the price is returned from the API).

```java
builder.getBestDailyRates(
            this, object: CartrawlerSDK.BestDailyRatesListener {

        @SuppressLint("SetTextI18n")
        override fun onNoResults(type: Int) {
            hideLoading()
            rentalPrice.text = "No Results"
        }

        override fun onError(type: Int, connectionError: CartrawlerSDK.ConnectionError) {
            hideLoading()
            Log.e("Builder", connectionError.message, connectionError)
            rentalPrice.text = connectionError.message
        }

        @SuppressLint("SetTextI18n")
        override fun onReceiveBestDailyRate(type: Int, price: Double, currency: String) {
            hideLoading()
            ctBestPrice.setPrice("$currency $price")
        }
    }, CartrawlerSDK.Builder.FLAG_RENTAL)
```