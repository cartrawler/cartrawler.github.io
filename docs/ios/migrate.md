---
layout: default
title: Migrate to 14.0.0
parent: iOS Integration
nav_order: 8
permalink: /docs/ios/migrate/
---

# Migrate from 13.2 and below to 14.0.0
{: .no_toc }

## Common 

{: .important }
The `implementationID` is needed by the SDK since it's used to fetch some configuration.<br/>

On SDK 13.2 and below we used to initialise the `CTContext` object with the method `CTContext(clientID:flow:)` this method has been marked as unavailable and it's not possible to use it on version 14.0.0.

Now to present the SDK it's needed to add the parameter `implementationID` to the `CTContext` initialisation using the method `CTContext(implementationID:clientID:flow:)`:<br/>

## Previous CTContext initialisation 
CTContext initialisation on versions below 14.0.0:
{: .warning }
```swift
let context = CTContext(clientID: "your client ID", 
                        flow: .standAlone or .inPath)
```

## New CTContext initialisation 
CTContext initialisation from version 14.0.0:
{: .new }
```swift
let context = CTContext(implementationID: "your implementation ID", 
                        clientID: "your client ID", 
                        flow: .standAlone or .inPath)
```