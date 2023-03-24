---
layout: default
title: Settings 
parent: Customisation
grand_parent: Android Integration
nav_order: 8
permalink: /docs/android/customisation/settings/
---

# Settings 
{: .no_toc }

On the landing and search pages, there is a button on the top right of the navigation bar that will open a settings page when tapped. 

---

## Selecting an icon 
We have provided three icons to chose from: <br />
![](/uploads/cog.png) ![](/uploads/hamburger.png) ![](/uploads/user.png)

To set your preferred icon, please use the `SettingsMenuIcon` enum class. 
It contain the values `HAMBURGER`, `USER`, `COG`. 

```kotlin
val sdkDataClientIdXYZ = CTSdkData.Builder(clientId = clientId)
    .withSettingsMenuIconType(SettingsMenuIcon.`CHOSEN_ICON`)
``` 
