---
title: USP (Unique Selling Point)
position: 6
type:
description:
right_code: >-
---

### CTUSPTheme

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

See Graphics below for descriptions on which style applies to which widget:

<picture>
  <img src="/uploads/usp_banner.svg">
</picture>
