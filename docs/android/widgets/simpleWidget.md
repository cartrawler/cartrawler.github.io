---
layout: default
title: Simple 
parent: Android SDK Widgets
grand_parent: Android
nav_order: 1
permalink: /docs/android/widgets/simple/
---

# Simple Widget

{: .no_toc }

---

![](/uploads/Simple_Loaded_Generic_style.png)

You can add a click listener to this `CTSimpleWidget` that will start the In Path or standalone flow, using the standard `View.OnClickListener`

Following the completion of the In Path flow, you may show a rental car has been added by displaying a CTSimpleAddedWidget or CTVehicleWidget instead when `onActivityResult` is called.