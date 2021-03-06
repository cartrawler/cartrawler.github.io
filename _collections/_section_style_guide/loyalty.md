---
title: Loyalty
position: 5
type:
description:
right_code: >-
---

It is possible to change the background colour and the text/icon colour of loyalty components to fit in with your app’s loyalty program branding. Loyalty components include a general messaging banner and more car and specific loyalty chips. See more on the components section of this style guide.

Similar to SDK theming, if you are changing the background colour to a dark background (and configuring the loyalty program as a dark theme), it is recommended you change the text and icon colour to a lighter colour for legibility. The SDK will change the Loyalty Program logo automatically for loyalty dark theme and light theme.

<picture>
  <source media="(max-width: 920px)" srcset="/uploads/loyalty-theming.png">
  <source media="(min-width: 920px)" srcset="/uploads/loyalty-theming.png">
  <img src="/uploads/loyalty-theming.png">
</picture>

### Theme Defaults 

The default is ```light``` and theme values are as follows:

| Attribute                   	| Color                                                                   	|
|-----------------------------	|-------------------------------------------------------------------------	|
| Primary Color       	         | ![#eeeef3](https://via.placeholder.com/10/eeeef3/000000?text=+) #e5ebed 	|
| Primary Text Color    	      | ![#333333](https://via.placeholder.com/10/333333/000000?text=+) #333333 	|
| Secondary Color     	         | ![#FFFFFF](https://via.placeholder.com/10/FFFFFF/000000?text=+) #FFFFFF 	|
| Secondary TextColor 	         | ![#FFFFFF](https://via.placeholder.com/10/000000/000000?text=+) #000000 	|

#### Dark mode defaults

The default is ```dark``` and theme values are as follows:

| Attribute                   	| Color                                                                   	|
|-----------------------------	|-------------------------------------------------------------------------	|
| Primary Color       	         | ![#000000](https://via.placeholder.com/10/000000/000000?text=+) #000000 	|
| Primary Text Color    	      | ![#FFFFFF](https://via.placeholder.com/10/FFFFFF/000000?text=+) #FFFFFF 	|
| Secondary Color     	         | ![#1E1E1E](https://via.placeholder.com/10/1E1E1E/000000?text=+) #1E1E1E 	|
| Secondary TextColor 	         | ![#FFFFFF](https://via.placeholder.com/10/FFFFFF/000000?text=+) #FFFFFF 	|

### CTLoyaltyTheme

#### Light Theme
Apply ```light``` to apply a dark logo for the light theme.

#### Dark Theme
Apply ```dark``` to apply a light (white) logo for the dark theme

### Applying the Theme - Android
Override the following properties in your SDK theme
```xml
   <style name="CarTrawlerSDKTheme" parent="CTDayNightTheme" >
        //..
        <item name="ctLoyaltyPrimaryColor">@color/loyaltyPrimaryColor</item>
        <item name="ctLoyaltyPrimaryTextColor">@color/loyaltyPrimaryTextColor</item>
        <item name="ctLoyaltySecondaryColor">@color/loyaltySecondaryColor</item>
        <item name="ctLoyaltySecondaryColorText">@color/loyaltySecondaryColorText</item>
        <item name="ctLoyaltyTheme">dark</item> //Either "dark" or "light" Default is "light"
   </style>
```   

### Applying the Theme - iOS
Override the following properties in your SDK style
```swift
   let style = CTStyle(theme: .dark,  // Main app style
             primaryColor: UIColor.gray)

   let loyaltyStyle = CTLoyaltyStyle(theme: .light) // Loyalty style Either .dark or .light. Default is .light
   loyaltyStyle.primaryColor = UIColor.lightGray // Optional, default #e5ebed
   loyaltyStyle.primaryTextColor = UIColor.darkGray // Optional, default #333333
   loyaltyStyle.secondaryColor = UIColor.blue // Optional, default #FFFFFF
   loyaltyStyle.secondaryTextColor = UIColor.white // Optional, default #000000
   
   style.loyaltyStyle = loyaltyStyle
```   

