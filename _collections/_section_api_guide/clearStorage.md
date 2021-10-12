---
title: Clear Storage
position: 6
type: 
description:
right_code: >-
---

You may want to tear the local storage of the SDK based on your use cases. 

### Clear Storage - iOS
iOS SDK provides a method to clear local data stored (Recent searches, Booking summary card), the data is stored in NSUserDefaults and it's cleared using the method below.

<b>It's not possible to clear the storage while the SDK is being presented on screen.</b>

```swift
  // Clear local storage
  CarTrawlerSDK.sharedInstance().clearStorage { (error) in
    if let error = error {
        print("Storage not cleared with error:" + error.localizedDescription)
    } else {
        print("Storage cleared with success")
    }
  }
```

### Clear Storage - Android

The Android SDK's storage (database of recent searches, and bookings) can be cleared prior to starting the cartrawler flow, the following API can be used to clear the storage.

Initialise our ``CarTrawlerDatabaseRepository`` class as follows:

````kotlin
    // You can use whatever thread you want for your use case
    val executorService = Executors.newSingleThreadExecutor() 
    val carTrawlerDatabase = CarTrawlerDatabase.build(applicationContext)
    val repository = CarTrawlerDatabaseRepository(carTrawlerDatabase, executorService)
````

Call the clear storage function as follows:
````kotlin
    // You can use whatever thread you want for your use case
    val repository = CarTrawlerDatabaseRepository(carTrawlerDatabase, executorService)

    repository.clearStorage()
````
