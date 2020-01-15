---
title: Installation
position: 1
type: Android
description: Supports Android 4.4 KitKat (SDK version 19) and above
right_code: >-
  ~~~ java
       repositories {
          maven {
              url "http://artifactory.cartrawler.com/artifactory/libs-release-local"
              credentials { 
              username = "your_username_here" 
              password = "your_password_here" 
              }
          }
       }
  ~~~

  {: title="Gradle (Root build.gradle) }

  ~~~ java 
  
   # rxjava
   -keep class rx.schedulers.Schedulers {
     public static <methods>;
   }
   -keep class rx.schedulers.ImmediateScheduler {
      public <methods>;
   }
   -keep class rx.schedulers.TestScheduler {
      public <methods>;
   }
   -keep class rx.schedulers.Schedulers {
      public static ** test();
   }
   -keepclassmembers class rx.internal.util.unsafe.*ArrayQueue*Field* {
      long producerIndex;
      long consumerIndex;
   }
   -keepclassmembers class rx.internal.util.unsafe.BaseLinkedQueueProducerNodeRef {
      long producerNode;
      long consumerNode;
   }
   
  ~~~

  {: title="Proguard" }
  
---

1. Add our maven repository and enter the artifactory credentials (see Gradle tab on the right).
2. Create a theme that extends the **CTAppTheme**. Please refer to <a href="https://cartrawler.github.io/#section_style_guidetheming" target="_blank">theme section</a> for further details.
3. If you are using proguard, update the proguard config as shown on the right.

**Clear Storage**

Note: The apps storage (database of recent searches, and bookings) can be cleared prior to starting the cartrawler flow, the following API can be used to clear the storage.
        
        fun clearStorage(context: Context) // Pass an Activity context
        
This is a static method that can be called directly from the CartrawlerSDK class, e.g

        CartrawlerSDK.clearStorage(activity)
       
