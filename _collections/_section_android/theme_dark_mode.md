---
title: Dark Mode
position: 10
type: Android
description:
right_code: >

  {: title="Dark Mode Support" }
---

You can configure dark mode support in the SDK by sending the constants from the AppCompatDelegate class. By default, dark mode is off.

Code sample

```kotlin

    builder.setDarkModeConfig(AppCompatDelegate.MODE_NIGHT_ON)
    CartrawlerSDK.Builder()
        .setDarkModeConfig(AppCompatDelegate.MODE_NIGHT_ON)
        // ..

```


The following constants are supported:

```kotlin

    AppCompatDelegate.MODE_NIGHT_FOLLOW_SYSTEM
    AppCompatDelegate.MODE_NIGHT_YES
    AppCompatDelegate.MODE_NIGHT_NO

```

#### How to style your dark mode theme
<br>
The SDK uses the Theme.MaterialComponents.DayNight theme which allows us to support dark mode.

You can place the SDK theme in the ```values-night``` folder to apply your dark mode colour palette that suits your brand guidelines.

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



