---
layout: default
title: Implementation Steps
parent: Android SDK Widgets
grand_parent: Widgets
nav_order: 0
permalink: /docs/widgets/androidWidgets/widget-implementation-steps
---

# Implementation Steps
{: .no_toc }

Each widget is independent of the Standalone and In Path flows, and can be used to launch either of them.

To implement the SDK's widgets within your app, please use the following steps:

---

## Theming 

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