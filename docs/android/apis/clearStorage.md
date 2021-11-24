---
layout: default
title: Clear Storage
parent: Android SDK APIs
grand_parent: Android
nav_order: 3
permalink: /docs/android/apis/clear-storage/
---

# Clear Storage

{: .no_toc }

The Recent Searches and Bookings shown on the Landing Page are stored are stored locally on device. This locally stored data can be cleared prior to starting a CarTrawler flow. 

---

<b>Note: It's not possible to clear the storage while the SDK is being presented on screen.</b>

Initialise our ``CarTrawlerDatabaseRepository`` class as follows:

```java
// You can use whatever thread you want for your use case
val executorService = Executors.newSingleThreadExecutor() 
val carTrawlerDatabase = CarTrawlerDatabase.build(applicationContext)
val repository = CarTrawlerDatabaseRepository(carTrawlerDatabase, executorService)
````

Call the `clearStorage` function as follows:
```java
// You can use whatever thread you want for your use case
val repository = CarTrawlerDatabaseRepository(carTrawlerDatabase, executorService)

repository.clearStorage()
````
