---
layout: default
title: Installation
parent: Android
nav_order: 1
---

# Installation

To add the CarTrawlerSDK to your app, add our maven repository and enter your artifactory credentials.

---

## Maven & Artifactory

### Add our maven repository and the artifactory credentials

* Locate the file gradle.properties in your pc under ~/.gradle/gradle.properties. 
* If the file does not exist create one and add the following credentials to it.

```groovy

 nativeArtifactoryUsername=<artifactory-username>
 nativeArtifactoryPassword=<artifactory-password>

```
Note: If you do not have these credentials contact you manager, IT or another developer in your team.

* Next, add the CarTrawler dependency to build.gradle. Please use the version number sent to you by the CarTrawler team

```groovy     

implementation "com.cartrawler.android:car-rental:$latestVersion" 

```

Create a theme that extends the ```CTDayNightTheme```. Please refer to the <a href="/docs/android/customisation/themes" target="_blank">theme section</a> for further details.

## App Permissions

On the SDK’s Search Screen, we have added the option for users to search for vehicles using their current location upon tapping the pick-up location EditText.

To make use of this feature, please ensure your Manifest has either the `ACCESS_COARSE_LOCATION` or `ACCESS_FINE_LOCATION` permission.

We will need to use this when asking for permission to use location services within your app if it has not already been granted.