---
layout: default
title: Themes
parent: Customisation
grand_parent: iOS
nav_order: 1
permalink: /docs/ios/customisation/themes
---

# Themes
{: .no_toc }

<details open markdown="block">
  <summary>
    Table of Contents
  </summary>
  {: .text-delta }
1. TOC
{:toc}
</details>

---

By default, the colour for text and icons on colour backgrounds are white. If your brand colours are a bright colour (high luminosity), it's recommended you change to a dark text theme for legibility of text and colours.
<picture>
  <source media="(max-width: 799px)" srcset="/uploads/theming-example.png">
  <source media="(min-width: 800px)" srcset="/uploads/theming-example.png">
  <img src="/uploads/theming-example.png">
</picture>

If you do not wish to set the four optional properties manually, the CarTrawler SDK will create the colours for you (Note: this is currently only an iOS feature within the SDK)

---

## Creating a CTStyle 

Theming is achieved by creating a `CTStyle` object and initialising the SDK with it.

`CTStyle` has the following properties:

* [Required] `theme` 
* [Required] `primaryColor` 
* `primaryLightColor` 
* `primaryDarkColor` 
* `ctaColor`
* `ctaFontColor`
* `secondaryCtaColor`
* `secondaryCtaFontColor` 

```java
let style = CTStyle(theme: .dark,  // .dark or .light
           primaryColor: UIColor.gray)
style.primaryLightColor = UIColor.lightGray // Optional, default light generated based on primary color
style.primaryDarkColor = UIColor.darkGray // Optional, default dark generated based on primary color
style.ctaColor = UIColor.blue // Optional, default iOS blue RGB(0,122,255)
style.ctaFontColor = UIColor.white  // Optional, default white or dark based on theme
style.secondaryCtaColor = UIColor.black // Optional, default primary color
style.secondaryCtaFontColor = UIColor.white // Optional, default white or dark based on theme
```
### Setting a Custom Font
{: .no_toc }

```java
style.regularFont = UIFont(name: "myCustomFont", size: 14)!
style.boldFont = UIFont(name: "myCustomFont", size: 14)!
style.italicFont = UIFont(name: "myCustomFont", size: 14)!
```

{: .important}
Please ensure any custom fonts used are included in your main bundle.

---

## SDK Initialisation
After CTStyle is configured, it must be passed into the SDK during initialisation in your AppDelegate:

```java
// In application(_:didFinishLaunchingWithOptions:)
CarTrawlerSDK.sharedInstance().initialiseSDK(with: style,
                               customParameters: nil,
                               production: false)
```
