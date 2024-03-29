---
layout: default
title: USPs (Unique Selling Point)
parent: Customisation
grand_parent: Android Integration
nav_order: 5
permalink: /docs/android/customisation/usp
---

# USPs
{: .no_toc }

The unique selling points on the landing page have certain elements that can be customised. 

<details open markdown="block">
  <summary>
    Table of Contents
  </summary>
  {: .text-delta }
1. TOC
{:toc}
</details>

---

![](/uploads/usp_banner.svg)

## Set Up

Override the following properties in your SDK theme
```xml
<style name="CarTrawlerSDKTheme" parent="CTDayNightTheme" >
     //..
     <item name="ctUSPLogoUrl">https://www.mylogo.com/mylogo.png</item>
</style>
```   
---

## Logo

For every logo url you can provide a local drawable instead of a remote url. All you need is to
use the following attributes:

```xml
<style name="CarTrawlerSDKTheme" parent="CTDayNightTheme" >
    <item name="ctUSPLogoDrawable">@drawable/my_logo_usp</item>
</style>
```

{: .note}
<small>If you provide both a <b>drawable</b> and an <b>url</b> the drawable takes priority, so your url will never be used. <br/>
If you don't provide any attributes for the logo, the component that shows the logo will be hidden (`View.GONE`) </small>