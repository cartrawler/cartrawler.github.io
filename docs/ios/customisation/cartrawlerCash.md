---
layout: default
title: CarTrawler Cash
parent: Customisation
grand_parent: iOS Integration
nav_order: 3
permalink: /docs/ios/customisation/cartrawler-cash
---

# CarTrawler Cash
{: .no_toc }

CarTrawler Cash provides enhanced cash voucher merchandising throughout the booking flow.

<details open markdown="block">
  <summary>
    Table of Contents
  </summary>
  {: .text-delta }
1. TOC
{:toc}
</details>

---

![](/uploads/cash_small_banner.svg)

![](/uploads/cash_big_banner.svg)

---

## Applying the theme

Create a `CTCashStyle` object and set the theme properties:
```java
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
``` 

Then, set this cashStyle as your `CTStyle` object's `cashStyle` property: 


```java
style.cashStyle = cashStyle
``` 
<small>(For CTStyle sample code, click<a href="/docs/ios/customisation/themes#creating-a-ctstyle"> here</a>)</small>


{: .note}
If dark colors are not set, default `CTCashStyle` colors will be used.

--- 

For every logo you can provide a `UIImage` or an `NSURL`. All you need is to
do is set the following properties:

```java
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

{: .note}
If logos are provided as `UIImage`, `NSURL` properties will be ignored.<br/>
If dark logos are not set, default `CTCashStyle` logos will be used.<br/>
If you don't provide any attributes for the logo, the component that shows the logo will be hidden. 

---

## SDK Initialisation with CashStyle

After `CTStyle` has been configured with your cashStyle, pass it into the SDK during initialisation:

```java
   style.cashStyle = cashStyle

   CarTrawlerSDK.sharedInstance().initialiseSDK(with: style,
                                 customParameters: nil,
                                 production: false)
```