---
layout: default
title: Home
nav_order: 1
description: "CarTrawler Native SDK Documentation"
permalink: /
---

# CarTrawler Car Rental SDK
{: .no_toc}

Welcome to the CarTrawler iOS and Android Native SDK documentation.
{: .fs-6 .fw-300 }

Here you'll find everything you need to integrate the SDKs and start accepting car rental bookings in your mobile apps.

<details markdown="block">
  <summary>
    Table of Contents
  </summary>
  {: .text-delta }
- TOC
{:toc}
</details>

[Get Started iOS](/docs/ios){: .btn .btn-primary .btn-blue .fs-5 .mb-4 .mb-md-0 .mr-2 } [Get Started Android](/docs/android){: .btn .btn-primary .btn-green .fs-5 .mb-4 .mb-md-0 .mr-2 } [Style Guide](/docs/style-guide){: .btn .btn-primary .btn-purple .fs-5 .mb-4 .mb-md-0 .mr-2 } [Migrate to 14.0.0 iOS](/docs/ios/migrate){: .btn .btn-primary .btn-blue .fs-5 .mb-4 .mb-md-0 .mr-2 } [Migrate to 14.0.0 Android](/docs/android/migrate){: .btn .btn-primary .btn-green .fs-5 .mb-4 .mb-md-0 .mr-2 }

---

## Flows 
A Flow is a set of CarTrawler UI screens, with a specific business objective. <br/>
There are two main business flows implemented in the SDK, <a href="/docs/user-flow#standalone-flow">Standalone</a> and <a href="/docs/user-flow#in-path-flow">In Path</a>.<br/> 

### Standalone 
{: .no_toc}
Allows the user to reserve a car rental product independently of a flight.

### In Path
{: .no_toc}
Allows the user to reserve a car rental product to accompany their flight. This product forms part of the user's flight itinerary. 

{: .note}
The In Path rental solution requires **additional backend development effort** and coordination between you and CarTrawler.

---

## Prerequisites 

Please ensure that you have received the following from a CarTrawler representative:

* Relevant Client ID(s) to use with the SDK (these will be provided by your Partner Manager)

{: .important}
You <b>MUST</b> use the correct clientID with our SDK in order for everything to work as expected, such as brand specific configurations. 

* Artifactory credentials (Android)
* Dedicated Slack support channel for integrating the SDK into your apps. 

{: .note}
Prior to go live we advise that you link back in with CarTrawler to test and verify that bookings are pointed to our production environment.