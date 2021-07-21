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
<dd>This used in the SDK as a main toggle to display some features related to the Cash Special Offer</dd>
<dt>setUSPDisplayType</dt>
<dd>Partners can choose which USP style they want to use in their apps</dd>
<dd>USPDisplayType.DEFAULT_STYLE</dd>
<dd>USPDisplayType.CHECK_STYLE</dd>
<dd>Note: If you don't call this method DEFAULT_STYLE will be used by default</dd>
</dl>
<br/>
