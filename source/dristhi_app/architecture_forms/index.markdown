---
layout: page
title: "Dristhi Forms"
---

# Dristhi Forms

## Tools and Standards used
* [XLSForm][1]: All the forms are authored as per XLSForm standard using Microsoft Excel
* [XForm][2]: XLSForms are then uploaded to http://formhub.org/
* [Enketo][3]
* [Enketo-Dristhi][4]

## How to add new forms to Dristhi App
1. Forms are authored according to XLSForm standard (using Microsoft Excel)
2. These forms are then uploaded to http://formhub.org/ which converts them into XForm standard. All Dristhi forms can be found at http://formhub.org/drishti_forms/ (this requires login)
3. XForm is then converted to Enketo HTML form by running the following command with the <form_id> replaced with the actual value, ``curl -X POST -ssl3 -d "server_url=https://formhub.org/drishti_forms&form_id=<form_id>" https://enketo.formhub.org/transform/get_html_form``.
4. This generates a xml document which contains two nodes, *model* and *form*, rest of the nodes can be ignored. *model* is the XForm Model instance, this is used by Enketo to do all the validations and calculations in the forms. *form* is the HTML DOM which Enketo uses to render the form
5. *model* and *form* are saved as *model.xml* and *form.xml*. These files are then copied to the location <dristhi_app_root>/dristhi-app/assets/www/form/<form_name>/model.xml and <dristhi_app_root>/dristhi-app/assets/www/form/<form_name>/form.xml. When a form is launched, form_name is passed in the query parameter. Using this Enketo renders the form
6. Another important part of form is form_definition.json file. This is a mapping file that is written for every form which maps the fields within the form to Dristhi database. More about form_definition.json later
7. So *model.xml*, *form.xml* and *form_definition.json* comprise a Enketo Form

## How to update forms on Dristhi app
1. Find the id of the form that needs to be updated. This can be done by locating the form at http://formhub.org/drishti_forms/
2. Find the name of the form on Dristhi app. This can be done by locating the form at <dristhi_app_root>/dristhi-app/assets/www/form directory
3. Run the following command by replacing <form_id> with the actual form_id, ``curl -X POST -ssl3 -d "server_url=https://formhub.org/drishti_forms&form_id=<form_id>" https://enketo.formhub.org/transform/get_html_form``
4. Copy the *model* node contents to <dristhi_app_root>/dristhi-app/assets/www/form/<form_name>/model.xml and the *form* node contents to <dristhi_app_root>/dristhi-app/assets/www/form/<form_name>/form.xml
5. Update the form_definition.json according to the changes made to *model*

## form_definition.json
This is a mapping file that is written for every form which maps the fields within the form to Dristhi database. An example form_definition.json is below:
``` json
{
    "form_data_definition_version": "1",
    "form": {
        "bind_type": "entity",
        "default_bind_path": "/model/instance/form_id/",
        "fields": [
            {
                "name": "field1",
                "source": "entity.value1",
                "bind": "/model/instance/form_id/field1",
                "shouldLoadValue": true,
                "value": "value1"
            },
            {
                "name": "field2",
                "source": "entity.value2",
                "bind": "/model/instance/form_id/field2",
                "value": "value2"
            }
        ]
    }
}
```
Explanation for the same below:  
**form_data_definition_version**  
This defines the version of the form definition. Every time the form definition changes, this version must be incremented. This will be useful when handling the form.  
**bind_type**  
This defines the entity for which the form is being filled. As of now, the possible values for this are *eligible_couple, mother or child*.  
**default_bind_path**  
This is the default value for bind path in case it is not specified in the field.  
**fields**  
This has all the fields that we have to map between Dristht DB and Forms.  
**name**  
This is the name of the field.  
**source**  
This specifies the mapping path for a field in Dristhi DB. For example, if the value for this is entity.value1 then the field value is loaded from or saved onto value1 field of entity when the entity is saved or updated. If no value is specified for this, the value is inferred as [bind_type].[name]  
**bind**  
This specifies the mapping path for a field in model.xml (Enketo form field). For example, if the value for this is /model/instance/form_id/field1 then control that corresponds to this XPath will be pre-populated with the corresponding value. If no value is specified for this, the value is inferred as [default_bind_path]/[name]  
**shouldLoadValue**  
This field tells Ziggy that the field should be pre-populated with the corresponding value.  
**value**  
This is the value for the field. When the form opens if *value* is specified then Enketo pre-populated the field with the specified on the form.  

