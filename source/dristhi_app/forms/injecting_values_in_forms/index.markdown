---
layout: page
title: "Injecting values in forms while loading"
---

# Injecting values to the form fields when the form loads

In certain cases of Dristhi App, we need the form to be loaded with few of the options pre-selected while the form opens. A child immunization form is a very good example of this. When the user taps on a particular immunization button, the child immunization form should open with the corresponding option button for that immunization pre-selected while the form opens.

This can be achieved by opening the form with the field overrides. 

A form can be opened without any of the form overrides as below:
```
openForm('form_name', client.entity_id)
```
But the same form can be opened with the overrides as below:
```
openFormWithFormOverrides('form_name', client.entity_id, '{"field_name_1":"value1", "field_name_2":"value2"}')
```
This would pre-populate "value1" and "value2" on fields "field_name_1" and "field_name_2" respectively.