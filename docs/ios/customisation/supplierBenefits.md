---
layout: default
title: Supplier Benefits
parent: Customisation
grand_parent: iOS
nav_order: 7
permalink: /docs/ios/customisation/supplier-benefits/
---

# Supplier Benefits

{: .no_toc }

On the vehicles list a banner may be shown to the user to allow them to apply some promo/discount
codes. This banner can be customized as you wish as described below:

---

![](/uploads/supplier_benefits_banner.png)

---

## Applying the theme

Create a `CTSupplierBenefitsStyle` object and set the theme properties:
```java
let supplierBenefitsStyle = CTSupplierBenefitsStyle()

supplierBenefitsStyle.bgColor = UIColor(hex: 0x4BA4B5) // Banner background color
supplierBenefitsStyle.textColor = UIColor(hex: 0xffffff) // Banner text color
supplierBenefitsStyle.buttonBgColor = UIColor(hex: 0x004958) // Banner button background color
supplierBenefitsStyle.buttonTextColor = UIColor(hex: 0x1DAF90) // Banner button text color

// Dark Mode colors
supplierBenefitsStyle.darkBgColor = UIColor(hex: 0xffffff) // Banner dark mode background color
supplierBenefitsStyle.darkTextColor = UIColor(hex: 0x000000) // Banner dark mode text color
supplierBenefitsStyle.darkButtonBgColor = UIColor(hex: 0x002c52) // Banner dark mode button background color
supplierBenefitsStyle.darkButtonTextColor = UIColor(hex: 0x1DAF90) // Banner dark mode button text color

style.supplierBenefitsStyle = supplierBenefitsStyle
```
Note: 
* If dark colors are not set, default `CTSupplierBenefitsStyle` colors will be used.

--- 

#### Supplier Benift Banner logo

For the logo url you can provide an URL. All you need to do is use the following attributes:

```java
supplierBenefitsStyle.logoURL = URL(string: "http://www.cartrawler.com/logo.png") // Supplier benefit banner logo

 // Dark Mode logo
supplierBenefitsStyle.darkLogoURL = URL(string: "http://www.cartrawler.com/dark_logo.png") // Supplier benefit dark mode banner logo
```
Note:
* If dark logo is not set, default `CTSupplierBenefitsStyle` logo will be used.
* If you don't provide any attributes for the logo, the component that shows the logo will be hidden.

---

## Fallback Colors

By default our CTStyle will use the following colors if partner doesn't initialise them

```java
supplierBenefitsStyle.bgColor -> CTStyle.primaryColor // CTStyle primary color
supplierBenefitsStyle.textColor -> CTStyle.themeColor // CTStyle text color. Light mode = white; Dark mode = black;
supplierBenefitsStyle.buttonBgColor -> CTStyle.ctaColor // CTStyle CTA color
supplierBenefitsStyle.buttonTextColor -> CTSTyle.ctaFontColor // CTStyle CTA font color
```

---

After `CTStyle` has been configured with your supplierBenefitsStyle, pass it into the SDK during initialisation:

#### SDK Initialisation with CTSupplierBenefitsStyle
```java
   style.supplierBenefitsStyle = supplierBenefitsStyle

   CarTrawlerSDK.sharedInstance().initialiseSDK(with: style,
                                 customParameters: nil,
                                 production: false)
```

---

## Enabling optional flag in SDK initialisation

<br/>
<h4><b>supplierBenefitAutoApplied</b></h4>
This allows Partners to initialise the SDK and opt in to apply ALL automatic codes that can be
applied for suppliers.

```java
let context = CTContext(clientID: "your clientID", flow: .standAlone)
context.supplierBenefitAutoApplied = true
CarTrawlerSDK.sharedInstance().present(from: self, context: context)
```