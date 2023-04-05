---
layout: default
title: Settings
parent: Customisation
grand_parent: iOS Integration
nav_order: 8
permalink: /docs/ios/customisation/settings/
---

# Settings
{: .no_toc }

On the landing and search pages, there is a button on the top right of the navigation bar that will open a settings page when tapped. 

---

## Selecting an icon 
We have provided three icons to chose from: <br />
![](/uploads/cog.png) ![](/uploads/hamburger.png) ![](/uploads/user.png)

To set your preferred icon, please use the `CTContext` class's `settingsIconType` property:

{: .important }
The `implementationID` is needed by the SDK to fetch partner specific configuration.<br/>

```java
import CarTrawlerSDK
let context = CTContext(implementationID: "your implementation ID", 
                        clientID: "your client ID", 
                        flow: .standAlone)
context.countryCode = "IE" 
context.currencyCode = "EUR" 
context.languageCode = "EN"

context.settingsIconType = .cog // or .hamburger, or .user
```   

{: .note}
This customisation is only available as part of the Standalone flow. 

