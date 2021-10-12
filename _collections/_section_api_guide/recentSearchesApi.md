---
title: Recent Searches API
position: 5
type:
description: Recent Searches API 
---

We expose the recent searches in the SDK via our API repositories. If you decide to remove a recent search using our repositories this will be reflected in the SDK as well.

### Recent Searches - iOS
//TODO iOS

### Recent Searches - Android

Initialise our ``CarTrawlerDatabaseRepository`` class as follows:

````kotlin
// You can use whatever thread you want for your use case
val executorService = Executors.newSingleThreadExecutor() 
val carTrawlerDatabase = CarTrawlerDatabase.build(applicationContext)
val repository = CarTrawlerDatabaseRepository(carTrawlerDatabase, executorService)
````

To fetch the top 3 recent searches we will expose the result in a callback as follows:

````kotlin
val repository = CarTrawlerDatabaseRepository(carTrawlerDatabase, executorService)

repository.recentSearches(object : RecentSearchesCallback {
    override fun onSuccess(list: List<RecentSearch>) {
        //..
    }

    override fun onError(message: String) {
        //..
    }
})
````

To remove a recent search you must pass in the created date of the recent search item
````kotlin
val repository = CarTrawlerDatabaseRepository(carTrawlerDatabase, executorService)

repository.removeRecentSearch(recentSearch.createdDate)
````

To remove all recent searches call the function as follows:
````kotlin
val repository = CarTrawlerDatabaseRepository(carTrawlerDatabase, executorService)

repository.removeAllRecentSearches()
````