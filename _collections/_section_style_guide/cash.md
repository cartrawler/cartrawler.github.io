---
title: Cartrawler Cash
position: 5
type:
description:
right_code: >-
---

### CTCashTheme

### Applying the Theme - Android
Override the following properties in your SDK theme
```xml
   <style name="CarTrawlerSDKTheme" parent="CTDayNightTheme" >
        //..
        <item name="ctCashBgStartColor">@color/bannerBgStartColor</item>
        <item name="ctCashBgEndColor">@color/bannerBgEndColor</item>
        <item name="ctCashPrimaryTextColor">@color/colorOnSurface</item>
        <item name="ctCashSecondaryTextColor">@color/colorSecondary</item>
        <item name="ctCashLogoUrl">https://www.mylogo.com/mylogo.png</item>
        <item name="ctCashSmallLogoUrl">https://www.mylogo.com/mylogo.png</item>
        <item name="ctUSPLogoUrl">https://www.mylogo.com/mylogo.png</item>
   </style>
```   

For every LogoUrl above you also have a way to provide a local drawable instead a remote url. All you need is to
use the following attributes:

```xml
    <style name="CarTrawlerSDKTheme" parent="CTDayNightTheme" >
        <item name="ctCashLogoDrawable">@drawable/my_logo</item>
        <item name="ctCashSmallLogoDrawable">@drawable/my_logo_small</item>
        <item name="ctUSPLogoDrawable">">@drawable/my_logo_usp</item>
    </style>
```

Notes: 
* If you provide both a <b>drawable</b> and an <b>url</b> the drawable takes priority, so your url will never be used
* If you don't provide any attributes for the logo, the component that shows the logo will be hidden (View.GONE) 

See Graphics below for descriptions on which style applies to which widget:

<picture>
  <img src="/uploads/cash_usp_banner.svg">
</picture>

<picture>
  <source media="(max-height: 367px)" srcset="/uploads/cash_small_banner.svg">
  <source media="(max-width: 1098px)" srcset="/uploads/cash_small_banner.svg">
  <img src="/uploads/cash_small_banner.svg">
</picture>

<picture>
  <source media="(max-width: 799px)" srcset="/uploads/cash_big_banner.svg">
  <source media="(min-width: 800px)" srcset="/uploads/cash_big_banner.svg">
  <img src="/uploads/cash_big_banner.svg">
</picture>
