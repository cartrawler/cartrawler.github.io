---
title: Theme Setup
position: 2
type: 
description: 'Customising the CarTrawler SDK'
right_code: >- 
---  
Theming is achieved by creating the CTStyle (iOS) and CTDayNightTheme (Android) objects and initialising the SDK with it.

<h4>iOS Theme Setup</h4>

The style objects have the following properties:
<br/>
theme
<br>primaryColor
<br>primaryLightColor (optional)
<br>primaryDarkColor (optional)<br>ctaColor (optional)
<br>ctaFontColor (optional)
<br>secondaryCtaColor (optional)
<br>secondaryCtaFontColor (optional)

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

<h4>Android Theme Setup</h4>

The Android SDK uses the Material Day Night theme ```Theme.MaterialComponents.DayNight```

Before proceeding, if you were using version 10.x.x and below we recommend you read the <a href="https://cartrawler.github.io/#section_androidtheme_migration" target="_blank">theme migration guide</a> on how to migrate your existing theme.

The following code snippet will show you how to style the app to suit your brand guidelines.

```xml
    <style name="SampleTheme" parent="CTDayNightTheme">
        <!-- The primary colour -->
        <item name="colorPrimary">@color/primaryColor</item>
        <!-- Colour applied on primary -->
        <item name="colorOnPrimary">@color/textColor</item>
        <!-- The primary variant colour (light or dark) -->
        <item name="colorPrimaryVariant">@color/primaryVariant</item>
        <!-- The status bar colour -->
        <item name="android:statusBarColor">@color/statusBar</item>
        <!-- The secondary colour -->
        <item name="colorSecondary">@color/secondaryColor</item>
        <!-- Colour applied on secondary (used for CTA buttons) -->
        <item name="colorOnSecondary">@color/textColor</item>
        <!-- Colour applied for selection states, toggles etc.. -->
        <!-- You can style it to colorSecondary if doesn’t apply to your case -->
        <item name="colorSecondaryVariant">@color/secondaryVariant</item>
    </style>
```

Once you completed styling your theme you can initialise the theme in the SDK builder as follows:

```java

  CartrawlerSDK.Builder()
  //..
  .setTheme(R.style.SampleTheme)
  .start***()
  
```

<h5>Optional colour overrides</h5>
<br>
You can override the following SDK attribute defaults if you need to.

```xml
        <!--Default Colours-->

        <!-- We recommend you keep the defaults -->
        <item name="android:windowBackground">@color/CT_White</item>
        <item name="android:colorBackground">@color/CT_Grey_Light</item>
        <item name="colorError">@color/CT_Red</item>
        <item name="colorOnError">@color/CT_Red</item>
        <item name="colorSurface">@color/CT_White</item>
        <item name="colorOnSurface">@color/CT_Black</item>
        <item name="colorOnBackground">@color/CT_Black</item>

        <!-- Customisable colours -->

        <!-- This is the attribute for text link colour -->
        <item name="CTTextLinkColor">@color/CT_Blue</item>
        <!-- This is the attribute for the horizontal progress bar on the availability screen. -->
        <!-- If you have a white primary colour, we recommend you override this in the theme and use your secondaryColor -->
        <item name="CTProgressBarColor">?colorPrimaryVariant</item>
        <!-- This is the attribute for the car illustrations on landing and booking confirmation screens -->
        <item name="CTIllustrationColor">?colorPrimaryVariant</item>
```

<br/>

<h4>Note</h4>

By Default, the colour for text and icons on colour backgrounds are white for native SDK. If your brand colours are a bright colour (high luminosity), it's recommended you change to a dark text theme for legibility of text and colours.
<picture>
  <source media="(max-width: 799px)" srcset="/uploads/theming-example.png">
  <source media="(min-width: 800px)" srcset="/uploads/theming-example.png">
  <img src="/uploads/theming-example.png">
</picture>

If you do not wish to set the four optional properties manually, the CarTrawler SDK will create the colours for you (Note: this is currently only an iOS feature within the SDK)
