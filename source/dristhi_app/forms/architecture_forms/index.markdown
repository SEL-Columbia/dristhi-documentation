---
layout: page
title: "Dristhi Form Architecture"
---

## Dristhi Form Architecture

![][forms_diagram]
[Sequence Diagram for Dristhi Forms][1]

The above mentioned components can be found in code as below:

* Form Activity is one of FormActivity or MicroFormActivity based on whether the form is opened in full screen mode or in micro form mode
* Enketo Form Template refers to the template.html of Enketo Dristhi. In Dristhi app, this can be found at <app_root>/dristhi-app/assets/www/enketo/template.html
* Dristhi Android Context is implemented as FormWebInterface. This object is injected into Enketo as a Javascript object using [Android Javascript Interface API][2]
* Ziggy Form Data Controller is the object in Ziggy that is implemented as ziggy/FormDataController. This can be found in Ziggy at <ziggy_root>/ziggy/src/FormDataController

More information about the sequence diagram messages below:

1. When a form is to be launched, Form Activity is used to open the form. Form name, Entity ID and meta data (field overrides, reminder ID, etc) are passed to Form Activity via the [Intent][3]
2. Form Activity then uses webView to load Enketo-Dristhi's template.html and passes to it the intent parameters mentioned above. It also injects Dristhi Android Context as a Javascript object. template.html is the skeleton of a Enketo form
3. Enketo then gets the XForm Model from Dristhi Android Context by calling getModel method. Dristhi Android Context would then return the XForm Model which is the contents of file at <dristhi_app_root>/dristhi-app/assets/www/form/<form_name>/model.xml
4. Enketo then gets the HTML Form DOM from Dristhi Android Context by calling getForm method. Dristhi Android Context would then return the DOM which is the contents of file at <dristhi_app_root>/dristhi-app/assets/www/form/<form_name>/form.xml
5. Enketo then calls Ziggy's Form Data Controller to get the form model. Form model will be similar in structue to form_definition, but with the values per-populated. Enketo passes the query params to Ziggy and Ziggy uses these to load the data from Dristhi App SQLite database and returns the form model to Enketo
6. Enketo uses XForm Model, HTML Form DOM and form model to render and pre-populate the form


[1]: {{root_url}}/images/custom/dristhi_app/forms.png
[2]: https://developer.android.com/guide/webapps/webview.html#UsingJavaScript
[3]: https://developer.android.com/reference/android/content/Intent.html
[forms_diagram]: {{root_url}}/images/custom/dristhi_app/forms.png "Dristhi Forms Sequence Diagram"