---
title: Loyalty Theming
position: 9
type: Android
description:

##Android Loyalty Theme 

Please refer to the loyalty [styleguide]() on how to apply the appropriate theme for your loyalty program.

###Theme Defaults
The default theme is ```light```

The default ```light``` theme values are as follows:


| Attribute        | Color           |   |
| ------------- |:-------------| -----:|
| CTLoyaltyPrimaryColor      | ![#eeeef3](https://via.placeholder.com/15/eeeef3/000000?text=+) #e5ebed 
| CTLoyaltyPrimaryTextColor  | ![#333333](https://via.placeholder.com/15/333333/000000?text=+) #333333      
| CTLoyaltySecondaryColor    | ![#FFFFFF](https://via.placeholder.com/15/FFFFFF/000000?text=+) #FFFFFF 
| CTLoyaltySecondaryColorText| ![#FFFFFF](https://via.placeholder.com/15/000000/000000?text=+) #000000

###CTLoyaltyTheme

####Light Theme
Apply ```light``` to apply a dark logo for the light theme.

####Dark Theme
Apply ```dark``` to apply a light (white) logo for the dark theme

###Applying the Theme
Override the following properties in your SDK theme
```kotlin
<style name="BestAppTheme" parent="Theme.AppCompat.Light">
   // my own custom styles
</style>

   <style name="CartTrawlerSDKTheme parent="CTDarkTheme">
        ....
        .... 
        <item name="CTLoyaltyPrimaryColor">@color/loyaltyPrimaryColor</item>
        <item name="CTLoyaltyPrimaryTextColor">@color/loyaltyPrimaryTextColor</item>
        <item name="CTLoyaltySecondaryColor">@color/loyaltySecondaryColor</item>
        <item name="CTLoyaltySecondaryColorText">@color/loyaltySecondaryColorText</item>
        <item name="CTLoyaltyTheme">dark</item> //Either "dark" or "light" Default is "light"
   </style>
```   

