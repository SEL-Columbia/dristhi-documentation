---
layout: page
title: "form_definition.json"
---

# form_definition.json
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
