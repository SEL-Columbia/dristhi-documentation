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

## How forms are added to Dristhi App
1. Forms are authored according to XLSForm standard (using Microsoft Excel)
2. These forms are then uploaded to http://formhub.org/ which converts them into XForm standard. All Dristhi forms can be found at http://formhub.org/drishti_forms/ (this requires login)
3. XForm is then converted to Enketo HTML form by running the following command with the <form_id> replaced with the actual value, ``curl -X POST -ssl3 -d "server_url=https://formhub.org/drishti_forms&form_id=<form_id>" https://enketo.formhub.org/transform/get_html_form``.
4. This generates a xml document which contains two nodes, *model* and *form*, rest of the nodes can be ignored. *model* is the XForm Model instance, this is used by Enketo to do all the validations and calculations in the forms. *form* is the HTML DOM which Enketo uses to render the form
5. *model* and *form* are saved as *model.xml* and *form.xml*. These files are then copied to the location <dristhi_app_root>/dristhi-app/assets/www/form/<form_name>/model.xml and <dristhi_app_root>/dristhi-app/assets/www/form/<form_name>/form.xml. When a form is launched, form_name is passed in the query parameter. Using this Enketo renders the form
6. Another important part of form is form_definition.json file. This is a mapping file that is written for every form which maps the fields within the form to Dristh database. More about form_definition.json later
7. So *model.xml*, *form.xml* and *form_definition.json* comprise a Enketo Form

## How to update forms on Dristhi app
1. Find the id of the form that needs to be updated. This can be done by locating the form at http://formhub.org/drishti_forms/
2. Find the name of the form on Dristhi app. This can be done by locating the form at <dristhi_app_root>/dristhi-app/assets/www/form directory
3. Run the following command by replacing <form_id> with the actual form_id, ``curl -X POST -ssl3 -d "server_url=https://formhub.org/drishti_forms&form_id=<form_id>" https://enketo.formhub.org/transform/get_html_form``
4. Copy the *model* node contents to <dristhi_app_root>/dristhi-app/assets/www/form/<form_name>/model.xml and the *form* node contents to <dristhi_app_root>/dristhi-app/assets/www/form/<form_name>/form.xml
5. Update the form_definition.json according to the changes made to *model*

## Structure of form_definition.json
An example form_definition.json is below:
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
This defines the entity for which the form is being filled. As of now, the possible values for this are eligible_couple, mother or child.  
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

[1]: http://opendatakit.org/help/form-design/xlsform/
[2]: https://en.wikipedia.org/wiki/XForms
[3]: https://enketo.org/
[4]: https://github.com/MartijnR/enketo-dristhi
