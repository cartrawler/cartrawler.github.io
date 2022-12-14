---
layout: default
title: Clear Storage
parent: iOS SDK APIs
grand_parent: APIs
nav_order: 3
permalink: /docs/sdk/ios/clear-storage
---

# Clear Storage

{: .no_toc }

The Recent Searches and Bookings shown on the Landing Page are stored within User Defaults. To remove this locally stored data, use the `clearStorage` function.

---

```java
// Clear local storage
CarTrawlerSDK.sharedInstance().clearStorage { (error) in
  if let error = error {
      print("Storage not cleared with error:" + error.localizedDescription)
  } else {
      print("Storage cleared with success")
  }
}
```

{: .note}
It's not possible to clear the storage while the SDK is being presented on screen.</b>