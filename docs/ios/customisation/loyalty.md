---
layout: default
title: Loyalty
parent: Customisation
grand_parent: iOS Integration
nav_order: 5
permalink: /docs/ios/customisation/loyalty
---

# Loyalty
{: .no_toc }

The colour of the background, text, and icon on our loyalty components can be changed to fit in with your app’s loyalty program branding. Loyalty components include banners as well as specific loyalty chips. The chip's height can also be increased if your logo requires more padding. See more on the components section of our style guide. 

<details open markdown="block">
  <summary>
    Table of Contents
  </summary>
  {: .text-delta }
1. TOC
{:toc}
</details>

---

Similar to SDK theming, if you are changing the background colour to a dark background (and configuring the loyalty program as a dark theme), it is recommended you change the text and icon colour to a lighter colour for legibility. The SDK will change the Loyalty Program logo automatically for the loyalty dark theme and light theme.

<picture>
  <source media="(max-width: 920px)" srcset="/uploads/loyalty-theming.png">
  <source media="(min-width: 920px)" srcset="/uploads/loyalty-theming.png">
  <img src="/uploads/loyalty-theming.png">
</picture>

---

## Theme Defaults 

The default is ```light``` and theme values are as follows:

| Attribute                   	 | Color                                                                   	|
|-----------------------------	 |-------------------------------------------------------------------------	|
| Primary Color       	         | ![#eeeef3](https://via.placeholder.com/10/eeeef3/000000?text=+) #e5ebed 	|
| Primary Text Color    	       | ![#333333](https://via.placeholder.com/10/333333/000000?text=+) #333333 	|
| Secondary Color     	         | ![#FFFFFF](https://via.placeholder.com/10/FFFFFF/000000?text=+) #FFFFFF 	|
| Secondary Text Color 	         | ![#FFFFFF](https://via.placeholder.com/10/000000/000000?text=+) #000000 	|
| Secondary Variant Color 	     | 	Defaults to Secondary Color attribute                                   |

---

## Dark mode defaults

The default is ```dark``` and theme values are as follows:

| Attribute                   	 | Color                                                                   	|
|-----------------------------	 |-------------------------------------------------------------------------	|
| Primary Color       	         | ![#000000](https://via.placeholder.com/10/000000/000000?text=+) #000000 	|
| Primary Text Color    	       | ![#FFFFFF](https://via.placeholder.com/10/FFFFFF/000000?text=+) #FFFFFF 	|
| Secondary Color     	         | ![#1E1E1E](https://via.placeholder.com/10/1E1E1E/000000?text=+) #1E1E1E 	|
| Secondary Text Color 	         | ![#FFFFFF](https://via.placeholder.com/10/FFFFFF/000000?text=+) #FFFFFF 	|
| Secondary Variant Color 	     | 	Defaults to Secondary Color attribute                                   |


---

## CTLoyaltyTheme

### Light Theme
{: .no_toc}
Apply ```light``` to apply a dark logo for the light theme.

### Dark Theme
{: .no_toc}
Apply ```dark``` to apply a light (white) logo for the dark theme

---

## Applying the Theme 
Override the following properties in your SDK style

```java
let style = CTStyle(theme: .dark,  // Main app style
          primaryColor: UIColor.gray)

let loyaltyStyle = CTLoyaltyStyle(theme: .light) // Loyalty style Either .light (default) or .dark

loyaltyStyle.primaryColor = UIColor.lightGray // Optional, default #e5ebed
loyaltyStyle.primaryTextColor = UIColor.darkGray // Optional, default #333333
loyaltyStyle.secondaryColor = UIColor.blue // Optional, default #FFFFFF
loyaltyStyle.secondaryVariantColor = UIColor.gray // Optional, defaults to Secondary Color attribute
loyaltyStyle.secondaryTextColor = UIColor.white // Optional, default #000000

style.loyaltyStyle = loyaltyStyle
```   

---

## Loyalty Chip Size
The default size of the chip is regular.

![](/uploads/loyalty_chip.png)

 This can be changed to large as follows: 

```java
let style = CTStyle(theme: .dark,  // Main app style
          primaryColor: UIColor.gray)

let loyaltyStyle = CTLoyaltyStyle(theme: .light) // Loyalty style Either .light (default) or .dark

loyaltyStyle.chipSize = .large // Optional, default .regular

style.loyaltyStyle = loyaltyStyle
```

## Loyalty Banner Landing Screen
The template of the loyalty banner on the landing screen can be modified. There are two templates available "Default" and "Logo & Text". The latter is a full width text with a central logo and an optional gradient background.

<img src="/uploads/loyalty_banner_templates.png">

The Logo & Text template can be styled as follows:

Create a style for the banner:
```java
let bannerStyle = CTBannerStyle(type: .landingLoyalty,
                                template: .logoAndText) // Either .default or .logoAndText. Default is .default
bannerStyle.textFont = UIFont(name: "P22JohnstonUnderground", size: 20) // Optional custom font

bannerStyle.textColor = UIColor.white // Defaults to iOS's black color
bannerStyle.bgColor = UIColor.gray // Defaults to the theme's primary colour
bannerStyle.secondaryBgColor = UIColor.black // Defaults to the bgColor
bannerStyle.logoImage = UIImage(named: "logo-white") // If not set, it retrieves it from the Loyalty API
bannerStyle.infoButtonColor = UIColor.white // Defaults to iOS's black color

bannerStyle.darkTextColor = UIColor.black // If not set, defaults to textColor
bannerStyle.darkBgColor = UIColor.white // If not set, defaults to bgColor
bannerStyle.darkSecondaryBgColor = UIColor.lightGray // If not set, defaults to secondaryBgColor
bannerStyle.darkLogoImage = UIImage(named: "logo") // If not set, defaults to logoImage
bannerStyle.darkInfoButtonColor = UIColor.black // If not set, defaults to infoButtonColor
```

Add the new style to the main style:
```java
var style = CTStyle(theme: .dark, primaryColor: UIColor.white)
style.add(bannerStyle)
```

Initialise the SDK with the style created:
```java
import CarTrawlerSDK

CarTrawlerSDK.sharedInstance().initialiseSDK(with: style,
                               customParameters: nil,
                               production: false)
```