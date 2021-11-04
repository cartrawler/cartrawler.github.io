---
title: Optional SDK Properties
position: 4
type: Android
description:
right_code: >
---

The SDK has some optional properties that can be passed in initialisation to use and/or display certain features

  ```kotlin
 CartrawlerSDK.Builder()
            .enableCustomCashTreatment()
            .setUSPDisplayType(USPDisplayType.CHECK_STYLE)
            .addPromotionCode(PromotionCodeType.WithCodeType("your-promotion-code-here")
  ```

<h4><b>enableCustomCashTreatment</b></h4>
This is used in the SDK in order to enable enhanced cash voucher merchandising throughout the booking flow. (Cash voucher banner, Cash voucher widgets etc)
<br/><br/>

<h4><b>setUSPDisplayType</b></h4>
This allows Partners to choose which homepage USP style they want to use in the app.
Note: If you don't call this method DEFAULT_STYLE will be used.
<br/>

| Type                 	| Description                                                                  	|
|-----------------------------	|-------------------------------------------------------------------------	|
| USPDisplayType.DEFAULT_STYLE  | This will be the default USP display type if you don't specify any|
| USPDisplayType.CHECK_STYLE    | This will display display another layout for the USP which can have <br/>a logo and will display checks instead of an icon	|

<br/>
<h4><b>addPromotionCode</b></h4>
This allows Partners to pass a promotion code type to the SDK as the main toggle to display promotion code field on the search form or not.
The following options can be used:

| Type                 	| Description                                                                  	|
|-----------------------------	|-------------------------------------------------------------------------	|
| PromotionCodeType.UseInAppInputType       	      | This will display an input field that will allow the user to input/paste a promotion code that will be sent to Availability Request |
| PromotionCodeType.WithCodeType("code")    	      | This will send the promotion code provided in the Availability Request. The input field will also be shown to the user in the search form	|

Note: If ```addPromotionCode``` method is not called with any of the options described above the promotion code field will not be shown in the search form screen.