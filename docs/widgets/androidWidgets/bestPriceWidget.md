---
layout: default
title: Best Price
parent: Android SDK Widgets
grand_parent: Widgets
nav_order: 3
# permalink: /docs/widgets/androidWidgets/best-price/
---

# Best Price Widget
{: .no_toc }

<b>cartrawler.core.ui.views.partner.CTBestPriceWidget</b>

The Best Price Widget is used to display the cheapest available vehicle price for a given location based on the pick up and drop off dates. To get this price, use our <a href="/docs/api/android/best-daily-rates#best-daily-rates">Best Daily Rates API</a>

---

![](/uploads/Pricing_Loaded_Generic_style.png)

## Setting the Best Price Widget Price

In order to use the `CTBestPriceWidget`, you will need to set the price on the widget, 
for example this can be done in `onReceiveBestDailyRate` (when the price is returned from the API).

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