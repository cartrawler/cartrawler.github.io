---
title: Usage
position: 2
type: 
description: 'Customising the CarTrawler SDK'
right_code: |-
  ```swift

  let style = CTStyle(theme: .dark,  // .dark or .light
             primaryColor: UIColor.gray)
   style.primaryLightColor = UIColor.lightGray // Optional, default light generated based on primary color
   style.primaryDarkColor = UIColor.darkGray // Optional, default dark generated based on primary color
   style.ctaColor = UIColor.blue // Optional, default iOS blue RGB(0,122,255)
   style.ctaFontColor = UIColor.white  // Optional, default white or dark based on theme
   style.secondaryCtaColor = UIColor.black // Optional, default primary color
   style.secondaryCtaFontColor = UIColor.white // Optional, default white or dark based on theme

  ```  
  {: title="iOS" }
  
  ~~~java
  //We have two themes, CTDarkTheme and CTLightTheme. 
  // You should only extend one of them and pass it to the SDK builder as shown below.

   <style name="BestAppTheme" parent="Theme.AppCompat.Light">
   // my own custom styles
   </style>

   <style name="CartTrawlerSDKTheme parent="CTDarkTheme">
        <item name="colorPrimary">@color/colorPrimary</item>
        <item name="colorPrimaryDark">@color/colorPrimaryDark</item>
        <item name="colorAccent">@color/colorAccent</item>
        <item name="CTPrimaryColor">@color/colorPrimary</item>
        <item name="CTPrimaryDarkColor">@color/colorPrimaryDark</item>
        <item name="CTPrimaryLightColor">@color/colorPrimaryLight</item>
        <item name="CTSecondaryColor">@color/colorPrimaryDark</item>
        <item name="CTAccentColor">@color/colorAccent</item>
   ... etc
   </style>

  CartrawlerSDK.Builder()
  ..
  .setTheme(R.style.CartTrawlerSDKTheme)
  .start***()
  ~~~
  {: title="Android" }
   
---  
Theming is achieved by creating the CTStyle (iOS) and CTAppTheme (Android) objects and initialising the SDK with it.

The style objects have the following properties:

iOS

theme
<br>primaryColor
<br>primaryLightColor (optional)
<br>primaryDarkColor (optional)<br>ctaColor (optional)
<br>ctaFontColor (optional)
<br>secondaryCtaColor (optional)
<br>secondaryCtaFontColor (optional)

Android
<br>primaryColor
<br>colorPrimaryDark
<br>colorAccent
<br>CTPrimaryColor
<br>CTPrimaryLightColor (optional)
<br>CTPrimaryDarkColor (optional)
<br>CTCTAColor(optional)
<br>CTCTATextColor (optional)
<br>CTSecondaryActionColor (optional)
<br>CTSecondaryActionTextColor (optional)


By Default, the colour for text and icons on colour backgrounds are white for native SDK. If your brand colours are a bright colour (high luminosity), it's recommended you change to a dark text theme for legibility of text and colours.
<picture>
  <source media="(max-width: 799px)" srcset="/uploads/theming-example.png">
  <source media="(min-width: 800px)" srcset="/uploads/theming-example.png">
  <img src="/uploads/theming-example.png">
</picture>

If you do not wish to set the four optional properties manually, the CarTrawler SDK will create the colours for you (Note: this is currently only an iOS feature within the SDK)
