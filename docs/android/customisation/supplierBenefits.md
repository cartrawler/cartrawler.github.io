---
layout: default
title: Supplier Benefits
parent: Customisation
grand_parent: Android Integration
nav_order: 7
permalink: /docs/android/customisation/supplier-benefits/
---

# Supplier Benefits
{: .no_toc }

On the vehicles list a banner may be shown to the user to allow them to apply some promo/discount
codes. This banner can be customized as you wish as described below.

<details open markdown="block">
  <summary>
    Table of Contents
  </summary>
  {: .text-delta }
1. TOC
{:toc}
</details>

---

![](/uploads/supplier_benefits_banner.png)

---

## Applying the theme

Override the following properties in your SDK theme
```xml
<style name="CarTrawlerSDKTheme" parent="CTDayNightTheme" >
     //..
     <item name="ctSupplierBenefitsLogoURL">https://www.mylogo.com/mylogo.png</item>
     <item name="ctSupplierBenefitsBgColor">@color/bannerBgColor</item>
     <item name="ctSupplierBenefitsButtonBgColor">@color/bannerButtonBgColor</item>
     <item name="ctSupplierBenefitsButtonTextColor">@color/bannerButtonTextColor</item>
     <item name="ctSupplierBenefitsTextColor">@color/bannerTextColor</item>
</style>
```   

For the logo url you can provide a local drawable instead of a remote url. All you need to do is
use the following attributes:

```xml
<style name="CarTrawlerSDKTheme" parent="CTDayNightTheme" >
    <item name="ctSupplierBenefitsLogoImage">@drawable/my_logo</item>
</style>
```

{: .note}
<small>If you provide both a <b>drawable</b> and a <b>url</b> the drawable will take priority, so your url will never be used.<br />
If you don't provide any attributes for the logo, the component that shows the logo will be hidden (`View.GONE`)</small>

---

## Fallback Colors

By default our CTDayNightTheme will use the following colors if partner doesn't initialise them

```xml
<style name="CTDayNightTheme" parent="Theme.MaterialComponents.DayNight.NoActionBar">
    <item name="ctSupplierBenefitsBgColor">?colorPrimary</item>
    <item name="ctSupplierBenefitsButtonBgColor">?colorSecondary</item>
    <item name="ctSupplierBenefitsButtonTextColor">?colorOnSecondary</item>
    <item name="ctSupplierBenefitsTextColor">?colorOnSecondary</item>
</style>
```

---

## Autoenabling Supplier Benefits During SDK initialisation

The SDK builder's `enableSupplierBenefitAutoApplyCodes` allows Partners to initialise the SDK and opt in to apply ALL automatic codes that can be applied for suppliers. It is optional. 

```kotlin
CartrawlerSDK
    .Builder()
    .enableSupplierBenefitAutoApplyCodes()
```