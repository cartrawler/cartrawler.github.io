---
title: Optional SDK Properties
position: 5
type: Android
description:
right_code: >-
---

The SDK has some optional properties that can be passed in initialisation to use and/or display certain features

  ```kotlin
 CartrawlerSDK.Builder()
            .enableCustomCashTreatment()
            .setUSPDisplayType(USPDisplayType.CHECK_STYLE)
  ```

<dl>
<dt>enableCustomCashTreatment</dt>
<dd>This is used in the SDK in order to enable enhanced cash voucher merchandising throughout the booking flow. (Cash voucher banner, Cash voucher widgets etc)</dd>
<dt>setUSPDisplayType</dt>
<dd>This allows Partners to choose which homepage USP style they want to use in the app.</dd>
<dd>USPDisplayType.DEFAULT_STYLE</dd>
<dd>USPDisplayType.CHECK_STYLE</dd>
<dd>Note: If you don't call this method DEFAULT_STYLE will be used.</dd>
</dl>
<br/>
