---
layout: default
title: CarTrawler Cash
parent: Customisation
grand_parent: Android Integration
nav_order: 4
permalink: /docs/android/customisation/cartrawler-cash/
---

# CarTrawler Cash
{: .no_toc }

CarTrawler Cash provides enhanced cash voucher merchandising throughout the booking flow.

---

![](/uploads/cash_small_banner.svg)

![](/uploads/cash_big_banner.svg)

---

## Applying the theme

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
</style>
```   

For every logo url you can provide a local drawable instead of a remote url. All you need is to
use the following attributes:

```xml
<style name="CarTrawlerSDKTheme" parent="CTDayNightTheme" >
    <item name="ctCashLogoDrawable">@drawable/my_logo</item>
    <item name="ctCashSmallLogoDrawable">@drawable/my_logo_small</item>
</style>
```


{: .note}
<small>If you provide both a <b>drawable</b> and a <b>url</b> the drawable will take priority, so your url will never be used.<br/>
If you don't provide any attributes for the logo, the component that shows the logo will be hidden (`View.GONE`)</small>