---
layout: default
title: CarTrawler Cash
parent: Customisation
grand_parent: iOS
nav_order: 3
permalink: /docs/ios/customisation/cartrawler-cash
---

# CarTrawler Cash

{: .no_toc }

CarTrawler Cash provides enhanced cash voucher merchandising throughout the booking flow.

---

![](/uploads/cash_small_banner.svg)

![](/uploads/cash_big_banner.svg)

---

## Applying the theme

<b>Create a CTCashStyle object and set the theme properties:</b>
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

   style.cashStyle = cashStyle
``` 
Note: 
* If dark colors are not set, default CTCashStyle colors will be used.

--- 

For every logo you can provide a UIImage or an NSURL. All you need is to
do is set the following properties:

#### Cash Style sample code
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
Note: 
* If logos are provided as UIImage, NSURL properties will be ignored.
* If dark logos are not set, default CTCashStyle logos will be used.
* If you don't provide any attributes for the logo, the component that shows the logo will be hidden. 

---


After CTStyle has been configured with your cashStyle, pass it into the SDK during initialisation:

#### SDK Initialisation with CashStyle
```java
   style.cashStyle = cashStyle

   CarTrawlerSDK.sharedInstance().initialiseSDK(with: style,
                                 customParameters: nil,
                                 production: false)
```