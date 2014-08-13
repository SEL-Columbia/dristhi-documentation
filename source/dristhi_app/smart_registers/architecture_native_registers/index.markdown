---
layout: page
title: "Smart Register Architecture (Native)"
---

# Smart Register Architecture (Native)

The EC Register is built using Android Native model. Eventually all the registers can be moved from the hybrid model to the native screens to provide better performance. A native register can be built with architecture as below. The architecture below is written with EC Register as example because as of now that is the only register that is in native model. But this can be extended to other existing and new registers

Sound knowledge of below mentioned tools and techniques are necessary to understand how these registers are developed:

* [Android Native Code](http://developer.android.com)

The architecture of the Native Register can be described as below:

* Register Activity is implemeted as [NativeECSmartRegisterActivity](https://github.com/SEL-Columbia/dristhi-app/blob/master/dristhi-app/src/main/java/org/ei/drishti/view/activity/NativeECSmartRegisterActivity.java) class. This is the Android activity that extends from [SecuredNativeSmartRegisterActivity](https://github.com/SEL-Columbia/dristhi-app/blob/master/dristhi-app/src/main/java/org/ei/drishti/view/activity/SecuredNativeSmartRegisterActivity.java)
* The various layouts required for the register to be displayed is defined in [res/layouts directory](https://github.com/SEL-Columbia/dristhi-app/blob/master/dristhi-app/res/layout/smart_register_client_gplsa_layout.xml) as xml files
* The class SecuredNativeSmartRegisterActivity has interfaces defined for the following
	* The various service modes for the register
	* The various filters that can be applied on the register
	* The sort options
	* The weights for the columns
* The total weight of the register is defined in the layout xml and the list of weights that is define how much width each column should take on the screen. For example if the weight of the whole register is mentioned to be 100, and the list of weights are defined as 50, 15, 10, 5 and 20, this means that the screen has 5 columns and the width of the columns are 50%, 15%, 10% 5% and 20% respectively
* The Activity, on initialization, instantiates the corresponding SmartRegistriesController (which is ECSmartRegistriesController in case of EC Register) and the VillageController
* The list of service options are overridden on the getEditOption method where the menu item name, the corresponding form to open are returned

The following are the sequence of events that happens when a native register is open:

1. When a native smart register is opened, the corresponding Register Activity is started (NativeECSmartRegisterActivity in case of EC Register)
2. The activity instantiates the Smart Register Controller
3. The UI is loaded from the layout.xml file
4. Register Controller loads the client list by calling the getClients method of SmartRegistriesClientProvider
5. Register controller pre-processes the client list before rendering it. Pre-process step includes converting date fields to DD/MM/YYYY format; title casing text fields like Wife Name, Village, etc; parsing reminders and service provided information, etc
6. Then the data is bound to the UI elements 

