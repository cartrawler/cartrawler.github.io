---
title: Cartrawler Cash
position: 5
type:
description:
right_code: >-
---

See Graphics below for descriptions on which style applies to which widget:

<picture>
  <source media="(max-height: 379px)" srcset="/uploads/cash_small_banner.svg">
  <source media="(max-width: 955px)" srcset="/uploads/cash_small_banner.svg">
  <img style="max-width: 955px; max-height:379px;" src="/uploads/cash_small_banner.svg">
</picture>

<picture>
  <source media="(max-width: 994px)" srcset="/uploads/cash_big_banner.svg">
  <source media="(max-height: 463x)" srcset="/uploads/cash_big_banner.svg">
  <img style="max-width: 994px; max-height:463x;" src="/uploads/cash_big_banner.svg">
</picture>

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

Notes: 
* If you provide both a <b>drawable</b> and an <b>url</b> the drawable takes priority, so your url will never be used
* If you don't provide any attributes for the logo, the component that shows the logo will be hidden (View.GONE) 
<br/>
<br/>

### Applying the Theme - iOS

<b>Create an instance of CTCashStyle object to receive the theme properties:</b>
```swift
   let cashStyle = CTCashStyle()
   cashStyle.textColor = UIColor(hex: 0xffffff) // Cash Primary Text Color
   cashStyle.bgColor = UIColor(hex: 0x4BA4B5) // Cash Background Start Color
   cashStyle.secondaryBgColor = UIColor(hex: 0x004958) // Cash Background End Color
   cashStyle.accentTextColor = UIColor(hex: 0x1DAF90) // Cash Secondary Text Color
   
   // Dark Mode colors
   cashStyle.darkTextColor = UIColor(hex: 0x000000) // Cash Primary Text Color
   cashStyle.darkBgColor = UIColor(hex: 0xffffff) // Cash Background Start Color
   cashStyle.darkSecondaryBgColor = UIColor(hex: 0x002c52) // Cash Background End Color
   cashStyle.darkAccentTextColor = UIColor(hex: 0x1DAF90) //  Cash Secondary Text Color

   style.cashStyle = cashStyle
``` 
Notes: 
* If dark colors are not set, default CTCashStyle colors will be used.

<br/>
<br/>
<b>For every logo you can provide an UIImage or a NSURL. All you need is to
do is set the following properties:</b>
```swift
  cashStyle.smallLogoImage = UIImage(named: "small_logo") // Cash Small Logo
  cashStyle.logoImage = UIImage(named: "logo") // Cash Logo

  // To use NSURL properties, UIImage properties must not be set
  cashStyle.smallLogoURL = URL(string: "http://www.cartrawler.com/small_logo.png") // Cash Small Logo
  cashStyle.logoURL = URL(string: "http://www.cartrawler.com/logo.png") // Cash Logo
   
   // Dark Mode logos
  cashStyle.darkSmallLogoImage = UIImage(named: "dark_small_logo") // Cash Small Logo
  cashStyle.darkLogoImage = UIImage(named: "dark_logo") // Cash Logo

  // To use NSURL properties, UIImage properties must not be set
  cashStyle.darkSmallLogoURL = URL(string: "http://www.cartrawler.com/dark_small_logo.png") // Cash Small Logo
  cashStyle.darkLogoURL = URL(string: "http://www.cartrawler.com/dark_logo.png") // Cash Logo
```
Notes: 
* If logos are provided as UIImage, NSURL properties will be ignored.
* If dark logos are not set, default CTCashStyle logos will be used.
* If you don't provide any attributes for the logo, the component that shows the logo will be hidden. 

<br/>
<br/>
<b>Set the cashStyle property in your CTStyle instance and pass to the SDK:</b>
```swift
   style.cashStyle = cashStyle

   CarTrawlerSDK.sharedInstance().initialiseSDK(with: style,
                                 customParameters: nil,
                                 production: false)
```