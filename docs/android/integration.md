---
layout: default
title: Integration
parent: Android
nav_order: 1
---

# Integration
{: .no_toc}

To add the CarTrawlerSDK to your app, add our maven repository and enter your artifactory credentials.

<details open markdown="block">
  <summary>
    Table of Contents
  </summary>
  {: .text-delta }
1. TOC
{:toc}
</details>

---

## Maven & Artifactory Repository

### Android Gradle Plugin (AGP) before 7.0

You should add the following repository into you root <b>build.gradle</b> file:

```groovy
repositories {
    maven {
        url "http://artifactory.cartrawler.com/artifactory/libs-release-local"
        credentials { 
            username = "your_username_here" 
            password = "your_password_here" 
        }
    }
}
```

### Android Gradle Plugin (AGP) 7+

You should add the following repository into you <b>settings.gradle</b> file:

```groovy
dependencyResolutionManagement {
    repositoriesMode.set(RepositoriesMode.FAIL_ON_PROJECT_REPOS)
    repositories {
        maven {
            url "http://artifactory.cartrawler.com/artifactory/libs-release-local"
            credentials {
                username = "your_username_here"
                password = "your_password_here"
            }
            allowInsecureProtocol = true
        }
    }
}
```

---

## Getting Credentials

If you do not have these credentials contact your manager, IT, or another developer in your team.

---

## Adding the Dependency to your App
Add the CarTrawler dependency to build.gradle. Please use the version number sent to you by CarTrawler.

```groovy     
implementation "com.cartrawler.android:car-rental:$latestVersion" 
```

{: .note}
We use semantic versioning to define the SDK version number, so the version number will always follow the pattern major.minor.patch, eg: 1.0.0

Create a theme that extends the ```CTDayNightTheme```. Please refer to the <a href="/docs/android/customisation/themes" target="_blank">theme section</a> for further details.

---

## App Permissions

On the SDK’s Search Screen, we have added the option for users to search for vehicles using their current location upon tapping the pick-up location EditText.

To make use of this feature, please ensure your Manifest has either the `ACCESS_COARSE_LOCATION` or `ACCESS_FINE_LOCATION` permission.

We will need to use this when asking for permission to use location services within your app if it has not already been granted.