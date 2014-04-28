---
layout: page
title: "Smart Register Architecture (Hybrid)"
---

# Smart Register Architecture (Hybrid)

FP, ANC, PNC and Child Register are built using Android Hybrid model and hence all of them have the same architecture as below. The architecture below is written with ANC Register as example, but the same applies to the other hybrid registers too.  

Sound knowledge of below mentioned tools and techniques are necessary to understand how these registers are developed:

* [AngularJS][5]
* [Android Javascript Interface API][6]
* [SQLCipher for Android][7] and [SQLite Database][8]

![][smart_register_hybrid_diagram]
[Sequence Diagram for Smart Register (Hybrid)][1]

The above mentioned components can be found in code as below:

* Register Activity is implemeted as ANCSmartRegisterActivity class. This is an Android Activity which has a webView control
* Register HTML is implemeted as [anc_register.html][2]. This is implemented as an AngularJS application
* Register Controller is implemented as [anc_register_app.js][3]. Additionally all the common functionality across the registers, like search, sort, etc, are implemented in a common controller called [list_view_controller][9]. list_view_controller is included in all the smart registers
* Android Context is implemented as ANCSmartRegisterController. When Register Activity loads the html into webView, it injects this Java object as a Javscript object using Android Javascript Interface API. Register HTML then uses this object to load the data from the local SQLite database
* Register Service is implemented as [anc_service.js][4]

More information about the sequence diagram messages below:

1. When a smart register is opened Register Activity is started
2. Register Actvity uses webView to load Register HTML
3. This initialises AngularJS application and controllers (Register Controller and list view controller)
4. Register Controller loads the client list
   1. It calls Get Clients on Android Context
   2. Android Context loads the data from SQLIte Database and returns the data in JSON format. This JSON is cached using [Cache][10] object to reduce the load time of the register
5. Register controller pre-processes the client list before rendering it. Pre-process step includes converting date fields to DD/MM/YYYY format; title casing text fields like Wife Name, Village, etc; parsing reminders and service provided information, etc
6. Register HTML then renders the client list


[1]: {{root_url}}/images/custom/dristhi_app/smart_register_hybrid.png
[2]: https://github.com/SEL-Columbia/dristhi-app/blob/master/dristhi-app/assets/www/smart_registry/anc_register.html
[3]: https://github.com/SEL-Columbia/dristhi-app/blob/master/dristhi-app/assets/js/smart_registry/anc_register_app.js
[4]: https://github.com/SEL-Columbia/dristhi-app/blob/master/dristhi-app/assets/js/smart_registry/anc_service.js
[5]: https://angularjs.org/
[6]: https://developer.android.com/guide/webapps/webview.html#UsingJavaScript
[7]: http://sqlcipher.net/sqlcipher-for-android/
[8]: https://sqlite.org/
[9]: https://github.com/SEL-Columbia/dristhi-app/blob/master/dristhi-app/assets/js/smart_registry/list_view_controller.js
[10]: https://github.com/SEL-Columbia/dristhi-app/blob/master/dristhi-app/src/main/java/org/ei/drishti/util/Cache.java

[smart_register_hybrid_diagram]: {{root_url}}/images/custom/dristhi_app/smart_register_hybrid.png "Smart Register (Hybrid) Sequence Diagram"
