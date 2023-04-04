---
layout: default
title: Migrate to 14.0.2
parent: iOS Integration
nav_order: 8
permalink: /docs/ios/migrate/
---

# Migrate from 13.2 and below to 14.0.2
{: .no_toc }

## Common 

{: .important }
The `implementationID` is needed by the SDK to fetch partner specific configuration.<br/>

On version 13.2.0 of the SDK and below, the `CTContext` object was initialised with a `clientID` using the method `CTContext(clientID:flow:)`. This method has been marked unavailable, it is no longer possible to use it with version 14.0.0.

To present the SDK now, the `CTContext` object must be initialised with an `implementationID`, as well as a `clientID`, using the method `CTContext(implementationID:clientID:flow:)`:<br/>

## Previous CTContext initialisation 
CTContext initialisation on versions below 14.0.2:
{: .warning }
```swift
let context = CTContext(clientID: "your client ID", 
                        flow: .standAlone or .inPath)
```

## New CTContext initialisation 
CTContext initialisation from version 14.0.2:
{: .new }
```swift
let context = CTContext(implementationID: "your implementation ID", 
                        clientID: "your client ID", 
                        flow: .standAlone or .inPath)
```