---
layout: default
title: Dark Mode
parent: Customisation
grand_parent: iOS
nav_order: 2
permalink: /docs/ios/customisation/dark-mode
---

# Dark Mode

{: .no_toc }

The SDKs both support dark mode and by default it will be turned off. The SDKs provide you the option to turn on, off or follow the system settings. Apply the setting that fits your use case.

---

## Set up

You can configure dark mode support in the SDK by setting the property <b>userInterfaceStyle</b> in the CTStyle class. By default, dark mode is off.

The following values are supported:

```java
    .dark // Forces to be always dark mode
    .light // Default - Forces to be always light mode
    .system // Use user system settings to determine if is light/dark mode
```

Dark mode colors can be configured using CTStyle and dark mode properties:

```java
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

After configuring the CTStyle object, pass it into the SDK during initialisation:

```java
// In application(_:didFinishLaunchingWithOptions:)
    CarTrawlerSDK.sharedInstance().initialiseSDK(with: style,
                                customParameters: nil,
                                production: false)
```
