---
layout: default
title: Recent Searches
parent: Android SDK APIs
grand_parent: APIs
nav_order: 4
permalink: /docs/api/android/recent-searches/
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

Initialise our `CTRecentSearchRepository` class as follows:

```kotlin
val context = this.applicationContext
val executor = Executors.newSingleThreadExecutor()
val repository = CTRecentSearchRepository(context, executor)
```

---

## Fetching Recent Searches 
To fetch the top 3 recent searches we will expose the result in a callback as follows:

```kotlin
repository.recentSearches(object : CTRecentSearchListener {
    override fun onSuccess(list: List<CTRecentSearchData>) {
        /* do what you need here */
    }

    override fun onError(message: String) {
        /* do what you need here */
    }
})
```
---

## Removing Recent Searches

To remove a recent search you must pass in the created date of the recent search item

```kotlin
repository.removeRecentSearch(createdAt = <created_date_here>)
```

---

## Clearing Recent Searches

To remove all recent searches call the function as follows:

```kotlin
repository.clearRecentSearches()
```
