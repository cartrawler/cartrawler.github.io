---
title: USP (Unique Selling Point)
position: 6
type:
description:
right_code: >-
---

See Graphics below for descriptions on which style applies to which widget:

<picture>
  <img style="max-width: 760px; max-height:462px;" src="/uploads/usp_banner.svg">
</picture>


### Applying the Theme - Android
Override the following properties in your SDK theme
```xml
   <style name="CarTrawlerSDKTheme" parent="CTDayNightTheme" >
        //..
        <item name="ctUSPLogoUrl">https://www.mylogo.com/mylogo.png</item>
   </style>
```   

For every logo url you can provide a local drawable instead of a remote url. All you need is to
use the following attributes:

```xml
    <style name="CarTrawlerSDKTheme" parent="CTDayNightTheme" >
        <item name="ctUSPLogoDrawable">@drawable/my_logo_usp</item>
    </style>
```

Notes: 
* If you provide both a <b>drawable</b> and an <b>url</b> the drawable takes priority, so your url will never be used
* If you don't provide any attributes for the logo, the component that shows the logo will be hidden (View.GONE) 

<br/>
<br/>

### Applying the Theme - iOS
<br/>
<b>Create an instance of CTStyle object to receive the theme properties, CTStyle full description can be found <a href="https://cartrawler.github.io/#section_style_guidetheming">here</a></b>
```swift
   let style = CTStyle(theme: .dark,  // .dark or .light
             primaryColor: UIColor.gray)
``` 
<br/>
<br/>
<b>To set the landing page logo:</b>
```swift
  style.landingPageLogoImage = UIImage(named: "usp_logo") // USP Logo

  // To use NSURL property, UIImage property must not be set
  cashStyle.landingPageLogoURL = URL(string: "http://www.cartrawler.com/usp_logo.png") // USP Logo
   
  // Dark Mode logo
  cashStyle.dmLandingPageLogoImage = UIImage(named: "dark_usp_logo") // USP Logo

  // To use NSURL property, UIImage property must not be set
  cashStyle.dmLandingPageLogoURL = URL(string: "http://www.cartrawler.com/dark_usp_logo.png") // USP Logo
```
Notes: 
* If logo is provided as UIImage, NSURL property will be ignored.
* If dark logo is not set, default landing page logo will be used.
* If you don't provide any attributes for the logo, the component that shows the logo will be hidden. 
<br/>
<br/>

<b>To set the landing page USP style as checkmark (Replacing icons by checkmark and displaying only a description for each item):</b>
```swift
  style.landingPageStyle = .checkmark // default is .default
```
<br/>
<br/>
<b>Pass you CTStyle instance to the SDK:</b>
```swift
   CarTrawlerSDK.sharedInstance().initialiseSDK(with: style,
                                 customParameters: nil,
                                 production: false)
```