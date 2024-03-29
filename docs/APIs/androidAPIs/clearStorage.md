---
layout: default
title: Clear Storage
parent: Android SDK APIs
grand_parent: APIs
nav_order: 3
permalink: /docs/api/android/clear-storage/
---

# Clear Storage

{: .no_toc }

The Recent Searches and Bookings shown on the Landing Page are stored are stored locally on device. This locally stored data can be cleared prior to starting a CarTrawler flow. 

---

Initialise our `CTRecentSearchRepository` class as follows:

```kotlin
val context = this.applicationContext
val executor = Executors.newSingleThreadExecutor()
val repository = CTRecentSearchRepository(context, executor)
```

Call the `clearStorage` function as follows:
```kotlin
repository.clearStorage()
```

{: .note}
It's not possible to clear the storage while the SDK is being presented on screen.