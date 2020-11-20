---
title: Theme Migration
position: 9
type: Android
description:
right_code: >

  {: title="Material Theme Migration" }
---

Supporting documentation for outlining the steps for migrating to v11.x.x for the styling of the material theme. 

If you haven’t already migrated to Material Theme we recommend that you read the [material documentation](https://material.io/develop/android/docs/getting-started) on how to get started. 
<br/>


Instead of inheriting ```CTDarkTheme```  , ```CTLightDarkTheme``` or ```CTAppTheme``` please migrate to the new ```CTDayNightTheme```.

```xml

    <style name="SampleTheme" parent="CTDayNightTheme">
        //..
    </style>

```

<h4>New material attributes</h4>
<br/>
The SDK theme uses the material theme attributes to style the SDK to suit your branding requirements.

You can then start mapping the old attributes to new ones as follows:

| Attribute name                   	| New Attribute                                                                   	|
|-----------------------------	|-------------------------------------------------------------------------	|
| colorPrimary       	         | Not changed 	|
| CTPrimaryColor    	      | colorPrimary	|
| colorPrimaryDark     	         |  colorPrimaryVariant (it can be your light or dark variant)	|
| CTPrimaryDarkColor 	         |  colorPrimaryVariant	|
| colorAccent 	         |  colorSecondary	|
| CTCTAAccentColor 	         |  colorSecondary	|
| CTPrimaryLightColor 	         |  colorPrimaryVariant (it can be your light or dark variant)	|


New attributes usage:

| Attribute name                 	| Description                                                                  	|
|-----------------------------	|-------------------------------------------------------------------------	|
| colorPrimary       	         | Colour applied on top of primary colour 	|
| colorOnPrimary    	      | Colour applied on top of primary colour	|
| colorSecondaryVariant     	         |  Colour for calendar selection state, car category selection state, search chip choice state and filter button colour. 	|
| colorSecondary 	         |  Colour used for CTA buttons. 	|
| colorOnSecondary 	         |  Colour applied on top of secondary colour	|
| android:statusBarColor 	         |  Colour used to style the Android status bar	|

