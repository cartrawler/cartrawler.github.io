---
layout: default
title: USPs (Unique Selling Point)
parent: Customisation
grand_parent: iOS
nav_order: 4
permalink: /docs/ios/customisation/usp
---

# USPs

{: .no_toc }

The unique selling points on the landing page have certain elements that can be customised. 

---

![](/uploads/usp_banner.svg)

### Set Up

First, create an instance of `CTStyle`. 

```java
let style = CTStyle(theme: .dark,  // .dark or .light
    primaryColor: UIColor.gray)
``` 
---


### Logo

#### Logo setup
```java
style.landingPageLogoImage = UIImage(named: "usp_logo") // USP Logo

// To use NSURL property, UIImage property must not be set
cashStyle.landingPageLogoURL = URL(string: "http://www.cartrawler.com/usp_logo.png") // USP Logo 

// Dark Mode logo
cashStyle.dmLandingPageLogoImage = UIImage(named: "dark_usp_logo") // USP Logo

// To use NSURL property, UIImage property must not be set
cashStyle.dmLandingPageLogoURL = URL(string: "http://www.cartrawler.com/dark_usp_logo.png") // USP Logo
```
Notes: 
* If logo is provided as `UIImage`, `NSURL` property will be ignored.
* If dark logo is not set, default landing page logo will be used.
* If you don't provide any attributes for the logo, the component that shows the logo will be hidden. 

---

### USP Style

The USP icons can be replaced with checkmarks by setting the `landingPageStyle`. 

#### Checkmark USP Sample code 
```java
style.landingPageStyle = .checkmark // default is .default
```

---

### Pass CTStyle instance into the SDK 

After configuring the `CTStyle` object, pass it into the SDK during initialisation:

#### SDK Initialisation with CTStyle Sample code
```java
CarTrawlerSDK.sharedInstance().initialiseSDK(with: style,
                              customParameters: nil,
                              production: false)
```