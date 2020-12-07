---
title: Installation
position: 1
type: Android
description: Supports Android 5 Lollipop (SDK version 21) and above
right_code: >-
  
---

**Add our maven repository and enter the artifactory credentials**

  ~~~groovy
       // Add to root build.gradle
       repositories {
          maven {
              url "http://artifactory.cartrawler.com/artifactory/libs-release-local"
              credentials { 
              username = "your_username_here" 
              password = "your_password_here" 
              }
          }
       }
       
       // CarTrawler dependency (Add to module build.gradle)
                   
       // Please use the version number sent to you by the CarTrawler team
       implementation "com.cartrawler.android:car-rental:$latestVersion" 
  ~~~

**Create a theme that extends the ```CTDayNightTheme```. Please refer to <a href="https://cartrawler.github.io/#section_style_guidetheming" target="_blank">theme section</a> for further details.**

**Clear Storage**

Note: The apps storage (database of recent searches, and bookings) can be cleared prior to starting the cartrawler flow, the following API can be used to clear the storage.

This is a static method that can be called directly from the CartrawlerSDK class

  ~~~kotlin
       CartrawlerSDK.clearStorage(activity)
  ~~~

        
       
