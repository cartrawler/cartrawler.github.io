---
layout: default
title: Settings Menu
parent: Customisation
grand_parent: Android Integration
nav_order: 8
permalink: /docs/android/customisation/settings/
---

# Settings Menu
{: .no_toc }

Customization of the landing and search screen toolbar settings menu button. 

---

## Setting an icon 
We have provided three icons to chose from(cog, hamburger and user): <br />
![COG](/uploads/cog.png) ![](/uploads/hamburger.png) ![](/uploads/user.png)


The SDK builder's `withSettingsMenuIconType` allows Partners to initialise the SDK with a specific icon. 
It is optional and the default icon is the `COG` 

To set your preferred icon, please use the `SettingsMenuIcon` enum class. 
It contain the values `HAMBURGER`, `USER`, `COG`. 

```kotlin
CartrawlerSDK
    .Builder()
    .withSettingsMenuIconType(SettingsMenuIcon.`CHOSEN_ICON`)
``` 
