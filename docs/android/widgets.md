---
layout: default
title: Android SDK Widgets
parent: Android
nav_order: 5
has_children: true
---

# Widgets

{: .no_toc }

In the Widget library we have four new Widgets classes.
The widgets can be added to your layouts dynamically or directly via in XML as per standard Android guidelines.

They can can also be combined to show different states of the basket. For example, in a flight booking scenario 
you might want to display a widget to advertise you can add a rental car to your basket at various points in the flow. 

1. cartrawler.core.ui.views.partner.CTSimpleWidget
2. cartrawler.core.ui.views.partner.CTSimpleAddedWidget
3. cartrawler.core.ui.views.partner.CTBestPriceWidget
4. cartrawler.core.ui.views.partner.CTVehicleWidget

## Theming widgets

Add the following XML styling to your theme xml resources. The example below defines default Android styles (with some CT brand styling) for these Widgets,
these can be completely tailored to your own brand styling. 

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