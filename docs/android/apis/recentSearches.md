---
layout: default
title: Recent Searches
parent: Android SDK APIs
grand_parent: Android
nav_order: 4
permalink: /docs/android/apis/recent-searches/
---

# Recent Searches
{: .no_toc }

We expose recent searches made within the SDK via our recentSearches API. Recent searches can also be cleared or removed individually, and this will be reflected in the SDK as well.

<details open markdown="block">
  <summary>
    Table of Contents
  </summary>
  {: .text-delta }
1. TOC
{:toc}
</details>

---

## Setup

Initialise our ``CarTrawlerDatabaseRepository`` class as follows:

```java
// You can use whatever thread you want for your use case
val executorService = Executors.newSingleThreadExecutor() 
val carTrawlerDatabase = CarTrawlerDatabase.build(applicationContext)
val repository = CarTrawlerDatabaseRepository(carTrawlerDatabase, executorService)
````
---

## Fetching Recent Searches 
To fetch the top 3 recent searches we will expose the result in a callback as follows:

```java
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
---

## Removing Recent Searches

To remove a recent search you must pass in the created date of the recent search item

```java
val repository = CarTrawlerDatabaseRepository(carTrawlerDatabase, executorService)

repository.removeRecentSearch(recentSearch.createdDate)
````

---

## Clearing Recent Searches

To remove all recent searches call the function as follows:

```java
val repository = CarTrawlerDatabaseRepository(carTrawlerDatabase, executorService)

repository.removeAllRecentSearches()
````