## entity_relationship.json
Another important component of Dristhi forms is entity_relationship.json. There is one entity_relationship.json per application (in this case Dristhi). As the name suggests, it defines the relationship between the various entities of Dristhi app. The entities that are part of the app as of now are *eligible_couple, mother and child*. Dristhi's entity_relationship.json is below:
```json
[
    {
        "parent": "eligible_couple",
        "child": "mother",
        "field": "wife",
        "kind": "one_to_one",
        "from": "eligible_couple.id",
        "to": "mother.ecCaseId"
    },
    {
        "parent": "mother",
        "child": "child",
        "field": "children",
        "kind": "one_to_many",
        "from": "mother.id",
        "to": "child.motherCaseId"
    }
]
```
Each entry in list defines a relation. We have two relationships in Dristhi, one_to_one between eligible_couple and mother and one_to_many between mother and child.
Explanation for the fields below:  
**parent**  
This specifies the parent entity in the given relation.  
**child**  
This specifies the child entity in the given relation.  
**field**  
This specifies the field on parent entity which allows us to load child entity for that parent. This was added to support the cases where entities are stored on Document based Databases like CouchDB. This is currently not used or implemented in the app.  
**kind**  
This specifies the type of relationship. Currently supported valyes are *one_to_one* and *one_to_many*.  
**from** and **to**  
Together these two values specify how to load parent and child entities from a relational database.  

## Dristhi Form Architecture

![][forms_diagram]
[Sequence Diagram for Dristhi Forms][5]

The above mentioned components can be found in code as below:

* Form Activity is one of FormActivity or MicroFormActivity based on whether the form is opened in full screen mode or in micro form mode
* Enketo Form Template refers to the template.html of Enketo Dristhi. In Dristhi app, this can be found at <app_root>/dristhi-app/assets/www/enketo/template.html
* Dristhi Android Context is implemented as FormWebInterface. This object is injected into Enketo as a Javascript object using [Android Javascript Interface API][6]
* Ziggy Form Data Controller is the object in Ziggy that is implemented as ziggy/FormDataController. This can be found in Ziggy at <ziggy_root>/ziggy/src/FormDataController

More information about the sequence diagram messages below:

1. When a form is to be launched, Form Activity is used to open the form. Form name, Entity ID and meta data (field overrides, reminder ID, etc) are passed to Form Activity via the [Intent][7]
2. Form Activity then uses webView to load Enketo-Dristhi's template.html and passes to it the intent parameters mentioned above. It also injects Dristhi Android Context as a Javascript object. template.html is the skeleton of a Enketo form
3. Enketo then gets the XForm Model from Dristhi Android Context by calling getModel method. Dristhi Android Context would then return the XForm Model which is the contents of file at <dristhi_app_root>/dristhi-app/assets/www/form/<form_name>/model.xml
4. Enketo then gets the HTML Form DOM from Dristhi Android Context by calling getForm method. Dristhi Android Context would then return the DOM which is the contents of file at <dristhi_app_root>/dristhi-app/assets/www/form/<form_name>/form.xml
5. Enketo then calls Ziggy's Form Data Controller to get the form model. Form model will be similar in structue to form_definition, but with the values per-populated. Enketo passes the query params to Ziggy and Ziggy uses these to load the data from Dristhi App SQLite database and returns the form model to Enketo
6. Enketo uses XForm Model, HTML Form DOM and form model to render and pre-populate the form


[1]: http://opendatakit.org/help/form-design/xlsform/
[2]: https://en.wikipedia.org/wiki/XForms
[3]: https://enketo.org/
[4]: https://github.com/MartijnR/enketo-dristhi
[5]: {{root_url}}/images/custom/dristhi_app/forms.png
[6]: https://developer.android.com/guide/webapps/webview.html#UsingJavaScript
[7]: https://developer.android.com/reference/android/content/Intent.html
[forms_diagram]: {{root_url}}/images/custom/dristhi_app/forms.png "Dristhi Forms Sequence Diagram"