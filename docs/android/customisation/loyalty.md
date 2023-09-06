---
layout: default
title: Loyalty
parent: Customisation
grand_parent: Android Integration
nav_order: 6
permalink: /docs/android/customisation/loyalty/
---

# Loyalty
{: .no_toc }

The colour of the background, text, and icon on our loyalty components can be changed to fit in with your app’s loyalty program branding. Loyalty components include banners as well as specific loyalty chips. See more on the components section of our style guide.

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

| Attribute                   	| Color                                                                   	|
|-----------------------------	|-------------------------------------------------------------------------	|
| Primary Color       	         | ![#eeeef3](https://via.placeholder.com/10/eeeef3/000000?text=+) #e5ebed 	|
| Primary Text Color    	       | ![#333333](https://via.placeholder.com/10/333333/000000?text=+) #333333 	|
| Secondary Color     	         | ![#FFFFFF](https://via.placeholder.com/10/FFFFFF/000000?text=+) #FFFFFF 	|
| Secondary Text Color 	         | ![#FFFFFF](https://via.placeholder.com/10/000000/000000?text=+) #000000 	|
| Secondary Variant Color 	     | 	Defaults to Secondary Color attribute                                   |

---

## Dark mode defaults

The default is ```dark``` and theme values are as follows:

| Attribute                   	| Color                                                                   	|
|-----------------------------	|-------------------------------------------------------------------------	|
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
Override the following properties in your SDK theme

```xml
<style name="CarTrawlerSDKTheme" parent="CTDayNightTheme" >
    //..
    <item name="ctLoyaltyPrimaryColor">@color/loyaltyPrimaryColor</item>
    <item name="ctLoyaltyPrimaryTextColor">@color/loyaltyPrimaryTextColor</item>
    <item name="ctLoyaltySecondaryColor">@color/loyaltySecondaryColor</item>
    <item name="ctLoyaltySecondaryColorText">@color/loyaltySecondaryColorText</item>
    <item name="ctLoyaltyTheme">dark</item> // Either "dark" or "light" Default is "light"
    <item name="ctLoyaltyChipSize">regular</item> // Either "regular" or "large" Default is "regular"
</style>
```   

---

## Loyalty Chip Size
The chip’s height can also be increased if your logo requires more padding. The default size of the chip is regular.
<img src="/uploads/loyalty_chip.png">

This can be changed to large as follows:
```xml
<style name="CarTrawlerSDKTheme" parent="CTDayNightTheme" >
    //..
    <item name="ctLoyaltyChipSize">regular</item> // Either "regular" or "large" Default is "regular"
</style>
```

## Loyalty Banner Landing Screen
The template of the loyalty banner on the landing screen can be modified. There are two templates available "Default" and "Logo & Text". The latter is a full width text with a central logo and an optional gradient background.

<img src="/uploads/loyalty_banner_templates.png">

The Logo & Text template can be styled as follows:

Create a style for the banner:
```xml
    <style name="ctLoyaltyLandingBannerStyle">
        <item name="ctBannerTemplate">logoAndTextBanner</item> // Either "defaultBanner" or "logoAndText". Default is "defaultBanner"
        <item name="ctBannerBackgroundColor">?colorPrimary</item> // Defaults to the theme's colorPrimary
        <item name="ctBannerSecondaryBackgroundColor">?colorSecondary</item> // Defaults to the theme's colorSecondary
        <item name="ctBannerTextColor">@android:color/white</item> // Defaults to Android's white color
        <item name="ctBannerInfoButtonColor">@android:color/white</item> // Defaults to Android's white color
        <item name="ctBannerImageDrawable">@drawable/your_logo</item> // If not set, it retrieves it from the Loyalty API
        <item name="ctBannerFont"> // Optional custom font
    </style>
```
Add the new style to the theme:
```xml
<style name="CarTrawlerSDKTheme" parent="CTDayNightTheme" >
    //..
    <item name="ctLoyaltyLandingBannerStyle">@style/ctLoyaltyLandingBannerStyle</item> 
</style>
```