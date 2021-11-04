---
title: Recent Searches API
position: 5
type:
description: Recent Searches API 
---

We expose the recent searches in the SDK via our API repositories. If you decide to remove a recent search using our repositories this will be reflected in the SDK as well.<br/><br/>

### Recent Searches - iOS

<b><u>Fetching Recent Searches</u></b><br/>
Call the requestRecentSearches function to fetch the three most recent searches a user has made within the SDK.<br/> These will be exposed outside the SDK in a callback as an array of CTRecentSearch objects as shown below: 

```swift
CarTrawlerSDK.sharedInstance().requestRecentSearches { searches, error in
    if let recentSearches: [CTRecentSearch] = searches {
        // recent searches fetched successfully
    }
    else if let error = error {
        // error occurred 
    }
}
```
<br/><b><u>Removing Recent Searches</u></b><br/>
To remove a recent search, pass it into the remove function: 

```swift
CarTrawlerSDK.sharedInstance().remove(search) { (success, error) in
    if success {
        // search successfully removed 
    } else if let error = error {
        // error occured
    }
}
```
<br/><b><u>Clearing Recent Searches</u></b><br/>
To remove all searches: 

```swift
CarTrawlerSDK.sharedInstance().removeAllRecentSearches()
```

<br/><b><u>Building a recent searches UI</u></b> <br/>
To build a UI similar to what is shown on the SDK landing page, we suggest displaying the searches in a table view using the following properties of the CTRecentSearch class

```swift
pickupLocationName
derivedPickupDate
derivedDropoffDate
```
```swift
let dateFormatter = DateFormatter()
dateFormatter.dateFormat = "dd-MM-yyyy HH:mm"
let dateString = "\(dateFormatter.string(from: recentSearch.derivedPickupDate)) - \(dateFormatter.string(from: recentSearch.derivedPickupDate))"
```

<br/><br/>



### Recent Searches - Android

<b><u>Setup</u></b><br/>
Initialise our ``CarTrawlerDatabaseRepository`` class as follows:

````kotlin
// You can use whatever thread you want for your use case
val executorService = Executors.newSingleThreadExecutor() 
val carTrawlerDatabase = CarTrawlerDatabase.build(applicationContext)
val repository = CarTrawlerDatabaseRepository(carTrawlerDatabase, executorService)
````

<br/><b><u>Fetching Recent Searches</u></b><br/>
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

<br/><b><u>Removing Recent Searches</u></b><br/>
To remove a recent search you must pass in the created date of the recent search item
````kotlin
val repository = CarTrawlerDatabaseRepository(carTrawlerDatabase, executorService)

repository.removeRecentSearch(recentSearch.createdDate)
````

<br/><b><u>Clearing Recent Searches</u></b><br/>
To remove all recent searches call the function as follows:
````kotlin
val repository = CarTrawlerDatabaseRepository(carTrawlerDatabase, executorService)

repository.removeAllRecentSearches()
````