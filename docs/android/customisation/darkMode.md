---
layout: default
title: Dark Mode
parent: Customisation
grand_parent: Android
nav_order: 3
permalink: /docs/android/customisation/dark-mode/
---

# Dark Mode

{: .no_toc }

The SDKs both support dark mode and by default it will be turned off. The SDKs provide you the option to turn on, off or follow the system settings. Apply the setting that fits your use case.

---

## Set up

You can configure dark mode support in the SDK by sending the constants from the AppCompatDelegate class. By default, dark mode is off.

The following constants are supported:

```java

AppCompatDelegate.MODE_NIGHT_FOLLOW_SYSTEM
AppCompatDelegate.MODE_NIGHT_YES
AppCompatDelegate.MODE_NIGHT_NO

```

Then initialise the SDK with the preferred setting:

```java
CartrawlerSDK.Builder()
    //..
    .setDarkModeConfig(AppCompatDelegate.NIGHT_FOLLOW_SYSTEM)
    .setTheme(R.style.SampleTheme)
    .start***
```
## How to Style your Dark Mode Theme

The SDK uses the Theme.MaterialComponents.DayNight theme which allows us to support dark mode.

You can place the SDK theme in your ```values-night``` folder to apply your dark mode colour palette that fits your brand requirements.

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