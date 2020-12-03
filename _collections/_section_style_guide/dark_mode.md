---
title: Dark Mode
position: 3
type: 
description: 'Customising the CarTrawler SDK'
right_code: >- 
---  

The SDKs both support dark mode and by default it will be turned off. The SDKs provide you the option to turn on, off or follow the system settings. Apply the setting that fits your use case.

<h4>iOS Dark Mode Setup</h4>
You can configure dark mode support in the SDK by setting the property <b>userInterfaceStyle</b> in the CTStyle class. By default, dark mode is off.

The following values are supported:

```swift
    .dark // Forces to be always dark mode
    .light // Default - Forces to be always light mode
    .system // Use user system settings to determine if is light/dark mode
```

Dark mode colors can be configured using CTStyle and dark mode properties:

```swift
    let style = CTStyle(theme: .dark,  // .dark or .light
                        primaryColor: UIColor.gray)
    style.userInterfaceStyle = .system
    style.dmPrimaryLightColor = UIColor.lightGray
    style.dmPrimaryDarkColor = UIColor.darkGray
    style.dmCtaColor = UIColor.blue
    style.dmCtaFontColor = UIColor.white
    style.dmSecondaryCtaColor = UIColor.black
    style.dmSecondaryCtaFontColor = UIColor.white
```

After CTStyle configured, it must be passed on AppDelegate initialise call:

```swift
  // In application(_:didFinishLaunchingWithOptions:)
  CarTrawlerSDK.sharedInstance().initialiseSDK(with: style,
                                 customParameters: nil,
                                 production: false)
```

<h4>Android Dark Mode Setup</h4>
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

<h5>How to style your dark mode theme</h5>
<br>
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