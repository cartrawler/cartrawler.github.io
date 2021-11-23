---
layout: default
title: Home
nav_order: 1
description: "CarTrawler Native SDK Documentation"
permalink: /
---

# CarTrawler Native
<!-- {: .fs-9 } -->

Welcome to the CarTrawler iOS and Android Native SDK documentation.
{: .fs-6 .fw-300 }

Here you'll find everything you need to integrate the SDKs and start accepting car rental bookings in your mobile apps.

[Get Started iOS](/docs/ios){: .btn .btn-primary .btn-blue .fs-5 .mb-4 .mb-md-0 .mr-2 } [Get Started Android](/docs/android){: .btn .btn-primary .btn-green .fs-5 .mb-4 .mb-md-0 .mr-2 } [Style Guide](/docs/style-guide){: .btn .btn-primary .btn-purple .fs-5 .mb-4 .mb-md-0 .mr-2 }

---

## Important Information

### Definitions

Car rental product
: Comprised of the physical car rental itself, and optional add-on product line items such as insurance and car extras (additional driver, GPS, child/toddler seat, etc).

Flow
: a set of CarTrawler UI screens, with a specific business objective. 

### Flows ###
There are two main business flows implemented in the SDK, <a href="/docs/style-guide/user-flow#standalone-flow">Standalone</a> and <a href="/docs/style-guide/user-flow#in-path-flow">In Path</a><br/>.
Note: the In Path rental solution requires **additional backend development effort** and organisation between you and CarTrawler.

Standalone
: Allows the user to reserve a car rental product independently of a flight.

Standalone deeplink
: a variant of the Standalone flow whereby the user is presented with the vehicle list based on the pick-up and drop-off parameters passed to the SDK, thus bypassing the initial search screen.

In Path
: Allows the user to reserve a car rental product to accompany their flight, this product forms part of the user's flight itinerary. 

## Prerequisites ##

Please ensure that you have received the following from a CarTrawler representative:
{: .present-before-paste}

### For Integration: ###
{: .present-before-paste}

* Relevant Client ID(s) to use with SDK (these will be provided by your Partner Manager)
* CarTrawler repository credentials
* Artifactory credentials (Android)
* Dedicated Slack support channel for integrating the SDK into your apps. 

### For Go-Live: ###

Prior to go live we advise that you link back in with CarTrawler to test and verify that bookings are pointed to our production environment.