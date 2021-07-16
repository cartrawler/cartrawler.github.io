---
title: Widgets
position: 6
type: Android
description:
right_code: >

  {: title="Theme" }
---


In the Widget library we have four new Widgets classes, see images to see how they would appear in your App.
These can be added to your layouts dynamically or directly via in XML as per standard Android guidelines.

1. cartrawler.core.ui.views.partner.CTSimpleWidget
2. cartrawler.core.ui.views.partner.CTSimpleAddedWidget
3. cartrawler.core.ui.views.partner.CTBestPriceWidget
4. cartrawler.core.ui.views.partner.CTVehicleWidget

These widgets can be combined to show different state of the basket, in a flight booking scenario, 
you might want to display a widget to advertise you can add a rental car to your basket at various points in the flow. 
For example you could show the following CTSimpleWidget:

<picture>
  <source media="(max-width: 799px)" srcset="/uploads/Simple_Loaded_State_Generic.png">
  <source media="(min-width: 800px)" srcset="/uploads/Simple_Loaded_State_Generic.png">
  <img src="/uploads/Simple_Loaded_State_Generic.png">
</picture>


You can add a click listener to this CTSimpleWidget that will start the inpath or standalone flow , using the standard View.OnClickListener

You may decide when onActivityResult is called following the completion of the inpath flow (Activity), you will show that a
rental car has beed added, by using displaying the view CTSimpleAddedWidget or alternatively CTVehicleWidget

<picture>
  <source media="(max-width: 799px)" srcset="/uploads/Simple_Added_State_Generic.png">
  <source media="(min-width: 800px)" srcset="/uploads/Simple_Added_State_Generic.png">
  <img src="/uploads/Simple_Added_State_Generic.png">
</picture>

**OR**

<picture>
  <source media="(max-width: 799px)" srcset="/uploads/Pricing_Added_State_Generic.png">
  <source media="(min-width: 800px)" srcset="/uploads/Pricing_Added_State_Generic.png">
  <img src="/uploads/Pricing_Added_State_Generic.png">
</picture>

In order to use the CTVehicleWidget, you will need pass it Vehicle Object, that is returned following the Cartrawler flow, see example below
on how you would achieve this:

````kotlin
override fun onActivityForResult(requestCode: Int, resultCode: Int, data: Intent?) {
       if (resultCode == Activity.RESULT_OK) {
           if (requestCode == 123) {
               	// Set the widget to the added state
               ctVehicleWidget.setVehicle(data.getParcelableExtra(CartrawlerSDK.VEHICLE_DETAILS))
              
           }
       }
    }
````
    
In order to use the CTBestPriceWidget, you will need to set the price on the widget, 
for example this can be done in onReceiveBestDailyRate (when the price is returned from the API).

```kotlin
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

<picture>
  <source media="(max-width: 799px)" srcset="/uploads/Pricing_Loaded_State_Generic.png">
  <source media="(min-width: 800px)" srcset="/uploads/Pricing_Loaded_State_Generic.png">
  <img src="/uploads/Pricing_Loaded_State_Generic.png">
</picture>


***Theming widgets***


Add the following XML styling to your theme xml resources. The example below defines default Android styles (with some CT brand styling) for these Widgets,
these can be completely tailored to your own brand styling. `

````xml
<style name="YourWidgetTheme" parent="Theme.AppCompat.Light">
          
            <!-- New Attributes for new Widgets -->
    
            <item name="CTSimpleWidgetImage">@drawable/ct_static_card_default</item>
            <item name="CTSimpleWidgetTitle">@style/TextAppearance.AppCompat.Title</item>
            <item name="CTSimpleWidgetSubtitle">@style/TextAppearance.AppCompat.Body1</item>
    
            <item name="CTBestPriceWidgetImage">@drawable/ct_static_card_default</item>
            <item name="CTBestPriceWidgetTitle">@style/TextAppearance.AppCompat.Title</item>
            <item name="CTBestPriceWidgetSubtitle">@style/TextAppearance.AppCompat.Body1</item>
    
            <item name="CTBestPriceWidgetCTAColor">@color/CT_ColorPrimary</item>
            <item name="CTBestPriceCTACornerRadius">@dimen/ct_bestprice_cta_corner_radius</item>
            <item name="CTBestPriceCTAText">@style/CTBestPriceCTAText</item>
    
            <item name="CTCarAddedCheckIconColor">@color/CT_ColorPrimary</item>
            <item name="CTCarAddedTitleText">@style/TextAppearance.AppCompat.Title</item>
            <item name="CTCarRemoveText">@style/CTWidgetRemoveAppearance</item>
    
            <item name="CTPriceLabelText">@style/CTWidgetLabelAppearance</item>
            <item name="CTPriceValueText">@style/TextAppearance.AppCompat.Title</item>
    
        </style>
````

See Graphics below for descriptions on which style applies to which widget


<picture>
  <source media="(max-width: 799px)" srcset="/uploads/Pricing_Loaded_Generic_style.png">
  <source media="(min-width: 800px)" srcset="/uploads/Pricing_Loaded_Generic_style.png">
  <img src="/uploads/Pricing_Loaded_Generic_style.png">
</picture>

<picture>
  <source media="(max-width: 799px)" srcset="/uploads/Simple_Added_Generic_style.png">
  <source media="(min-width: 800px)" srcset="/uploads/Simple_Added_Generic_style.png">
  <img src="/uploads/Simple_Added_Generic_style.png">
</picture>

<picture>
  <source media="(max-width: 799px)" srcset="/uploads/Pricing_Added_Generic_style.png">
  <source media="(min-width: 800px)" srcset="/uploads/Pricing_Added_Generic_style.png">
  <img src="/uploads/Pricing_Added_Generic_style.png">
</picture>

<picture>
  <source media="(max-width: 799px)" srcset="/uploads/Simple_Loaded_Generic_style.png">
  <source media="(min-width: 800px)" srcset="/uploads/Simple_Loaded_Generic_style.png">
  <img src="/uploads/Simple_Loaded_Generic_style.png">
</picture>
