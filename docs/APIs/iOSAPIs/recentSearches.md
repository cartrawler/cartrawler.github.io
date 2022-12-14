---
layout: default
title: Recent Searches
parent: iOS SDK APIs
grand_parent: APIs
nav_order: 4
permalink: /docs/api/ios/recent-searches
---

# Recent Searches
{: .no_toc }

We expose recent searches made within the SDK via the `requestRecentSearches` function. Recent Searches can also be cleared or removed individually, and this will be reflected in the SDK as well.

<details open markdown="block">
  <summary>
    Table of Contents
  </summary>
  {: .text-delta }
1. TOC
{:toc}
</details>

---

## Fetching Recent Searches

Call the `requestRecentSearches` function to fetch the three most recent searches a user has made within the SDK.<br/> These will be exposed outside the SDK in a callback as an array of `CTRecentSearch` objects as shown below: 

```java
CarTrawlerSDK.sharedInstance().requestRecentSearches { searches, error in
    if let recentSearches: [CTRecentSearch] = searches {
        // recent searches fetched successfully
    }
    else if let error = error {
        // error occurred 
    }
}
```
---

## Removing Recent Searches

To remove a recent search, pass it into the remove function: 

```java
CarTrawlerSDK.sharedInstance().remove(search) { (success, error) in
    if success {
        // search successfully removed 
    } else if let error = error {
        // error occured
    }
}
```
---

## Clearing Recent Searches

To remove all recent searches at once, use the removeAllRecentSearchesFunction:

```java
CarTrawlerSDK.sharedInstance().removeAllRecentSearches()
```
---

## Building a Recent Searches UI

To build a UI similar to what is shown on the SDK landing page, we suggest displaying the searches in a table view using the following properties of the CTRecentSearch class: <br />
`pickupLocationName`, `derivedPickupDate`, and `derivedDropoffDate`.

For example: 

```java
let dateFormatter = DateFormatter()
dateFormatter.dateFormat = "E d MMM, h:mm"
let dateString = "\(dateFormatter.string(from: recentSearch.derivedPickupDate)) - \(dateFormatter.string(from: recentSearch.derivedDropoffDate))"
```