---
title: Styling Theme
position: 9
type: Android
description:
right_code: >

  {: title="Styling Theme" }
---

The SDK uses the Material Day Night theme ```Theme.MaterialComponents.DayNight```

Before proceeding, if you were using version 10.x.x and below we recommend you read the theme migration guide.

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

#### Optional colour overrides
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


